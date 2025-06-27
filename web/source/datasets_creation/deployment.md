---
layout: default
title: Deployment
permalink: /datasets_creation/deployment
parent: V5. Semiautomatic datasets creation
has_toc: true
nav_order: 3
---

# Deployment

This section covers installation methods, configuration steps, and usage guidelines for deploying TCI.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Configuration](#configuration)
4. [Starting the Services](#starting-the-services)
5. [Using the Python Client](#using-the-python-client)
6. [Cleanup & Maintenance](#cleanup--maintenance)

## Prerequisites {#prerequisites}

- Python 3.8+
- Docker & Docker Compose (or Kubernetes)
- PostgreSQL 12+
- Object storage (e.g., AWS S3)

## Installation {#installation}

### 1. Clone the repository

```bash
git clone https://github.com/example/traffic-capture-interface.git
cd traffic-capture-interface
```

### 2. Python dependencies

```bash
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 3. Docker Compose

```bash
docker-compose up -d --build
```

## Configuration {#configuration}

Edit `.env`:

```dotenv
TCI_DB_URL=postgresql://user:pass@db:5432/tci
TCI_STORAGE_ENDPOINT=http://storage:9000
TCI_SECRET_KEY=your_secret
```

## Starting the Services {#starting-the-services}

```bash
# Ensure db and storage are running
docker-compose up -d core api web
```

## Using the Python Client {#using-the-python-client}

```bash
pip install pytci
python - << 'EOF'
from pytci.client import TCIClient
client = TCIClient(base_url="http://localhost:8000", token="YOUR_TOKEN")
job = client.create_capture(["line1"], max_size_gb=1, duration_days=1)
client.upload_script(job.id, "./process.sh")
client.download_capture(job.id, "./output.pcap")
EOF
```

## Cleanup & Maintenance {#cleanup--maintenance}

- Database migrations: `alembic upgrade head`
- Log rotation: configure `logrotate` for `/var/log/tci/*.log`
- Remove old artifacts:
  ```bash
tci-cleanup --days 30
```
