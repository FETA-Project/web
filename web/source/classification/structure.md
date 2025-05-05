---
layout: default
title: Structure and Virtual Machine
nav_order: 1
permalink: classification/structure
parent: V3. Classification
has_children: false
---

# Structure of the Deliverable and Demonstration Virtual Machine

The set of classification modules for detecting security threats is provided as a software archive containing source code, installation scripts, and sample data. The structure of the archive is shown in following:
```
.
├── LICENSE……………………………………License file
├── README.md………………………………Readme file
├── Vagrantfile………………………………Definition of the virtual machine
├── clfs
│   ├── crypto-clf…………………………Cryptocurrency mining detector
│   ├── ssh-clf………………………………SSH classifier
│   ├── tunnel-clf…………………………Network tunnel detector
│   └── webservice-clf…………………TLS and QUIC communication classifiers
├── global_dependencies……Dependencies required for installation
│   ├── rpm
│   └── yum-packages
└── install-fetav3.sh…………………Installation script
```

The archive also contains a Vagrantfile that defines a virtual machine intended for demonstration purposes. After launching the virtual machine using the vagrant up command in the terminal, all required dependencies for running the classification modules are automatically installed. These include the NEMEA system, the Python programming language, and standard machine learning libraries such as PyTorch and Scikit-learn. Following dependency installation, the classification modules themselves are also installed. The installation process generates scripts in the user’s home directory that simplify running the modules on the demonstration data. A listing of the home directory with these scripts is shown in followig:
```
[vagrant@oracle8 ~]$ ls
run-miner-det.sh   run-tunnel-visualiser.sh      run-webservice-tls.sh
run-ssh.sh         run-webservice-evaluation.sh
run-tunnel-det.sh  run-webservice-quic.sh
```
