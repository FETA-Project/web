---
layout: default
title: Software for working with data
parent: V4. Datasets
permalink: /datasets/software_for_data/
nav_order: 2
---

# Software for working with data

To facilitate work with the created datasets, we have developed two tools, CESNET DataZoo and CESNET TS-Zoo, which allow easy downloading of datasets and their use in experiments. Both tools are available as open-source.

## CESNET DataZoo

The CESNET DataZoo tool was created to simplify work with datasets containing network flows. The tool is available as a Python library and allows for easy downloading of datasets, their filtering, and use in experiments. The tool is designed to be easily extensible with new datasets.

### Key Features

*   **Easy download:** Datasets can be downloaded with a single command. The tool automatically handles downloading from the Zenodo platform and unpacking the data.
*   **Data filtering:** The tool allows for filtering data based on various criteria, such as time range, protocol, or IP address.
*   **Integration with machine learning libraries:** The tool provides data in a format that is directly usable in popular machine learning libraries such as Scikit-learn and PyTorch.
*   **Extensibility:** New datasets can be easily added to the tool by creating a simple configuration file.

## CESNET TS-Zoo

The CESNET TS-Zoo tool is a specialized tool for working with time-series datasets. Similar to DataZoo, it is available as a Python library and simplifies the process of downloading and preparing time-series data for analysis.

### Key Features

*   **Support for various time-series formats:** The tool can work with various data formats, including CSV, JSON, and Parquet.
*   **Data preprocessing:** It includes functions for data preprocessing, such as resampling, normalization, and handling of missing values.
*   **Visualization:** The tool offers functions for easy visualization of time series.
*   **Benchmarking:** It allows for easy comparison of different anomaly detection and prediction models on the provided datasets.

