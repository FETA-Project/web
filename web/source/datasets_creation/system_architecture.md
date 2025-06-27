---
layout: default
title: System Architecture
permalink: /datasets_creation/system-architecture
parent: V5. Semiautomatic datasets creation
has_toc: true
nav_order: 2
---

# System Architecture

The TCI system is composed of multiple interconnected components, detailed below.

## Table of Contents

1. [Core Engine](#core-engine)
2. [REST API](#rest-api)
3. [Web Interface](#web-interface)
4. [Python Client (PyTCI)](#python-client-pytci)
5. [Data Processing Scripts](#data-processing-scripts)
6. [Storage & Artifact Management](#storage--artifact-management)

## Core Engine {#core-engine}

The core engine orchestrates capture job creation, status monitoring, and artifact tracking.

![Core engine overview](/assets/hwctt-fig1.png)

## REST API {#rest-api}

- **POST** `/api/v1/captures` — Create job
- **GET** `/api/v1/captures/{id}` — Job status
- **POST** `/api/v1/captures/{id}/scripts` — Upload script
- **GET** `/api/v1/captures/{id}/download` — Retrieve dataset

```json
{
  "id": "string",
  "links": ["A", "B"],
  "status": "pending",
  "created_at": "2025-06-27T12:00:00Z"
}
```

## Web Interface {#web-interface}

The UI allows manual job creation, script management, and visual monitoring.

![Web UI]( /assets/hwres-fig1.png )

## Python Client (PyTCI) {#python-client-pytci}

```python
from pytci.client import TCIClient
client = TCIClient(base_url="https://tci.example.com", token="YOUR_TOKEN")
```  

## Data Processing Scripts {#data-processing-scripts}

Users can submit custom shell or Python scripts to annotate captures.

![Script upload UI](/assets/classification_nn.png)

## Storage & Artifact Management {#storage--artifact-management}

Captured files, scripts, and logs are stored in object storage with metadata in a relational database.

![Storage schematic](/assets/hwacc-fig1.png)
