---
layout: default
title: V4. Datasets
permalink: /datasets
has_toc: true
nav_order: 5
has_children: true
---

# Datasets

This document describes the achievement of the planned result of the project *Analysis of encrypted traffic using network flows* (code VJ02010024), specifically the output VJ02010024-V4 entitled *Datasets for training and testing detectors using machine learning techniques* (hereinafter referred to as V4). The aim of this output is to create high-quality, representative, and annotated datasets based on real network traffic, which will be used for training and testing machine learning models. Such datasets are a key prerequisite for the development of functional, reliable, and accurate detection models. The content of the result is schematically shown in the figure below and consists of three parts: 1) Datasets, 2) Software for working with data, and 3) Cataloging and analysis of datasets.

![Overview of the V4 result]({{ site.baseurl }}/assets/v4_image9.png)

The datasets were created and published throughout the implementation phase of the project based on the needs of the developed network detectors and current trends in network security. A total of 26 datasets were created and published during the project, including 4 datasets published as impacted journal publications, for example, in prestigious journals of the Nature family. These datasets are gradually being used by the professional community, which turns to us as authors and provides feedback.

Based on feedback from the international scientific community, supporting software was designed and developed to significantly facilitate working with large data files. The result is two libraries, DataZoo and TSZoo, implemented in Python—the standard in the field of data science. These libraries provide users with convenient access to datasets and at the same time automate common operations such as splitting, merging, or selecting data, which significantly streamlines the entire analysis process. In addition, automating this process increases the reproducibility of overall experiments and reduces the error rate during the evaluation of machine learning models.

The quality of the datasets was one of the important research activities that we intensively focused on during the project. The output is new analytical methods for measuring data quality. These analytical methods have been integrated into the developed tool Katoda, which is a system for making datasets accessible in the form of a catalog with metadata, i.e., analysis results and other descriptions. Katoda integrates the V4 result in a clear web application accessible to the general public and can include any datasets used by the network security community.

Thanks to the unique high-quality results, the research team has built a strong position in the professional community, primarily due to valuable datasets that have been downloaded more than 22,096 times. Although the scientific articles describing the datasets in impacted journals were published mainly in the second half of the project, at the time of preparing this document, we recorded more than 122 citations, with their further progressive increase expected in the coming years. The V4 result thus represents a significant contribution to the scientific community in the form of high-quality, widely applicable datasets, whose relevance and impact will most likely continue to grow even after the project is completed.

# References

1.  <span id="ref1"></span>García, S.; Bogado Garcia, J.; Hynek, K.; Vekshin, D.; Čejka, T.; Wasicek, A. *Large Scale Analysis of DoH Deployment on the Internet.* In: *Computer Security - ESORICS 2022*. Cham: Springer International Publishing, 2022. p. 145-165. ISSN 0302-9743. ISBN 978-3-031-17142-0.
2.  <span id="ref2"></span>Jeřábek, K.; Hynek, K.; Ryšavý, O.; Burgetová, I. *DNS Over HTTPS Detection Using Standard Flow Telemetry.* *IEEE Access*. 2023, 2023(11), 50000-50012. ISSN 2169-3536.
3.  <span id="ref3"></span>Koumar, J.; Hynek, K.; Čejka, T. *Network Traffic Classification Based on Single Flow Time Series Analysis.* In: *2023 19th International Conference on Network and Service Management (CNSM)*. New York: IEEE, 2023. International Conference on Network and Service Management. vol. 19.
4.  <span id="ref4"></span>Luxemburk, J.; Čejka, T. *Fine-grained TLS services classification with reject option.* *Computer Networks*. 2023, 220 ISSN 1389-1286.
5.  <span id="ref5"></span>Luxemburk, J.; Hynek, K.; Čejka, T.; Lukačovič, A.; Šiška, P. *CESNET-QUIC22: A large one-month QUIC network traffic dataset from backbone lines.* *Data in Brief*. 2023, 2023(46), ISSN 2352-3409.
6.  <span id="ref6"></span>Luxemburk, J.;, Čejka, T.;. *CESNET-TLS22: A large dataset for fine-grained classification of TLS services* [DATASET], 2023. Zenodo. https://doi.org/10.5281/zenodo.7965515
7.  <span id="ref7"></span>Plný, R.; Hynek, K.; Čejka, T. *DeCrypto: Finding Cryptocurrency Miners on ISP networks.* In: *Secure IT Systems*. Cham: Springer, 2022. p. 139-158. ISSN 0302-9743. ISBN 978-3-031-22294-8.
8.  <span id="ref8"></span>Uhříček, D.; Hynek, K.; Čejka, T.; Kolář, D. *BOTA: Explainable IoT malware detection in large networks.* *IEEE Internet of Things Journal*. 2023, 10(10), 8416-8431. ISSN 2327-4662.
9.  <span id="ref9"></span>Koumar, J.; Hynek, K.; Čejka, T.; Pavel, Š. *CESNET-TimeSeries24: Time Series Dataset for Network Traffic Anomaly Detection and Forecasting.* *Scientific Data*. 2025, 12(1), Hynek, K.; Luxemburk, J.; Pešek, J.; Čejka, T.; Šiška, P. *CESNET-TLS-Year22: A year-spanning TLS network traffic dataset from backbone lines.* *Scientific Data*. 2024, 11(1), ISSN 2052-4463.
10. <span id="ref10"></span>Jeřábek, K.; Hynek, K.; Ryšavý, O. *Comparative analysis of DNS over HTTPS detectors.* *Computer Networks*. 2024, 247(247), ISSN 1389-1286.
11. <span id="ref11"></span>Koumar, J.; Hynek, K.; Pešek, J.; Čejka, T. *NetTiSA: Extended IP flow with time-series features for universal bandwidth-constrained high-speed network traffic classification.* *Computer Networks*. 2024, 240 1-22. ISSN 1389-1286.
12. <span id="ref12"></span>Hranický, R.; Horák, A.; Polišenský, J.; Ondryáš, O.; Jeřábek, K.; and Ryšavý, O.; *"Spotting the Hook: Leveraging Domain Data for Advanced Phishing Detection."* *2024 20th International Conference on Network and Service Management (CNSM)*, Prague, Czech Republic, 2024,
13. <span id="ref13"></span>Hranický, R.; Horák, A.; Polišenský, J.; Jeřábek, K.; and Ryšavý, O.; *"Unmasking the Phishermen: Phishing Domain Detection with Machine Learning and Multi-Source Intelligence."* *NOMS 2024-2024 IEEE Network Operations and Management Symposium*, Seoul, Korea, Republic of, 2024, pp. 1-5, doi: 10.1109/NOMS59830.2024.10575573.
14. <span id="ref14"></span>Soukup, D.; Uhříček, D.; Vašata, D.; Čejka, T. *Machine Learning Metrics for Network Datasets Evaluation.* In: *ICT Systems Security and Privacy Protection*. Cham: Springer, 2024. p. 307-320. vol. 679. ISSN 1868-422X. ISBN 978-3-031-56326-3.
15. <span id="ref15"></span>Jančička, L.; Koumar, J.; Soukup, D.; Čejka, T. *Analysis of Statistical Distribution Changes of Input Features in Network Traffic Classification Domain.* In: *NOMS 2024-2024 IEEE Network Operations and Management Symposium*. Seoul: IEEE CLEO/Pacific Rim, 2024. ISSN 2374-9709. ISBN 979-8-3503-2793-9.
16. <span id="ref16"></span>Jančička, L.; Soukup, D.; Koumar, J.; Němec, F.; Čejka, T. *MFWDD: Model-based Feature Weight Drift Detection Showcased on TLS and QUIC Traffic.* In: *2024 20th International Conference on Network and Service Management (CNSM)*. New York: IEEE, 2024. ISSN 2165-963X. ISBN 978-3-903176-66-9.
17. <span id="ref17"></span>Luxemburk, J.; and Hynek, K.;. 2023. *DataZoo: Streamlining Traffic Classification Experiments.* In: *Proceedings of the 2023 on Explainable and Safety Bounded, Fidelitous, Machine Learning for Networking (SAFE '23)*. Association for Computing Machinery, New York, NY, USA, 3–7. https://doi.org/10.1145/3630050.3630176
