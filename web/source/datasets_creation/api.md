---
layout: default
title: API
parent: V5. Semiautomatic datasets creation
permalink: /datasets_creation/api
nav_order: 2
---

# Application Programming Interface (API)

## REST API

Communication with the TCI core system (described in the *Core System* section) is implemented using the **Flask** framework and a **REST API**, which enables interaction with the system through simple HTTP requests.  

- **Authentication** is based on **JSON Web Tokens (JWT)**, ensuring that all actions are performed by an authenticated user.  
- **CSRF protection** is implemented using the `Flask‑WTF` module.  
- The main API is available under `/api/v1/<...>`, allowing parallel development of future API versions while maintaining backward compatibility.  

All API requests are validated in terms of format, content, and user permissions before processing. Each endpoint has a predefined JSON request/response structure. Detailed endpoint specifications and return codes are provided in the **Developer Documentation**.

---

## PyTCI Library

In many cases, it is useful to interact with TCI programmatically. For example, an automated capture can be triggered when a security threat or anomaly is detected, avoiding the delay of manual operation.

The **PyTCI library** provides a Python interface for managing and processing capture jobs (according to user permissions). Its architecture is designed for future extensibility, supporting all actions available via the web interface.

### Installation

PyTCI is part of the TCI package and can be installed using standard Python methods, e.g.:

```bash
pip install pytci
```

Example:

```python
# Import the library
from pytci import PyTCI

# Create an instance and set the access token
tci = PyTCI("localhost", 8080)
tci.set_access_token("<token generated via web interface>")

# Retrieve the list of existing jobs and print their names
existing_jobs = tci.get_jobs()
for job in existing_jobs:
    print(job.name)

# Create a new capture job
new_job = tci.create_job(
    name="My new job!",
    duration=10,
    max_capture_data=50_000,
    lines=["CaptureLine1"],
    filter='',
    script_name=None,
    script_args=None
)

# Get information about the created job and print its status
new_job_info = tci.get_job_status(new_job.id)
print(new_job_info.status)
```

