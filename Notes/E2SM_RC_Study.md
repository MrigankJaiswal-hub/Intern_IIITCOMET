# E2SM-RC Study

## E2 Service Model – RAN Control

---

# Objective

This document provides a comprehensive study of:

* E2SM-RC
* RAN Control Architecture
* Near-RT RIC Control Framework
* RIC Control Messages
* xApp-Based Optimization
* Policy Control
* Resource Management
* Mobility Control
* QoS Optimization
* RIS-Aware Control

This study extends the E2SM-KPM study.

Relationship:

```text
E2SM-KPM = Monitoring

E2SM-RC = Control
```

Together they form the complete O-RAN closed-loop optimization framework.

---

# 1. O-RAN Control Framework

In O-RAN:

```text
Network Monitoring
         +
Network Control
```

enables:

```text
AI-Native Optimization
```

---

# 2. O-RAN Architecture

```mermaid
flowchart TD

    SMO["SMO"]

    RIC["Near-RT RIC"]

    XAPP["xApp"]

    GNB["O-DU / O-CU"]

    UE["UE"]

    SMO --> RIC

    RIC --> XAPP

    RIC <-->|E2 Interface| GNB

    GNB --> UE
```

---

# 3. What is E2SM-RC?

## Full Form

E2SM-RC

= E2 Service Model – RAN Control

Purpose:

```text
Control Network Behaviour
```

through:

```text
Near-RT RIC
```

---

# 4. Why E2SM-RC Exists

E2SM-KPM only provides:

```text
Observability
```

Example:

```text
CQI = 5

Throughput = 10 Mbps
```

The network knows a problem exists.

However:

```text
How should the network react?
```

E2SM-RC provides the answer.

---

# 5. Monitoring vs Control

```mermaid
flowchart LR

    KPM["E2SM-KPM"]

    RC["E2SM-RC"]

    KPM -->|"Observe"| KPI["KPIs"]

    RC -->|"Control"| NETWORK["Network Behaviour"]
```

---

# 6. E2SM-RC Architecture

```mermaid
flowchart TD

    KPI["Network KPIs"]

    RIC["Near-RT RIC"]

    XAPP["AI xApp"]

    RC["E2SM-RC"]

    GNB["gNB"]

    KPI --> RIC

    RIC --> XAPP

    XAPP --> RC

    RC --> GNB
```

---

# 7. Role of E2SM-RC

E2SM-RC enables:

* Resource allocation
* QoS control
* Mobility optimization
* Traffic steering
* Admission control
* Beam management
* Energy optimization

---

# 8. RAN Control Categories

```mermaid
flowchart TD

    RC["E2SM-RC"]

    QOS["QoS Control"]

    MOB["Mobility Control"]

    RES["Resource Control"]

    TS["Traffic Steering"]

    BEAM["Beam Control"]

    ENERGY["Energy Saving"]

    RC --> QOS

    RC --> MOB

    RC --> RES

    RC --> TS

    RC --> BEAM

    RC --> ENERGY
```

---

# 9. Resource Control

Resource Control manages:

```text
PRBs
Scheduling
Bandwidth
Cell Capacity
```

Example:

```text
Cell A = Congested

Cell B = Underutilized
```

The xApp can redistribute resources.

---

# 10. Mobility Control

Mobility Control manages:

```text
Handover Decisions
Cell Reselection
Load Balancing
```

---

# 11. Traffic Steering

Traffic Steering allows:

```text
Move Users
from
Congested Cell

to

Less Loaded Cell
```

---

# 12. QoS Control

QoS Control optimizes:

```text
Latency
Throughput
Reliability
```

Examples:

| Application        | Priority |
| ------------------ | -------- |
| VoIP               | High     |
| Video              | Medium   |
| Web Browsing       | Medium   |
| Background Traffic | Low      |

---

# 13. Beam Management

Beam Management controls:

```text
Beam Selection
Beam Switching
Beam Refinement
```

---

# 14. E2SM-RC Procedure

Step 1

KPIs collected.

---

Step 2

xApp analyzes network.

---

Step 3

Decision generated.

---

Step 4

RIC Control Request sent.

---

Step 5

gNB applies policy.

---

# 15. E2SM-RC Workflow

```mermaid
sequenceDiagram

participant gNB

participant RIC

participant xApp

gNB->>RIC: KPI Reports

RIC->>xApp: KPI Data

xApp->>RIC: Control Decision

RIC->>gNB: RIC Control Request

gNB-->>RIC: Control Acknowledge
```

---

# 16. RIC Control Message

E2SM-RC uses:

```text
RIC Control Request
```

to deliver:

```text
Policies
Actions
Commands
```

---

# 17. Closed-Loop Optimization

```mermaid
flowchart TD

    KPI["KPIs"]

    AI["AI Engine"]

    CONTROL["RIC Control"]

    NETWORK["Network"]

    KPI --> AI

    AI --> CONTROL

    CONTROL --> NETWORK

    NETWORK --> KPI
```

---

# 18. Relation with E2AP

E2AP provides:

```text
Communication Framework
```

E2SM-RC provides:

```text
Control Semantics
```

Together:

```mermaid
flowchart LR

    E2AP["E2AP"]

    RC["E2SM-RC"]

    GNB["gNB"]

    E2AP --> RC

    RC --> GNB
```

---

# 19. Relation with E2SM-KPM

```mermaid
flowchart TD

    KPM["E2SM-KPM"]

    RC["E2SM-RC"]

    KPI["Telemetry"]

    CTRL["Control"]

    KPM --> KPI

    KPI --> RC

    RC --> CTRL
```

---

# 20. Example Throughput Optimization

Before:

```text
CQI = 4

MCS = 6

Throughput = 12 Mbps
```

KPM reports problem.

---

AI detects issue.

---

RC sends:

```text
Scheduling Optimization
```

---

After:

```text
CQI = 10

MCS = 18

Throughput = 75 Mbps
```

---

# 21. RIS Integration

RIS can become a controllable network element.

Future Architecture:

```mermaid
flowchart LR

    RIS["RIS"]

    GNB["gNB"]

    KPM["E2SM-KPM"]

    RIC["Near-RT RIC"]

    XAPP["RIS xApp"]

    RC["E2SM-RC"]

    RIS --> GNB

    GNB --> KPM

    KPM --> RIC

    RIC --> XAPP

    XAPP --> RC

    RC --> RIS
```

---

# 22. RIS-Aware Control Loop

```mermaid
flowchart TD

    RIS["RIS"]

    CHANNEL["Radio Channel"]

    KPI["CQI / SINR / Throughput"]

    XAPP["RIS xApp"]

    RC["E2SM-RC"]

    RIS --> CHANNEL

    CHANNEL --> KPI

    KPI --> XAPP

    XAPP --> RC

    RC --> RIS
```

---

# 23. Future AI-Based RIC

Inputs:

```text
CQI
SINR
PRB Usage
MCS
Throughput
Latency
```

Outputs:

```text
RIS Configuration
Beam Selection
Scheduler Policy
Power Optimization
```

---

# 24. Reinforcement Learning Control

State:

```text
CQI
SINR
Traffic Load
```

Action:

```text
Control Policy
```

Reward:

```text
Higher Throughput
Lower Latency
Better Coverage
```

---

# 25. Research Vision

Future architecture:

```mermaid
flowchart TD

    UE["UE"]

    RIS["RIS"]

    GNB["gNB"]

    KPM["E2SM-KPM"]

    RIC["Near-RT RIC"]

    XAPP["AI xApp"]

    RC["E2SM-RC"]

    UE --> RIS

    RIS --> GNB

    GNB --> KPM

    KPM --> RIC

    RIC --> XAPP

    XAPP --> RC

    RC --> RIS
```

This creates:

```text
AI-Native Closed-Loop Optimization
```

---

# 26. Relation to Your Internship

Current Progress:

```mermaid
flowchart TD

    CORE["OAI Core"]

    UER["UERANSIM"]

    ORAN["O-RAN Study"]

    KPM["E2SM-KPM"]

    RC["E2SM-RC"]

    XAPP["xApp"]

    RIS["RIS Research"]

    CORE --> UER

    UER --> ORAN

    ORAN --> KPM

    KPM --> RC

    RC --> XAPP

    XAPP --> RIS
```

---

# Mentor Discussion Questions

### What is E2SM-RC?

A service model that enables Near-RT RIC to control network behaviour.

### What is the difference between KPM and RC?

KPM collects KPI measurements.

RC sends control actions.

### What can E2SM-RC control?

Resources, mobility, QoS, beam management, traffic steering, and future RIS configurations.

### Why is E2SM-RC important?

It converts AI decisions into actual network actions.

### How does RIS relate to E2SM-RC?

A RIS-aware xApp can use E2SM-RC to dynamically modify RIS configurations.

---

# Conclusion

E2SM-RC is the control counterpart of E2SM-KPM in O-RAN. While E2SM-KPM provides real-time network telemetry, E2SM-RC enables intelligent control actions that optimize network behaviour. Together, they form the foundation of AI-native RAN optimization. In future RIS-assisted 6G networks, E2SM-RC will allow RIS-aware xApps to dynamically reconfigure the radio environment, creating autonomous and self-optimizing wireless systems.
