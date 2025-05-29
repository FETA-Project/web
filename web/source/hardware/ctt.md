---
layout: default
title: Connection Tracking Table
nav_order: 3
permalink: hardware/ctt
parent: V1. Hardware
has_children: false
---

# Connection Tracking Table

The component known as the Connection Tracking Table (CTT) is primarily responsible for storing and managing context information for active network flows. It enables stateful processing of network traffic or aggregation of statistical information for individual network flows, but it can also be used for other forms of hardware acceleration. In order to be able to work with units of up to tens of millions of network flows, the CTT stores the relevant context information in an external DRAM-type memory.

The conceptual diagram of the CTT operation is shown in Figure 1. With the arrival of a record of a new packet (Packet Record structure), the CTT searches for existing context information for the corresponding network flow (Find Context operation) in the network flow context storage (Flow Context Storage block). The searched context information is then passed together with the Packet Record structure to the external Flow Context Processor unit. In this unit, a user-defined update of the context information takes place according to the information stored in the Packet Record structure (Context Update operation). The output of the Flow Context Processor unit is therefore the updated context information for the relevant network flow and further information for further processing of the packet (Packet Info structure). Packet processing then ends with writing the context information back to the Flow Context Storage (Finalize Context operation) and passing the Packet Info structure to the next unit.

![Conceptual diagram of the functioning of the network flow table.]({{ site.baseurl }}/assets/hwctt-fig1.png)
*Figure 1: Conceptual diagram of the functioning of the network flow table.*

The basic activity of the CTT unit therefore consists in implementing a read-modify-write cycle (see Find Context, Context Update and Finalize Context operations in Figure 1), within which the context information of the relevant network flow is read, updated and stored back using the information in the Packet Record structure of the incoming packet. In the case of the first packet of a new flow, the Find Context operation fails, and therefore a new context is created and initialized within the Context Update operation, which is subsequently stored in the Flow Context Storage by the Finalize Context operation. On the contrary, in the case of termination of network flow monitoring (e.g. when the adjustable timeout expires or based on a request from the software), the current context information is not stored in the Flow Context Storage as part of the Finalize Context operation, but is exported to the output.

## CTT Architecture

The architecture of the Connection Tracking Table (CTT) and its connection to other components is designed to enable efficient processing of incoming packets and their metadata. The architecture of the implemented network flow table and its connection to related components is shown in Figure 2. The key starting point is the Flow State Table module, which serves as a hash table for identifying active network flows. When a new packet arrives, this table is used to determine whether the corresponding flow already exists. If not, an entry is created and context information is requested from the Flow Context Storage, which can be obtained either from the Bucket Cache (buffer memory in the FPGA) or from external memory via the External Memories Manager.

Regardless of whether the flow already exists, the metadata of each packet is stored in the Metadata Buffer - a specialized memory structure based on unidirectional linked lists. Each element of the list contains the flow identifier, the packet metadata, and a pointer to the next element. Metadata is stored in the order of arrival and each record is assigned a so-called ticket, which determines the address in memory.

![ Architecture of the network flow table (colored blocks) and its connection to related components (white blocks).]({{ site.baseurl }}/assets/hwctt-fig2.png)
*Figure 2:  Architecture of the network flow table (colored blocks) and its connection to related components (white blocks).*

In the case of a new flow, the Flow State Table allocates a new ticket and writes it as the initial element of the list in the Metadata Buffer. In the case of an existing flow, it updates the pointer to the last element of the list. Entries in the table can be released only if there is no pending metadata left in the buffer for the given flow.

The Bucket Cache module serves as a cache of contextual information for recently processed flows. When a context request is made, the presence of data in this cache is first verified, which saves access to slower external memory. If the context is not found in the cache, access is made via the External Memories Manager, which ensures the translation of virtual addresses into physical ones and allows access to various types of memory - from DRAM to HBM to internal FPGA memory - transparently to other CTT modules.

The key control element of the CTT is the Modify Manager. This module coordinates the reading of metadata from the Metadata Buffer and passes it along with the relevant context to the Modify Unit (also known as the Flow Context Processor), which performs its own update of the context information. An important aspect is maintaining the processing order according to the arrival of packets, which ensures flow consistency. The output from the Modify Unit is the updated context and the Packet Info structure, which determines how the processed packet should be handled. After processing all available metadata for a particular flow, the updated context is written back to the Bucket Cache. The Modify Manager also manages the export of context data to output units or the host system and signals when it is possible to release an entry from the Flow State Table.

## Selected features of the implemented CTT
The implemented CTT addresses all the required parameters that are essential from the point of view of deploying accelerated probes in 400Gb networks. Specifically, the following parameters were achieved in CTT:

- **Scalable capacity of the network flow table.** Thanks to the External Memories Manager module, it is possible to connect up to 16 independent blocks of external memory to the CTT, not only of the DRAM type. The use of these memories is fully transparent from the CTT perspective and, regardless of the selected configuration, corresponds to access to one contiguous memory space. At the same time, the number of necessary accesses to external memory is reduced by using the Bucket Cache module, which implements a buffer for Flow Context Storage, and thus allows servicing a number of requests for access to external memory using only the memories available on the FPGA chip used.

- **High performance.** The current CTT implementation allows processing multiple new Packet Record structures in a single clock cycle; this option can be parameterized using the appropriate generic parameter. Thanks to this, the performance of the current implementation is sufficiently high for deployment in 400Gb networks and meets the expected assumptions.

- **Ensuring data consistency.** By storing metadata for incoming packets in the Metadata Buffer module in the form of one-way linked lists, where each network flow has its own dedicated list, it is ensured that the metadata of packets of the same flow will be stored — and therefore subsequently processed — in the same order in which they appeared at the input. This memory arrangement does not limit the processing of metadata of packets of different flows outside the original order, because information about the currently processed network flows is stored independently of the metadata, specifically in the Flow State Table module.

- **High memory utilization rate thanks to the proposed addressing scheme.** In order to achieve a high level of memory utilization allocated for Flow Context Storage, we have chosen an approach known as bucketing for the implementation of the relevant hash table. It consists in dividing each addressable hash table entry into multiple so-called buckets into which colliding records can be placed. When using N buckets, the hash table is not marked as full when the first collision occurs, but only when the Nth collision occurs on one item.
