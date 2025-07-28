---
layout: default
title: Architecture
parent: V5. Semiautomatic datasets creation
permalink: /datasets_creation/architecture
nav_order: 1
---

# Example Usage of the TCI System

The TCI system is designed to enable researchers to create annotated network traffic datasets in a simple yet secure way, with a strong emphasis on preserving user privacy. These datasets can be used for network traffic analysis and related experiments.

## Example Scenarios

- **Researcher scenario:**  
  A researcher wants to create a dataset of QUIC protocol traffic, as there are currently no publicly available datasets for this relatively new protocol. The researcher logs into the TCI system and configures a new capture on monitored network links A and B. They specify a maximum size of 10 GB and a maximum duration of 1 day. A traffic filter is applied to capture only QUIC traffic. To process the dataset, the researcher uploads a Bash script that uses pre‑configured tools (e.g., `ipfixprobe` or the NEMEA framework) for automated processing. Once the capture job is submitted, the researcher can log out. After the capture is completed, the dataset is stored in TCI and can be downloaded with a single click.

- **Network administrator scenario:**  
  A network administrator wants to capture traffic related to detected anomalies for later analysis. They write a Python script that is triggered by an anomaly detection tool. Using the **PyTCI library**, the script automatically starts a capture on the affected network link. After the anomaly is resolved, the administrator has a ready dataset that can be analyzed and used to improve the configuration of detection and defense systems.

---

# Architecture Overview

The TCI system is modular, enabling easy future maintenance and expansion. Its core components are described below.  
The system is implemented in **Python 3.9**, and its architecture is shown in *Figure 1*.

![High‑level architecture diagram]({{ site.baseurl }}/assets/v5_image2.png)

---

## Core Components

### Core System
The core manages the initialization and interconnection of all modules, including database connectivity, Docker client integration, and module coordination. It is implemented using the **Flask** framework.

### Database ORM
The system uses **SQLAlchemy** to provide an Object‑Relational Mapping (ORM) interface for relational databases. Database migrations are supported to allow incremental schema updates without data loss. The pilot deployment used a **MySQL backend**, but the design allows other database systems.

### LDAP Client
The LDAP client module creates local user accounts based on LDAP groups and synchronizes them at each login. It supports both central LDAP‑based authentication and independent local user accounts.

### Logging Module
This module collects and outputs system events and user interactions. Logs follow a standardized format with timestamps and module identifiers. Logs can be collected by system logging tools (e.g., *journald* / *syslog*) or stored as local files.

### Docker Client
Captured data can be processed using user‑provided shell scripts (e.g., anonymization, statistics generation). For security, these scripts are executed in **Docker containers**, allowing isolated environments with custom tools.

---

## TCI Backend

The backend handles job creation and management. A **job** represents one or more parallel captures with identical parameters. It consists of two parts:

- **Hive** – The server component responsible for managing jobs, communicating with Drones via **Socket.IO**, queuing and launching captures, updating job states, and retrieving collected data. It also provides an API for the information system.
- **Drone** – A lightweight client component running on network probes or devices. It performs captures on specified network interfaces using tools like `tcpdump` or NDK utilities. Drone runs as a system service (currently supports **systemd** on Linux; can be extended to macOS `launchd` or Windows services).

---

# User Interface

The graphical interface is implemented using **Angular 13.3.5** with **Material Design 2** components. It separates application logic from design, uses **TypeScript**, and provides an intuitive, interactive experience.

## Key Features

- **Login page** – Authentication via username/password (*Figure 2*).  
  ![Login form]({{ site.baseurl }}/assets/v5_image2.png)

- **Dashboard** – Overview of all capture jobs (*Figure 3*).  
  ![Job list]({{ site.baseurl }}/assets/v5_image3.png)

- **Job details** – Detailed status view of each job, including data distribution across Drones (*Figure 4*).  
  ![Job details]({{ site.baseurl }}/assets/v5_image4.png)

- **Job creation form** – Dynamic form for creating new capture jobs, validating inputs in real‑time (*Figure 5*).  
  ![Job creation form]({{ site.baseurl }}/assets/v5_image5.png)

- **User management** – List of user accounts, options to edit or remove them, and a form to create new local users (*Figures 6 & 7*).  
  ![User list]({{ site.baseurl }}/assets/v5_image6.png)  
  ![New user form]({{ site.baseurl }}/assets/v5_image7.png)

- **Authorization groups** – Management of permission groups (*Figure 8*).  
  ![Authorization groups]({{ site.baseurl }}/assets/v5_image8.png)

- **Drone overview** – List of available network links and active Drone modules (*Figure 9*).  
  ![Drone list]({{ site.baseurl }}/assets/v5_image9.png)

- **Application tokens** – For integration with external tools (e.g., PyTCI) (*Figure 10*).  
  ![Token management]({{ site.baseurl }}/assets/v5_image10.png)

- **Logs** – Administrators can download logs for auditing and debugging (*Figure 11*).  
  ![Log list]({{ site.baseurl }}/assets/v5_image11.png)