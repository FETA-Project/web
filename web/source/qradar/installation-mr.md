---
layout: default
title: MalwareRadar Installation
permalink: /qradar/install/malwareradar
has_toc: false
nav_order: 2
has_children: false
parent: V2. QRadar
---

# MalwareRadar Installation

> See the original repository for the latest version of this document and code: [rysavy-ondrej/feta-malware-radar](https://github.com/rysavy-ondrej/feta-malware-radar)
---

This project supports flexible deployment strategies to accommodate various operational environments and use cases. The deployment options provide the capability to execute components either on a single system or as a distributed collection of Docker containers, allowing seamless integration into different workflows.

## Deployment Modes

### Single-System Deployment
In this mode, all components of the system are deployed and executed on a single machine. Components can be configured to interact using:
- **Pipeline configurations:** Chain components together in a structured pipeline to process data.
- **Operational modes:**
  - **Online mode:** Real-time data processing where components process input streams as they arrive.
  - **Offline mode:** Batch processing of pre-recorded data.
- **Communication method:** Components communicate through standard input/output (stdin/stdout) streams, simplifying connectivity without requiring complex inter-process communication.

This deployment mode is suitable for smaller-scale systems or environments where simplicity and low overhead are prioritized.
Deployment for multiple OSes is supported. See the corresponding folder.

### Docker-Based Deployment
For more scalable and modular deployment, components can be packaged as Docker containers. In this mode:
- Components communicate using **gRPC**, a high-performance, language-agnostic RPC (Remote Procedure Call) framework, enabling robust and efficient inter-container communication.
- Docker-based deployment provides:
  - **Horizontal scalability:** Components can be distributed across multiple systems or scaled independently based on workload requirements.
  - **Isolation:** Each component operates within its containerized environment, reducing dependency conflicts and improving maintainability.
  - **Portability:** The system can be deployed consistently across different environments, including development, testing, and production.

This mode is ideal for larger systems or scenarios requiring distributed and scalable architectures.

## Folder Structure

### [Windows](Windows/Readme.md)
The `Windows` folder contains resources and scripts for single-system deployment on Windows OS. It includes:
- Executables and scripts for individual components.
- Configuration files for pipeline setup.
- Documentation on how to execute components in both online and offline modes using stdin/stdout.

### [Docker](Docker/Readme.md)
The `Docker` folder provides everything needed for Docker-based deployment, including:
- Dockerfiles for building containerized versions of each component.
- `docker-compose.yml` files for managing multi-container deployments.
- Configuration and environment files tailored for gRPC communication and container networking.
- Instructions for setting up, running, and managing the Dockerized system.

## Choosing a Deployment Mode
The choice of deployment mode depends on the specific use case:
- Use **Single-System Deployment** for simpler setups, local testing, or scenarios with limited resources.
- Opt for **Docker-Based Deployment** for production environments, distributed systems, or when scalability and portability are key considerations.

Refer to the respective sub-folder documentation for detailed instructions on setting up and managing each deployment mode.


## Build the Project

To deploy for the target environment use provided script that builds the project and fetches the required 3rd party tools:

* [build-windows.ps1](build-windows.ps1) script automates the process of building multiple .NET projects, packaging their outputs, and preparing them for deployment alongside a third-party tool (FluentBit).
* [build-linux.ps1](build-linux.ps1)
* [build-docker.ps1](build-docker.ps1) script automates the process of building a Docker image, tagging it, and pushing it to Docker Hub. The image is tagged and pushed to the rysavyondrej/fetanol repository on Docker Hub. The docker image is prepared for the deployment as decribed in [Docker Deploy](Docker/Readme.md).


# Single-System Deployment

This deployment mode involves running all components of the system on a single machine. It is ideal for smaller-scale systems or environments that prioritize simplicity and low overhead.

The deployment leverages the Fluent-Bit tool to integrate the pipeline with input monitoring tools using Fluent-Bit’s TCP listener input plugin and to send data to SIEM or log collection systems via Fluent-Bit’s output plugins.

Key Features:

* Ease of Deployment: All components run locally on a single system, simplifying configuration and management.
* Integration Capabilities: Supports integration with monitoring tools like Suricata, Flowmon, and Joy via Fluent-Bit.
* Customizable: Easily adaptable for various output systems, such as Splunk, Elasticsearch, or other SIEMs.

Prerequisites:

* A Windows or Linux system with Fluent-Bit installed. Download Fluent-Bit: Official Fluent-Bit Downloads
* PowerShell (for running scripts on Windows). Ensure that the system’s execution policy allows running scripts. Use `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned` if necessary.
* Pre-trained malware detection model file. The default model file is provided at cfg/malware-model.default.zip.

## Deployment Instructions
The deployment of Malware Radar and its integration with monitoring tools has been streamlined using prepared PowerShell scripts. These scripts automate the setup process, making it easier to configure and run the system with minimal manual effort.

To forward data from monitoring tools (e.g., Suricata, Flowmon, Joy) to Malware Radar, the Fluent-Bit tool is utilized. Fluent-Bit acts as a data pipeline component, enabling seamless communication between the monitoring tools and Malware Radar. Specifically, Fluent-Bit’s forward input/output plugins are used for efficient data forwarding.

1. Script Arguments:

* ListenPort: Specifies the TCP port on which the service listens for incoming traffic. Default: 9080.
* Source: Identifies the data source being monitored. Default: flowmon. Supported sources: flowmon, suricata, joy.
* ModelFile: Path to the pre-trained malware detection model. Default: cfg/malware-model.default.zip.

2. Execution Example:

```bash
.\Run-MalwareRadar.ps1 -ListenPort 9080 -Source suricata -ModelFile cfg/malware-model.default.zip
```

3.	The pipeline consists of the following components:
* FlowReader: Reads incoming data from the specified source.
* ContextCollector: Aggregates and enriches the data contextually.
* MalwareDetector: Analyzes the data using the specified model to detect malicious patterns.
* Communication between components occurs via stdout/stdin.

## Handling Output

Since Fluent-Bit on Windows disables the stdin input plugin, you need to employ a workaround to process the pipeline’s output.

Example Workflow:

1. Save Output to a File:

    ```bash
    .\Run-MalwareRadar.ps1 > detections.json
    ```

2. Forward Data to Splunk: Use Fluent-Bit to read the output file (detections.json) and forward it to Splunk:

    ```bash
    fluent-bit -i tail -p path=detections.json -o splunk -p host=127.0.0.1 -p port=8088
    ```

3. Alternatively, send data to Elasticsearch: You can configure Fluent-Bit to send data to other destinations such as Elasticsearch:

    ```bash
    fluent-bit -i tail -p path=detections.json -o elasticsearch -p Host=127.0.0.1 -p Port=9200
    ```

## Input Data Handling

The system expects input data to be received via a TCP socket on a specified port (default is 9080). This allows seamless integration with various monitoring tools like Suricata, Flowmon, and Joy, which can send JSON-formatted logs to the pipeline.

To feed data from Suricata, you can use the provided Run-SuricataMonitor.ps1 script. This script runs Suricata on the local machine, monitors a specified network interface, and sends the generated eve.json logs to the defined TCP port.

The following command runs Suricata to monitor a network interface with the IP address 10.0.0.1 and sends the monitoring data to localhost on port 9080:

```bash
.\Run-SuricataMonitor.ps1 -InterfaceAddress 10.0.0.1 -TargetHost localhost -TargetPort 9080
```

Parameter Details:

* `-InterfaceAddress`: Specifies the IP address of the network interface to be monitored by Suricata.
* `-TargetHost`: Defines the hostname or IP address of the machine where the data should be sent. Typically set to localhost for single-system deployments.
* `-TargetPort`: The TCP port on which the pipeline listens for incoming data. Default is 9080.

How It Works:

1. Suricata captures network traffic from the specified interface.
2. The logs are formatted as JSON (eve.json) and streamed to the TCP socket at the defined host and port.
3. The pipeline consumes the incoming data, processes it, and outputs malware detection results.
