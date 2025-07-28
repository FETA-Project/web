---
layout: default
title: V5. Semiautomatic datasets creation
permalink: /datasets_creation
has_toc: true
nav_order: 6
has_children: true
---

# Introduction

This document describes the development and outcome of **V5 – the Traffic Capture Infrastructure (TCI)**, a software solution designed to support the semi‑automatic creation of annotated network traffic datasets.  
The primary goal of TCI is to **simplify the process of capturing and processing network traffic samples** that can be used for training and evaluating network traffic analysis and machine learning methods.  
Beyond research, TCI can also serve network operators and security teams as a tool for **detailed traffic analysis, troubleshooting, and investigating ongoing security incidents**.

During the first year of development, the team designed and implemented a **modular system capable of capturing traffic and processing it automatically**. All operations can be performed either through an intuitive **graphical user interface** or by using **custom software tools** interacting with the system’s **programmatic API**.  
The design of both the user interface and the API focused heavily on **authentication, authorization, and activity logging**. The system provides configurable permission levels that are checked for every operation and maintains comprehensive logs to support auditing and tracking of all actions.

TCI also includes **support tools**, such as scripts for automated installation and configuration, which were used during pilot deployments on internal servers. The running instances of TCI have been tested and are continuously used by researchers to create datasets for machine learning experiments.  
Experience gained from these pilot deployments has been incorporated into further system improvements.

## Background and Related Work

The definition of requirements for such a system began even before the implementation phase, leveraging the team’s **previous research and development experience**. Knowledge gained from **manual data collection and annotation**, for example in [5](#ref5), was incorporated into the design of TCI.

The **system architecture** was successfully presented at the international student conference PESW 2022 [1](#ref1).  
Feedback from the research community helped validate the design and finalize the system requirements.

During the development phase, **stable prototype versions** were deployed in pilot environments, enabling their use for preparing datasets used in related research activities.  
Thanks to TCI, several datasets were published (e.g., [2](#ref2)) as well as research results based on experiments with TCI‑prepared datasets (e.g., [3](#ref3), [4](#ref4)).

### References

1. <span id="ref1"></span> Hejcman L.; Beneš D. *Traffic Capture Infrastructure.* In: *Proceedings of the 10th Prague Embedded Systems Workshop.* Praha: CTU, Faculty of Information Technology, 2022, pp. 68‑72. ISBN 978‑80‑01‑07015‑4.  
2. <span id="ref2"></span> Jeřábek K.; Hynek K.; Čejka T.; Ryšavý O. *Collection of datasets with DNS over HTTPS traffic.* *Data in Brief,* Vol. 42, 2022. ISSN 2352‑3409. https://doi.org/10.1016/j.dib.2022.108310  
3. <span id="ref3"></span> Luxemburk J.; Čejka T. *Fine‑grained TLS services classification with reject option.* *Computer Networks,* Vol. 220, 2023. ISSN 1389‑1286. https://doi.org/10.1016/j.comnet.2022.109467  
4. <span id="ref4"></span> Hulák M.; Čejka T. *Classification of network traffic.* In: *Proceedings of the 10th Prague Embedded Systems Workshop.* Praha: CTU, Faculty of Information Technology, 2022, pp. 52‑58. ISBN 978‑80‑01‑07015‑4.  
5. <span id="ref5"></span> Tropková Z.; Hynek K.; Čejka T. *Novel HTTPS classifier driven by packet bursts, flows, and machine learning.* In: *2021 17th International Conference on Network and Service Management (CNSM).* IEEE, 2021, pp. 345‑349. doi: 10.23919/CNSM52442.2021.9615561.  