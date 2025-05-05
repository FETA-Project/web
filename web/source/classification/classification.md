---
layout: default
title: V3. Classification
permalink: /clasification
has_toc: true
nav_order: 4
has_children: true
has_toc: false
---

# Introduction

This set of software modules offers production‑ready classifiers for encrypted traffic based on machine‑learning techniques. The aim is to significantly enhance the capability to classify encrypted network traffic, which is crucial for identifying security threats and improving network situational awareness. The application of modern machine‑learning technologies introduces new perspectives and effective tools for protecting information systems against complex cyber threats. In this way, the proposed software solution can strengthen organizations’ ability to detect and respond to security incidents in real time.

This document describes the achievement of a planned deliverable of the **FETA** project (ID VJ02010024), specifically **VJ02010024‑V3: A Set of Classification Modules for Security Threat Detection** (hereafter referred to as *V3*). The aim of this software solution is to develop production‑ready classifiers for encrypted traffic using machine learning. Machine‑learning technologies have the potential to significantly improve the classification of encrypted network traffic, which is essential for identifying security threats and ensuring situational awareness within computer networks.

During the first year of the project implementation phase, in collaboration with the application guarantor, relevant types of security threats and suitable use cases for network‑traffic classification were identified to effectively target the detection capabilities of *V3*. Intensive research conducted and validated across all consortium members led to the development of the first module prototypes, verifying the concept of the proposed threat‑classification approach. Thanks to innovative techniques—such as the use of weak indicators—high classification accuracy was achieved, often significantly surpassing previously published methods. As a result, numerous related academic papers were published in prestigious journals [2](#ref2), [3](#ref3), [7](#ref7), [14](#ref14) and presented at renowned international conferences [1](#ref1), [4](#ref4), [5](#ref5), [6](#ref6), [8](#ref8), [12](#ref12), [13](#ref13).

Based on the research results and in consultation with the application guarantor, the classification‑module set for detecting security threats was specified to consist of the following five components:

* SSH Connection Classifier
* TLS Service Classification
* QUIC Service Classification
* Network Tunnel Detection
* Cryptocurrency‑Mining Detection

In the second year of the project, selected modules were further refined in terms of classification accuracy, processing speed, and code sustainability. For effective stream processing of network data, the **NEMEA** system [15](#ref15) was utilized, enabling the easy integration of new classifiers both within the development team and by external users such as the application guarantor. Additionally, a reusable framework of functions and methods was developed, which will facilitate the future development of new machine‑learning‑based classification algorithms, allowing quicker responses to emerging security threats and evolving requirements of the guarantor and other users.

The *V3* deliverable was successfully deployed and tested in the fall of 2023 in a real‑world environment—the **CESNET3** network—which connects over half a million users to the internet daily. Furthermore, a virtual‑machine image was created for testing and demonstrating the functionality of *V3*. This image includes the developed classification and detection algorithms as well as example input data to verify and showcase correct operation.

Although further development and enhancement of *V3* are planned for the remainder of the implementation phase, the current set of software modules already provides effective classification of network traffic and detection of a range of network‑security threats. The solution is even suitable for deployment in high‑speed networks, as verified within the national **CESNET3** infrastructure. This enables the achieved result to help protect critical network infrastructure and, consequently, a large number of its users.

# References

1. <span id="ref1"></span> García, S.; Bogado Garcia, J.; Hynek, K.; Vekshin, D.; Čejka, T.; Wasicek, A. *Large Scale Analysis of DoH Deployment on the Internet.* In: *Computer Security – ESORICS 2022*. Cham: Springer International Publishing, 2022, pp. 145‑165. ISSN 0302‑9743. ISBN 978‑3‑031‑17142‑0.  

2. <span id="ref2"></span> Hynek, K.; Vekshin, D.; Luxemburk, J.; Čejka, T.; Wasicek, A. *Summary of DNS Over HTTPS Abuse.* *IEEE Access*, 2022, 10, 54668‑54680. ISSN 2169‑3536.  

3. <span id="ref3"></span> Jeřábek, K.; Hynek, K.; Ryšavý, O.; Burgetová, I. *DNS Over HTTPS Detection Using Standard Flow Telemetry.* *IEEE Access*, 2023, 11, 50000‑50012. ISSN 2169‑3536.  

4. <span id="ref4"></span> Koumar, J.; Hynek, K.; Čejka, T. *Network Traffic Classification Based on Single‑Flow Time‑Series Analysis.* In: *2023 19th International Conference on Network and Service Management (CNSM).* New York: IEEE, 2023.  

5. <span id="ref5"></span> Koumar, J.; Čejka, T. *Network‑Traffic Classification Based on Periodic‑Behavior Detection.* In: *Proceedings of 2022 18th International Conference on Network and Service Management (CNSM).* New York: IEEE, 2022, pp. 359‑363. ISSN 2165‑9605. ISBN 978‑3‑903176‑51‑5.  

6. <span id="ref6"></span> Koumar, J.; Čejka, T. *Unevenly Spaced Time Series from Network Traffic.* In: *Proceedings of the 7th Network Traffic Measurement and Analysis Conference.* Piscataway: IEEE, 2023. ISBN 978‑3‑903176‑58‑4.  

7. <span id="ref7"></span> Luxemburk, J.; Čejka, T. *Fine‑grained TLS Services Classification with Reject Option.* *Computer Networks*, 2023, 220. ISSN 1389‑1286.  

8. <span id="ref8"></span> Luxemburk, J.; Hynek, K.; Čejka, T. *Encrypted‑Traffic Classification: The QUIC Case.* In: *Proceedings of the 7th Network Traffic Measurement and Analysis Conference.* Piscataway: IEEE, 2023. ISBN 978‑3‑903176‑58‑4.  

9. <span id="ref9"></span> Luxemburk, J.; Hynek, K.; Čejka, T.; Lukačovič, A.; Šiška, P. *CESNET‑QUIC22: A Large One‑Month QUIC Network‑Traffic Dataset from Backbone Lines.* *Data in Brief*, 2023. ISSN 2352‑3409.  

10. <span id="ref10"></span> Luxemburk, J.; Čejka, T. *CESNET‑TLS22: A Large Dataset for Fine‑Grained Classification of TLS Services* [DATASET], 2023. Zenodo. <https://doi.org/10.5281/zenodo.7965515>  

11. <span id="ref11"></span> Luxemburk, J.; Hynek, K.; Čejka, T.; Lukačovič, A.; Šiška, P. *CESNET‑QUIC22: A Large One‑Month QUIC Network‑Traffic Dataset from Backbone Lines* [DATASET], 2023. Zenodo. <https://doi.org/10.5281/zenodo.7963302>  

12. <span id="ref12"></span> Melcher, L.; Hynek, K.; Čejka, T. *Tunneling Through DNS‑over‑TLS Providers.* In: *Proceedings of 2022 18th International Conference on Network and Service Management (CNSM).* New York: IEEE, 2022, pp. 359‑363. ISSN 2165‑9605. ISBN 978‑3‑903176‑51‑5.  

13. <span id="ref13"></span> Plný, R.; Hynek, K.; Čejka, T. *DeCrypto: Finding Cryptocurrency Miners on ISP Networks.* In: *Secure IT Systems.* Cham: Springer, 2022, pp. 139‑158. ISSN 0302‑9743. ISBN 978‑3‑031‑22294‑8.  

14. <span id="ref14"></span> Uhříček, D.; Hynek, K.; Čejka, T.; Kolář, D. *BOTA: Explainable IoT Malware Detection in Large Networks.* *IEEE Internet of Things Journal*, 2023, 10 (10), 8416‑8431. ISSN 2327‑4662.  

15. <span id="ref15"></span> Čejka, T.; Bartoš, V.; Švepeš, M.; Rosa, Z.; Kubátová, H. *NEMEA: A Framework for Network Traffic Analysis.* In: *12th International Conference on Network and Service Management.* Montréal: IEEE, 2016, pp. 195‑201. ISSN 2165‑963X. ISBN 978‑3‑901882‑85‑2.  

16. <span id="ref16"></span> Veselý, V.; Žádník, M. *How to Detect Cryptocurrency Miners? By Traffic Forensics!* *Digital Investigation*, 2019, 31. ISSN 1742‑2876.  