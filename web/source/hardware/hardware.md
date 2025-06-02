---
layout: default
title: V1. Hardware
permalink: /hardware
has_toc: true
nav_order: 2
has_children: true
---

# Hardware

Modern digital infrastructure faces unprecedented demands for security, flexibility and throughput. Organizations operating large-scale high-speed networks, such as research institutions, backbone providers or data center operators, need solutions capable of not only capturing but also analyzing huge volumes of network traffic in real time – including encrypted traffic.

This section describes the result of **VJ02010024-V1: "Architectures and software for high-speed network traffic processing"**, the main goal of which was to create a software module (hereinafter referred to as the FETA module) that will extend the functionality of the CESNET high-speed monitoring probes so that the probes can be used for online analysis and classification of network traffic using machine learning (ML) methods, up to 400 Gb/s. The entire architecture was created for smart network cards containing FPGA technology.

When designing the new architecture, we first defined the main use cases for probes: statistics collection, incident detection using IDS Suricata, data preparation for ML classifiers, data set collection, and network traffic filtering. For each scenario, we chose an appropriate method of hardware acceleration in order to build on existing probes and expand them with a new FETA module.

Based on the analysis, we used existing parts of the NDK platform: traffic filtering, DMA transfers, and other elements with a throughput of 400 Gb/s. A new FETA module was designed, the core of which is the Connection Tracking Table (CTT) component, which allows for efficient statistics collection and the execution of specific actions over network flows. CTT ensures access control to external memory and data consistency between computing units implementing accelerations for individual scenarios. The architecture and the FETA module are described in the chapter on hardware architecture, including optimizations aimed at saving FPGA resources.

As part of the implementation of the V1 result, we focused on putting the CTT and the entire system into operation on a 400G card. The architecture was designed to support the collection of basic flow statistics and the possibility of expanding it with additional metrics according to future requirements of ML classifiers. Flow processing supports various levels of data reduction and IDS Suricata acceleration without the need for additional modules. Acceleration of the ipfixprobe tool was also implemented and this functionality was successfully verified in the CESNET3 network.

The implementation of the FETA module is complex, but easily expandable. The measurement results confirm high performance, but also significant demands on FPGA resources, which required optimization. One of the main ones was the removal of acceleration of the ML classifiers themselves - although we proposed an effective way to accelerate them, integrating them directly into the probe was not suitable. Classification is more efficient at the collector, where data from multiple probes is available and flow analysis is not tied to each packet. Further savings were brought by reducing feature extraction from the payload - because no classifier uses it yet, this part of the system is currently empty, but ready for future additions.