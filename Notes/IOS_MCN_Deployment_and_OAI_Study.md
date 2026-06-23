# IOS-MCN Deployment and OpenAirInterface (OAI) Study Notes

# Introduction

The Indian Open Source Mobile Communication Network (IOS-MCN) is an open-source 5G ecosystem developed to enable indigenous deployment of 5G and Beyond-5G communication networks.

The platform integrates:

* O-RAN
* 5G Core
* OpenAirInterface (OAI)
* Near-RT RIC
* SMO
* Cloud Native Infrastructure
* Containerized Deployments

The objective is to create an interoperable, scalable, and intelligent mobile communication ecosystem.

---

# What is OpenAirInterface (OAI)?

OpenAirInterface (OAI) is an open-source implementation of:

* 4G LTE
* 5G NR
* 5G Core Network

It provides software implementations of:

```text
UE
gNB
5GC
```

allowing researchers to deploy complete 5G networks on commodity hardware.

---

# IOS-MCN Technology Stack

```mermaid
flowchart TB

User["UE"]

subgraph RAN["O-RAN Network"]

ORU["O-RU"]
ODU["O-DU"]
OCU["O-CU"]

ORU --> ODU
ODU --> OCU

end

subgraph CORE["5G Core"]

AMF
SMF
UPF
NRF
AUSF
UDM

end

RIC["Near-RT RIC"]

SMO["SMO"]

User --> ORU

OCU --> CORE

RIC --> ODU

SMO --> RIC

CORE --> INTERNET["Internet"]
```

---

# OpenAirInterface Components

## OAI gNB

Acts as the 5G base station.

Responsibilities:

* Radio communication
* Scheduling
* Resource allocation
* Mobility support

---

## OAI 5G Core

Implements:

```text
AMF
SMF
UPF
NRF
AUSF
UDM
```

Provides:

* Registration
* Authentication
* Session Management
* Data Connectivity

---

# OAI Deployment Models

## Bare Metal Deployment

```text
Ubuntu
 ├── OAI gNB
 ├── OAI Core
 └── Local Services
```

Advantages:

* High performance
* Low latency

Disadvantages:

* Difficult scaling

---

## Docker Deployment

```text
Ubuntu
 └── Docker
       ├── AMF Container
       ├── SMF Container
       ├── UPF Container
       ├── NRF Container
       └── AUSF Container
```

Advantages:

* Easy deployment
* Reproducibility
* Isolation

---

## Kubernetes Deployment

```text
Ubuntu
 └── Kubernetes Cluster
         ├── AMF Pod
         ├── SMF Pod
         ├── UPF Pod
         ├── NRF Pod
         └── AUSF Pod
```

Advantages:

* High scalability
* Automation
* Production readiness

---

# Why Docker Matters

Modern 5G deployments rarely run directly on the host OS.

Most components run as containers.

Example:

```bash
docker ps
```

Output:

```text
AMF
SMF
UPF
NRF
AUSF
UDM
```

Each service runs independently.

---

# Important Docker Commands

## List Containers

```bash
docker ps
```

---

## List All Containers

```bash
docker ps -a
```

---

## List Images

```bash
docker images
```

---

## View Logs

```bash
docker logs <container_name>
```

Example:

```bash
docker logs amf
```

---

## Enter Container

```bash
docker exec -it amf bash
```

---

## Stop Container

```bash
docker stop amf
```

---

# Kubernetes in IOS-MCN

Kubernetes manages large deployments.

Responsibilities:

* Container scheduling
* Service discovery
* Auto-healing
* Scaling

---

# Kubernetes Architecture

```mermaid
flowchart TB

Master["Control Plane"]

Node1["Worker Node 1"]

Node2["Worker Node 2"]

Master --> Node1

Master --> Node2

Node1 --> AMF["AMF Pod"]

Node1 --> NRF["NRF Pod"]

Node2 --> SMF["SMF Pod"]

Node2 --> UPF["UPF Pod"]
```

---

# O-RAN Interfaces

## Open Fronthaul

```text
O-RU ↔ O-DU
```

Carries:

* IQ samples
* Timing information

---

## F1 Interface

```text
O-DU ↔ O-CU
```

Carries:

* User data
* Control signaling

---

## E2 Interface

```text
Near-RT RIC ↔ O-DU/O-CU
```

Used for:

* AI optimization
* Scheduling control
* Resource management

---

# Near-RT RIC

## Purpose

Provides intelligent control of the RAN.

Applications:

* Load balancing
* Scheduling optimization
* Beam management
* Mobility optimization

---

# SMO

Service Management and Orchestration.

Responsibilities:

* Deployment
* Monitoring
* Configuration
* Analytics

SMO manages:

```text
RIC
O-RU
O-DU
O-CU
Core Network
```

---

# OAI Registration Flow

```mermaid
sequenceDiagram

participant UE
participant gNB
participant AMF
participant AUSF
participant UDM

UE->>gNB: Registration Request
gNB->>AMF: Forward Request

AMF->>AUSF: Authentication

AUSF->>UDM: Subscriber Data

UDM-->>AUSF: Authentication Vector

AUSF-->>AMF: Authentication Success

AMF-->>UE: Registration Accept
```

---

# OAI PDU Session Flow

```mermaid
sequenceDiagram

participant UE
participant gNB
participant AMF
participant SMF
participant UPF

UE->>gNB: PDU Session Request

gNB->>AMF: Forward Request

AMF->>SMF: Session Setup

SMF->>UPF: Create Tunnel

UPF-->>SMF: Tunnel Created

SMF-->>AMF: Session Accepted

AMF-->>UE: PDU Session Accept
```

---

# Where MAC Layer Exists

The MAC layer resides inside:

```text
O-DU
```

Protocol Stack:

```text
RRC
PDCP
RLC
MAC
PHY
```

Responsibilities:

* Scheduling
* PRB Allocation
* HARQ
* QoS

---

# RIS Integration with IOS-MCN

Future architecture:

```mermaid
flowchart LR

UE

RIS

ORU["O-RU"]

ODU["O-DU"]

RIC["Near-RT RIC"]

UE --> RIS

RIS --> ORU

ORU --> ODU

RIC --> ODU

ODU --> CORE["5G Core"]
```

---

# RIS and MAC Interaction

RIS improves:

```text
SNR
 ↓
CQI
 ↓
MCS
 ↓
Scheduling Efficiency
 ↓
Throughput
```

This is a major research direction in Beyond-5G systems.

---

# Deployment Workflow

Typical IOS-MCN deployment process:

```text
Ubuntu Setup
      ↓
Docker Installation
      ↓
Container Deployment
      ↓
5G Core Deployment
      ↓
OAI gNB Deployment
      ↓
UE Registration
      ↓
PDU Session Establishment
      ↓
Traffic Validation
      ↓
O-RAN Integration
      ↓
RIC Integration
      ↓
RIS Integration
```

---

# Practical Skills Required

For successful IOS-MCN deployment:

## Linux

* File system
* Networking
* Process management

---

## Docker

* Images
* Containers
* Logs
* Networking

---

## Kubernetes

* Pods
* Services
* Deployments

---

## Wireshark

* Packet capture
* Protocol analysis

---

## O-RAN

* O-RU
* O-DU
* O-CU
* RIC

---

## 5G Core

* AMF
* SMF
* UPF
* Registration
* PDU Session

---

# Internship Relevance

This deployment knowledge directly supports:

* IOS-MCN deployment
* OAI deployment
* O-RAN studies
* MAC-layer research
* RIS-assisted communication
* 5G testbed validation
* UGV communication
* IoT connectivity

---

# Key Takeaways

1. IOS-MCN combines O-RAN, OAI, and 5G Core technologies.
2. OAI provides open-source implementations of gNB and 5GC.
3. Docker is the foundation of modern 5G deployments.
4. Kubernetes enables scalable orchestration.
5. MAC-layer functionality primarily resides in the O-DU.
6. Near-RT RIC provides intelligent radio optimization.
7. SMO manages the overall network lifecycle.
8. Registration and PDU Session Establishment are the first deployment milestones.
9. RIS can be integrated with O-RAN and RIC for intelligent beam control.
10. Understanding the relationship between IOS-MCN, OAI, MAC, O-RAN, and RIS is critical for contributing to advanced 5G testbeds.
