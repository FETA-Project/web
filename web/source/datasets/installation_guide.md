---
layout: default
title: Installation Guide
parent: V4. Datasets
permalink: /datasets/installation_guide/
nav_order: 4
---

# Installation Guide

This guide describes the installation of the tools developed for working with the datasets.

## CESNET DataZoo and TS-Zoo

Both tools, `CESNET DataZoo` and `CESNET TS-Zoo`, are available as Python libraries and can be installed using the `pip` package manager.

### Prerequisites

*   Python 3.8 or higher
*   `pip` package manager

### Installation

To install the libraries, run the following commands in your terminal:

```bash
pip install cesnet-datazoo
pip install cesnet-ts-zoo
```

### Verification

After installation, you can verify that the tools are correctly installed by importing them in Python:

```python
import cesnet_datazoo
import cesnet_ts_zoo

print(f"CESNET DataZoo version: {cesnet_datazoo.__version__}")
print(f"CESNET TS-Zoo version: {cesnet_ts_zoo.__version__}")
```

If the installation was successful, the versions of the installed libraries will be printed.