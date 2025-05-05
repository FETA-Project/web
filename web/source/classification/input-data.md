---
layout: default
title: Input Data
nav_order: 2
permalink: classification/input-data
parent: V3. Classification
has_children: false
---

# Input Data

The classification module set is designed for deployment on a flow collector, a standard component of typical network monitoring infrastructures (see Figure 3). The Collector Server gathers flows (metadata about traffic on monitored links), which are distributed from network probes using the IPFIX transport protocol. The "Network Flow Collector" software receives and processes IPFIX data and forwards it for further analysis by the developed modules. Data distribution within the Collector server is handled using the NEMEA (Network Measurement Analysis) system <a href="/clasification#ref15">15</a>.

![Network flow collection architecture showing the collector server receiving IPFIX data from network probes](/assets/classification_input.png)
*Figure 3: Architecture of network flow collection and processing system*

## NEMEA System and Module Interaction

NEMEA is a streaming, modular detection system for analyzing network flows. It consists of independent modules that receive, process, and forward data in real time. NEMEA modules perform various functions—from basic flow filtering to advanced detection of suspicious traffic patterns using machine learning. The V3 deliverable is implemented as a set of NEMEA modules.

The NEMEA system allows for interaction between modules by transmitting data in the UniRec record format. UniRec records are structured in a tabular format, where each row represents one IP flow. The information contained in each UniRec record is summarized in Table 1, representing all the metadata provided by the probe for each flow. The table also includes extended fields added as part of the V3 development, such as features used for tunnel detection. The developed V3 modules then consume these data records and enrich them with analytical results.

*Table 1: Description of Basic Information Fields Provided by the Network Probe*

| Field Name           | Description                                                                 |
|----------------------|-----------------------------------------------------------------------------|
| DST_IP               | Destination IP address                                                      |
| SRC_IP               | Source IP address                                                           |
| BYTES                | Bytes sent from source to destination                                       |
| BYTES_REV            | Bytes sent from destination to source                                       |
| TIME_FIRST           | Timestamp of the first packet                                               |
| TIME_LAST            | Timestamp of the last packet                                                |
| PACKETS              | Packets sent from source to destination                                     |
| PACKETS_REV          | Packets sent from destination to source                                     |
| DST_PORT             | Destination port                                                            |
| SRC_PORT             | Source port                                                                 |
| PROTOCOL             | Transport layer protocol                                                    |
| TCP_FLAGS            | Logical OR of TCP flags (source → destination)                              |
| TCP_FLAGS_REV        | Logical OR of TCP flags (destination → source)                              |
| IDP_CONTENT          | First 100 bytes of the first data packet (source → destination)             |
| IDP_CONTENT_REV      | First 100 bytes of the first data packet (destination → source)             |
| PPI_PKT_DIRECTIONS   | List of directions of the first 30 packets                                  |
| PPI_PKT_LENGTHS      | List of lengths of the first 30 packets                                     |
| PPI_PKT_TIMES        | List of timestamps of the first 30 packets                                  |
| PPI_PKT_FLAGS        | List of TCP flags in the first 30 packets                                   |
| D_PHISTS_IPT         | Histogram of inter-packet intervals (destination → source)                  |
| D_PHISTS_SIZES       | Histogram of packet sizes (destination → source)                            |
| S_PHISTS_IPT         | Histogram of inter-packet intervals (source → destination)                  |
| S_PHISTS_SIZES       | Histogram of packet sizes (source → destination)                            |
| TLS_SNI              | Domain name in TLS Server Name Indication                                   |
| TLS_JA3_FINGERPRINT  | JA3 client fingerprint                                                      |
| QUIC_SNI             | Domain name in QUIC Server Name Indication                                  |
| QUIC_UA              | User Agent in QUIC Client Hello                                             |
| OVPN_CONF_LEVEL      | Confidence level for OpenVPN tunnel detection                               |
| SSA_CONF_LEVEL       | Confidence level for TCP-over-tunnel detection                              |
| WG_CONF_LEVEL        | Confidence level for WireGuard tunnel detection                             |