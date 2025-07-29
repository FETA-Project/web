---
layout: default
title: Set of Classification Modules
nav_order: 3
permalink: classification/modules
parent: V3. Classification
has_children: false
---

# Set of Classification Modules

{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## SSH Classification

The SSH (Secure Shell) protocol is one of the most widely used protocols for remote console access to computers connected to a network. In addition, the protocol allows file transfers and port tunneling. Communication within the SSH protocol is encrypted and generally considered secure. However, due to misconfigured SSH servers or weak administrative passwords, SSH is a frequent target of attackers attempting to break in using brute-force attacks.

For this reason, it is appropriate to develop a classifier capable of annotating SSH connections (e.g., assigning labels such as successful/unsuccessful login), thereby helping to identify IP addresses that are attacking across the managed network.

### Input Data Requirements

The SSH classifier requires input in the form of network flows in the UniRec format. These flows must be enriched with sequences of packet lengths, their directions, timestamps, and TCP flags. Additionally, to reliably recognize SSH communication, the flows must contain the first 100 bytes of transmitted data. Finally, histograms representing the distribution of packet lengths and inter-packet gaps within the flow are also needed.

The minimal set of information fields from Table 1 required for correct functionality of the module is:

```
DST_IP, SRC_IP, BYTES, BYTES_REV, TIME_FIRST, TIME_LAST, PACKETS,
PACKETS_REV, DST_PORT, SRC_PORT, PROTOCOL, TCP_FLAGS, TCP_FLAGS_REV,
IDP_CONTENT, IDP_CONTENT_REV, PPI_PKT_DIRECTIONS, PPI_PKT_FLAGS,
PPI_PKT_LENGTHS, PPI_PKT_TIMES, D_PHISTS_IPT, D_PHISTS_SIZES,
S_PHISTS_IPT, S_PHISTS_SIZES
```

### Module Architecture

TODO
*Figure 4: Architecture of the SSH Protocol Classification Module*

The module is implemented in Python and designed for **stream processing** of network flows. As such, it minimizes intermediate storage and buffering to ensure suitability for deployment in large-scale monitoring infrastructures. The architecture of the module, shown in Figure 4, consists of five components:

- Filtering  
- MAC (Message Authentication Code) Classification  
- Authentication Detector  
- Timing Detector  
- Traffic Type Detector  

The functionality of each component is described in detail below.

#### Filtering

The filtering block selects only relevant network flows. In this case, it selects flows carrying SSH communication. At the beginning of an SSH session, the protocol transmits an unencrypted text string identifying the SSH version (e.g., `SSH-2.0-OpenSSH_9.3`), which can be used for detection. If the network flow does not contain this string, it is considered irrelevant and excluded from further classification to save computational resources.

#### MAC Classification

The SSH protocol supports a wide variety of cryptographic protection algorithms. The algorithm is selected during session setup based on preferences configured on the client and server. The selected encryption algorithm significantly impacts packet sizes and traffic patterns. Since subsequent analysis blocks rely on this, a MAC classifier was developed to accurately estimate the encryption used, based on packet sizes.

This information is passed to the other detectors, allowing them to adapt recognition thresholds and improve accuracy. The MAC classifier is implemented using machine learning. After extensive experimentation, the **Random Forest algorithm** (with 5 decision trees, each of maximum depth 42) produced the best results, correctly identifying the used cipher in approximately **90% of cases**.

#### Authentication Detector

The authentication detector recognizes communication patterns during login. Based on a detailed analysis of the SSH protocol, a set of conditions was created to identify patterns of successful authentication. These conditions work with sequences of encrypted packet sizes and directions. The classifier also uses information from the MAC classifier to adapt expected packet size thresholds.

Thanks to its deterministic nature, the authentication detector can distinguish between successful and failed logins with **100% accuracy**, provided the SSH protocol is implemented according to specification. Moreover, based on the packet lengths involved in user authentication, the detector can infer whether login occurred using a **password** or a **public key**.

#### Timing Detector

The timing detector attempts to identify login automation tools by analyzing typing speed. If the password is entered too quickly, the detector classifies it as automated input. The minimum human input delay is set to **1 second**, based on internal research and existing literature.

#### Traffic Type Detector

Based on the volume of transferred data and histograms of packet lengths and inter-packet intervals, the classifier can infer the type of SSH traffic. Currently, it can reliably distinguish (with **over 95% accuracy**) between three types of activity:

- **File download**  
- **File upload**  
- **Interactive terminal usage**

### Module Output

The classifier output consists of the original network flow data, enriched with contextual information about the SSH session. The contextual data can be grouped into four categories:

- Authentication result  
- Type of authentication used  
- Timing analysis of the login  
- SSH traffic type  

The output labels are described in **Table 2**.

*Table 2: Description of Added Fields Based on SSH Traffic Analysis*

| Category                        | (UniRec Field Name)            | Label      | Description                                                                 |
|---------------------------------|--------------------------------|------------|-----------------------------------------------------------------------------|
| **Authentication Result**       | `AUTHENTICATION_RESULT`       | `fail`     | Unsuccessful login                                                          |
|                                 |                                | `auth_ok`  | Successful login                                                            |
|                                 |                                | `unknown`  | Unable to determine                                                         |
| **Authentication Method**       | `AUTHENTICATION_METHOD`       | `key`      | Login using key                                                             |
|                                 |                                | `password` | Login using password                                                        |
|                                 |                                | `unknown`  | Unable to determine                                                         |
| **Authentication Timing**       | `AUTHENTICATION_TIMING`       | `user`     | Login by human user                                                         |
|                                 |                                | `auto`     | Login by automated tool                                                     |
|                                 |                                | `unknown`  | Unable to determine                                                         |
| **SSH Traffic Type**            | `TRAFFIC_CATEGORY`            | `upload`   | File upload (e.g., `scp` command)                                           |
|                                 |                                | `download` | File download (e.g., `scp` command)                                         |
|                                 |                                | `terminal` | Use of interactive terminal                                                 |
|                                 |                                | `other`    | Other types (e.g., port tunneling, scripted commands)                       |
|                                 |                                | `unknown`  | Unable to determine traffic type (e.g., due to failed login)                |

### Testing

The classifier was tested during deployment in the **CESNET3 network** over a 3-month period. Throughout testing, various devices were identified that did not fully conform to the SSH protocol specification. This impacted the accuracy of distinguishing successful vs. unsuccessful logins compared to laboratory datasets used during design.

Despite these findings, the classifier achieved **high accuracy** and classified flows in the CESNET3 network with an accuracy of **99.6%**.

## TLS Service Classification

The TLS protocol is one of the most important internet protocols for encrypted communication and is used, for example, to transmit HTTP/2 web traffic. Encrypted TLS communication can be divided into two main parts—initial secure channel establishment (handshake) and application data transfer. During the handshake, the client and server exchange information required to initialize encryption protocols and authenticate the server using a certificate (client authentication is less common).

In older versions of the TLS protocol, the handshake was unencrypted and could be analyzed for network monitoring and security purposes. Since TLS version 1.3, most of the handshake communication is encrypted, with the exception of the initial messages sent by the client (`ClientHello`) and the server (`ServerHello`). The `ClientHello` message, which contains crucial information for monitoring and understanding TLS communication, is also expected to become encrypted in the future through the TLS extension **Encrypted Client Hello**. This extension is already implemented in the most widely used browsers and is expected to be deployed in production in 2024.

The inability to access information exchanged during the TLS handshake will have a major negative impact on network management and protection. For example, it will no longer be possible to extract the **Server Name Indication** (SNI) field containing the domain of the destination web server. SNI is currently used in firewalls, IDS systems like Suricata, and parental control systems.

Therefore, this project investigates **alternative methods for monitoring and classifying TLS traffic** to maintain visibility into encrypted communications. Our proposed classification method is based on **processing packet sequences**, which enables us to predict the service the user is communicating with in a TLS session, effectively replacing the missing information from the encrypted `ClientHello`.

### Input Data Requirements

The TLS service classifier requires network flows in **UniRec format** as input. These flows must be enriched with:

- Sequences of packet lengths  
- Packet directions  
- Packet timestamps  
- TCP flags  
- Histograms of packet lengths and inter-packet gaps  

The **minimal set of information fields** from Table 1 required for correct module operation is:

```
TIME_FIRST, TIME_LAST, BYTES, BYTES_REV, PACKETS, PACKETS_REV,
TCP_FLAGS, TCP_FLAGS_REV, PPI_PKT_TIMES, PPI_PKT_DIRECTIONS,
PPI_PKT_LENGTHS, PPI_PKT_FLAGS, S_PHISTS_SIZES, D_PHISTS_SIZES,
S_PHISTS_IPT, D_PHISTS_IPT, FLOW_END_REASON
```

### Module Architecture

The module is implemented in **Python** and consists of the following main components:

- **DataZoo library**, which provides training datasets and data preprocessing functions  
- **Classifier definition**, which can be either a neural network or a **LightGBM** model based on decision trees  
- **Trained model weights**, either for the neural network (trained on a dataset from DataZoo) or a saved LightGBM model  

The TLS service classifier processes individual network flows. From each flow, it selects the required fields and applies preprocessing using the **DataZoo library**. Then, service prediction is performed using the classification model. The trained models are included as part of the V3 output.

The **DataZoo library** plays a key role as it provides the training datasets used to train the classification models and includes preprocessing functions, such as standardization of packet sizes and histogram normalization. Using this library ensures that input data are preprocessed the same way during both training and live deployment.

### DataZoo Library

The goal of the **DataZoo library** is to support the development of models for encrypted traffic classification. The library contains three datasets created and published within the project:  

- `CESNET-TLS22` <a href="{{ site.baseurl }}/clasification#ref7">7</a>
- `CESNET-QUIC22` <a href="{{ site.baseurl }}/clasification#ref9">9</a>
- `CESNET-TLS-Year22`  

For training the TLS service classifier, the dataset `CESNET-TLS-Year22` was used. This dataset contains traffic captured in the **CESNET3** network throughout the entire year 2022. The long capture period allows for training and optimizing classification models to reflect production traffic, which often changes significantly over time.

### Classifier Architecture

We evaluated two methods for TLS service classification: **LightGBM** and **deep neural networks**.  

- **LightGBM** is a gradient-boosted decision tree model and is considered one of the most advanced machine learning methods. It does not require designing a specific architecture but does require tuning a large number of parameters.  
- In contrast, for neural networks, **architecture design is crucial**.

The best results were achieved using a **deep neural network** with the architecture shown in **Figure 5**. This network uses **1D convolutional layers** to process packet sequence data and **fully connected layers** to handle other flow characteristics (shown as “flow statistics” in the figure), such as flow duration, packet count, or the presence of TCP flags. In the middle of the network, the features are combined into an **embedding**—a fixed-length numerical vector representing the flow. The final part of the neural network uses this embedding to predict the TLS service.

### Module Output

The classifier output consists of the original flow information enriched with a **predicted TLS service label**. The classifier can identify a total of **174 services**, with a selection of 50 shown in following listing. The full list of services and corresponding domain names is available in [<a href="{{ site.baseurl }}/clasification#ref10">10</a>].

```
google-ads, microsoft-defender, facebook-media, office365, rubiconproject,
seznam-ssp, tiktok, mcafee-gti, google-conncheck, microsoft-diagnostic,
doh, apple-location, microsoft-update, instagram, dropbox, outlook,
skype, google-fonts, teams, google-safebrowsing, seznam-email, google-www,
maps-cz, facebook-messenger, microsoft-settings, seznam-authentication,
google-services, microsoft-authentication, bing, avast, appnexus,
apple-itunes, google-authentication, seznam-media, twitter, facebook-web,
google-play, pubmatic, autodesk, snapchat, grammarly, eset-edtd,
facebook-graph, microsoft-onedrive, unity-games, youtube, microsoft-push,
apple-updates, spotify, o2tv
```

### Testing

The classifier was validated using a sample of **production traffic from the CESNET3 network**. For testing purposes, only flows related to services on which the classifier was trained were used (no unknown class detection). The classifier performed correctly with an **accuracy score of 92.5%**.

{: .note }
To use the neural network-based classifier, the collector must be equipped with a **GPU** due to the increased computational demands.


![Neural Network Architecture of the TLS and QUIC Service Classifier]({{ site.baseurl }}/assets/classification_nn.png)
*Figure 5: Neural Network Architecture of the TLS and QUIC Service Classifier*
