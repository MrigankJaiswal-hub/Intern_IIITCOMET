# KPM xApp Implementation Guide

## Building Your First O-RAN xApp Using FlexRIC

---

# Objective

This document explains how a Key Performance Measurement (KPM) xApp is implemented in a Near-RT RIC environment using FlexRIC.

By the end of this guide, you will understand:

* KPM xApp Architecture
* E2SM-KPM Workflow
* KPI Subscription Mechanism
* KPI Report Processing
* FlexRIC SDK Structure
* xApp Development Flow
* RIS-Aware KPI Collection

This guide marks the transition from:

```text
Theory
```

to

```text
Implementation
```

---

# 1. Current Progress

Completed:

```text
✓ OAI Core Deployment

✓ UERANSIM Deployment

✓ UE Registration

✓ PDU Session Establishment

✓ O-RAN Architecture Study

✓ E2 Interface Study

✓ E2AP Study

✓ E2SM-KPM Study

✓ E2SM-RC Study

✓ FlexRIC Study
```

Current Stage:

```text
KPM xApp Implementation
```

---

# 2. What Does a KPM xApp Do?

Purpose:

```text
Receive KPI Reports
Analyze KPIs
Display KPIs
Trigger Decisions
```

Examples:

```text
CQI Monitoring

Throughput Monitoring

PRB Monitoring

Latency Monitoring
```

---

# 3. High-Level Architecture

```mermaid
flowchart LR

    UE["UE"]

    GNB["gNB"]

    AGENT["E2 Agent"]

    RIC["FlexRIC"]

    KPM["KPM xApp"]

    UE --> GNB

    GNB --> AGENT

    AGENT --> RIC

    RIC --> KPM
```

---

# 4. Data Flow

```mermaid
sequenceDiagram

participant UE

participant gNB

participant E2Agent

participant FlexRIC

participant xApp

UE->>gNB: User Traffic

gNB->>E2Agent: KPI Collection

E2Agent->>FlexRIC: E2SM-KPM Report

FlexRIC->>xApp: KPI Report
```

---

# 5. KPM xApp Responsibilities

The xApp performs:

### KPI Subscription

Request metrics from gNB.

---

### KPI Reception

Receive E2SM-KPM reports.

---

### KPI Parsing

Extract:

```text
CQI

MCS

PRB

Throughput

Latency
```

---

### KPI Visualization

Generate:

```text
Logs

Graphs

Dashboards
```

---

### KPI Analytics

Support:

```text
AI Models

Optimization

Decision Making
```

---

# 6. FlexRIC Software Architecture

```mermaid
flowchart TD

    SDK["FlexRIC SDK"]

    E2AP["E2AP"]

    KPM["E2SM-KPM"]

    XAPP["KPM xApp"]

    SDK --> E2AP

    E2AP --> KPM

    KPM --> XAPP
```

---

# 7. xApp Lifecycle

```mermaid
flowchart TD

    START["Start xApp"]

    CONNECT["Connect to RIC"]

    SUBSCRIBE["Subscribe KPI"]

    RECEIVE["Receive Reports"]

    ANALYZE["Analyze KPIs"]

    DISPLAY["Display Results"]

    START --> CONNECT

    CONNECT --> SUBSCRIBE

    SUBSCRIBE --> RECEIVE

    RECEIVE --> ANALYZE

    ANALYZE --> DISPLAY
```

---

# 8. KPI Subscription Process

The xApp must first subscribe.

Architecture:

```mermaid
sequenceDiagram

participant xApp

participant RIC

participant gNB

xApp->>RIC: KPI Subscription Request

RIC->>gNB: E2 Subscription

gNB-->>RIC: Subscription Accepted

RIC-->>xApp: Subscription Active
```

---

# 9. Example KPIs

## CQI

Channel Quality Indicator

Range:

```text
1 – 15
```

---

## MCS

Modulation and Coding Scheme

Examples:

```text
QPSK

16QAM

64QAM

256QAM
```

---

## PRB

Physical Resource Block

Measures:

```text
Resource Utilization
```

---

## Throughput

Measures:

```text
Data Rate
```

---

## Latency

Measures:

```text
Network Delay
```

---

# 10. KPI Report Structure

Example:

```json
{
  "cell_id": 1,
  "cqi": 12,
  "mcs": 20,
  "prb": 68,
  "throughput": 120,
  "latency": 10
}
```

---

# 11. KPI Processing Workflow

```mermaid
flowchart LR

    REPORT["KPM Report"]

    PARSE["Parser"]

    KPI["Extract KPIs"]

    DECISION["Analytics"]

    REPORT --> PARSE

    PARSE --> KPI

    KPI --> DECISION
```

---

# 12. Basic xApp Logic

Pseudo-code:

```python
while True:

    report = receive_kpm_report()

    cqi = report.cqi

    throughput = report.throughput

    print(cqi)

    print(throughput)
```

---

# 13. CQI Monitoring Example

Pseudo-code:

```python
if cqi < 5:
    print("Poor Channel")

elif cqi < 10:
    print("Moderate Channel")

else:
    print("Good Channel")
```

---

# 14. Throughput Monitoring Example

Pseudo-code:

```python
if throughput < 20:
    print("Low Throughput")

else:
    print("Healthy Throughput")
```

---

# 15. PRB Utilization Monitoring

Pseudo-code:

```python
if prb > 80:
    print("Network Congestion")
```

---

# 16. KPI Dashboard Architecture

```mermaid
flowchart TD

    CQI["CQI"]

    PRB["PRB"]

    MCS["MCS"]

    TH["Throughput"]

    LAT["Latency"]

    DASH["Dashboard"]

    CQI --> DASH

    PRB --> DASH

    MCS --> DASH

    TH --> DASH

    LAT --> DASH
```

---

# 17. AI-Driven KPM xApp

Input:

```text
CQI

PRB

MCS

SINR
```

Output:

```text
Optimization Recommendation
```

---

Architecture:

```mermaid
flowchart TD

    KPI["KPIs"]

    AI["AI Engine"]

    DECISION["Decision"]

    KPI --> AI

    AI --> DECISION
```

---

# 18. Relation to RIS

This is where your internship becomes unique.

RIS improves:

```text
Channel Quality
```

which affects:

```text
CQI
```

which affects:

```text
MCS
```

which affects:

```text
Throughput
```

---

# 19. RIS Monitoring Loop

```mermaid
flowchart TD

    RIS["RIS"]

    SNR["Higher SNR"]

    CQI["Higher CQI"]

    MCS["Higher MCS"]

    TH["Higher Throughput"]

    KPM["KPM xApp"]

    RIS --> SNR

    SNR --> CQI

    CQI --> MCS

    MCS --> TH

    TH --> KPM
```

---

# 20. RIS-Aware xApp Concept

Future xApp:

```mermaid
flowchart TD

    RIS["RIS"]

    KPI["KPIs"]

    KPM["KPM xApp"]

    AI["AI Logic"]

    CONTROL["RIS Control"]

    RIS --> KPI

    KPI --> KPM

    KPM --> AI

    AI --> CONTROL

    CONTROL --> RIS
```

---

# 21. Future Control Loop

Current KPM xApp:

```text
Monitor Only
```

Future RC xApp:

```text
Monitor + Control
```

---

# 22. Complete O-RAN Workflow

```mermaid
flowchart TD

    UE["UE"]

    GNB["gNB"]

    E2["E2 Agent"]

    RIC["FlexRIC"]

    KPM["KPM xApp"]

    AI["AI Engine"]

    UE --> GNB

    GNB --> E2

    E2 --> RIC

    RIC --> KPM

    KPM --> AI
```

---

# 23. Mentor Discussion Questions

### What is a KPM xApp?

An xApp that receives and analyzes network KPIs.

### What is E2SM-KPM?

A service model used for KPI reporting.

### What KPIs can be collected?

CQI, MCS, PRB, Throughput, Latency, SINR.

### Why is KPM important?

It provides visibility into network performance.

### Why is KPM needed before RC?

Control decisions require KPI observations.

### How does RIS relate to KPM?

RIS performance improvements are reflected through KPI changes.

---

# 24. Implementation Roadmap

```mermaid
flowchart TD

    FLEXRIC["FlexRIC"]

    KPM["KPM xApp"]

    RC["RC xApp"]

    AI["AI Engine"]

    RIS["RIS xApp"]

    FLEXRIC --> KPM

    KPM --> RC

    RC --> AI

    AI --> RIS
```

---

# 25. Research Roadmap

Current:

```text
✓ OAI Core

✓ UERANSIM

✓ O-RAN Study

✓ E2 Interface

✓ E2AP

✓ E2SM-KPM

✓ E2SM-RC

✓ FlexRIC

✓ KPM xApp
```

Next:

```text
FlexRIC Deployment
        ↓
KPM xApp Execution
        ↓
RC xApp Study
        ↓
RC xApp Development
        ↓
RIS-Aware xApp
```

---

# Conclusion

The KPM xApp is the first practical xApp developed in O-RAN environments. It subscribes to KPI measurements using E2SM-KPM, processes real-time network data, and provides the foundation for AI-driven optimization. Understanding KPM xApps is essential before developing RC xApps and RIS-aware control applications using FlexRIC and Near-RT RIC.
