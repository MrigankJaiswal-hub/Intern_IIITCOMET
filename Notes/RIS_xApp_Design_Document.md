# RIS-Aware xApp Design Document

## Author

Mrigank Jaiswal

## Internship

COMET Foundation – IIIT Bangalore

## Project Phase

Phase 3 – RIS-Aware xApp Design

## Domain

O-RAN • Near-RT RIC • FlexRIC • RIS • E2SM-KPM • E2SM-RC • MAC Scheduling • 5G/6G

---

# 1. Introduction

The evolution of 5G-Advanced and 6G networks requires intelligent control of the radio environment. Reconfigurable Intelligent Surfaces (RIS) provide a programmable propagation environment capable of improving signal quality, reducing interference, and increasing spectral efficiency.

However, RIS requires a decision-making framework capable of:

* Monitoring network conditions
* Evaluating RIS effectiveness
* Selecting optimal RIS configurations
* Triggering network adaptation

The O-RAN architecture provides such a framework through:

* Near-RT RIC
* E2 Interface
* E2SM-KPM
* E2SM-RC
* xApps

This document presents the design of a RIS-Aware xApp that utilizes KPM metrics to optimize RIS behavior and influence MAC scheduling decisions.

---

# 2. System Objective

The RIS-Aware xApp will:

1. Collect KPM metrics from the RAN.
2. Analyze network performance.
3. Detect degraded radio conditions.
4. Estimate RIS optimization opportunities.
5. Generate RIS control recommendations.
6. Deliver control decisions using E2SM-RC.
7. Improve scheduling efficiency.

---

# 3. Overall Architecture

```mermaid
flowchart TD

RIS["RIS Surface"]

UE["User Equipment"]

KPM["KPM Reports"]

RIC["Near-RT RIC"]

XAPP["RIS-Aware xApp"]

RC["E2SM-RC Control"]

MAC["MAC Scheduler"]

RIS --> UE

UE --> KPM

KPM --> RIC

RIC --> XAPP

XAPP --> RC

RC --> MAC
```

---

# 4. End-to-End Data Flow

```mermaid
sequenceDiagram

participant UE
participant gNB
participant Agent
participant RIC
participant xApp
participant Scheduler

UE->>gNB: Traffic

gNB->>Agent: Measurements

Agent->>RIC: KPM Indications

RIC->>xApp: KPM Reports

xApp->>xApp: Analytics

xApp->>RIC: RC Control Request

RIC->>Scheduler: Scheduling Control
```

---

# 5. Functional Components

The xApp consists of six logical modules.

```mermaid
flowchart TD

A["KPM Collector"]

B["Metrics Database"]

C["Analytics Engine"]

D["Decision Engine"]

E["RIS Optimizer"]

F["RC Controller"]

A --> B

B --> C

C --> D

D --> E

E --> F
```

---

# 6. KPM Collector Module

Purpose:

Receive E2SM-KPM reports.

Input:

```text
DRB.UEThpDl
DRB.UEThpUl
DRB.RlcSduDelayDl
RRU.PrbTotDl
RRU.PrbTotUl
PDCP Volume
```

Output:

```text
Time-series KPI database
```

---

# 7. Analytics Engine

Purpose:

Analyze radio performance trends.

Inputs:

* Throughput
* Delay
* PRB utilization
* CQI

Outputs:

```text
Network Quality Score
```

---

# 8. Analytics Workflow

```mermaid
flowchart TD

Metrics["KPM Metrics"]

Filter["Filtering"]

Average["Moving Average"]

Trend["Trend Detection"]

Score["Network Quality Score"]

Metrics --> Filter

Filter --> Average

Average --> Trend

Trend --> Score
```

---

# 9. RIS Optimization Logic

The xApp estimates whether RIS reconfiguration could improve performance.

```mermaid
flowchart TD

LowCQI["Low CQI"]

LowThroughput["Low Throughput"]

HighDelay["High Delay"]

RISGain["Estimated RIS Gain"]

Decision["Need RIS Optimization?"]

LowCQI --> RISGain

LowThroughput --> RISGain

HighDelay --> RISGain

RISGain --> Decision
```

---

# 10. RIS State Selection

RIS may have multiple beam states.

Example:

```text
State A
State B
State C
State D
```

Selection process:

```mermaid
flowchart TD

Current["Current RIS State"]

Evaluate["Evaluate KPIs"]

Predict["Predict Gain"]

Select["Best RIS State"]

Current --> Evaluate

Evaluate --> Predict

Predict --> Select
```

---

# 11. Decision Engine

The decision engine determines whether intervention is required.

Example logic:

```python
if CQI < 6:
    optimize_RIS()

if throughput < threshold:
    optimize_RIS()

if delay > threshold:
    optimize_RIS()
```

---

# 12. xApp Decision Pipeline

```mermaid
flowchart TD

KPM["KPM Metrics"]

Analysis["Analytics"]

Decision["Decision Engine"]

RISOpt["RIS Optimization"]

Control["RC Control Action"]

KPM --> Analysis

Analysis --> Decision

Decision --> RISOpt

RISOpt --> Control
```

---

# 13. E2SM-RC Integration

The xApp communicates through E2SM-RC.

Purpose:

* Radio control
* Resource control
* Scheduler guidance

Architecture:

```mermaid
flowchart TD

xApp

RC["E2SM-RC"]

RIC["Near-RT RIC"]

Agent["E2 Agent"]

Scheduler

xApp --> RC

RC --> RIC

RIC --> Agent

Agent --> Scheduler
```

---

# 14. Scheduler Interaction

The xApp does not replace the scheduler.

Instead it provides guidance.

```mermaid
flowchart TD

RIS["RIS Improvement"]

CQI["CQI Increase"]

Scheduler["MAC Scheduler"]

MCS["Higher MCS"]

PRB["Efficient PRB Allocation"]

Throughput["Higher Throughput"]

RIS --> CQI

CQI --> Scheduler

Scheduler --> MCS

Scheduler --> PRB

MCS --> Throughput

PRB --> Throughput
```

---

# 15. CQI-Based Scheduler Optimization

Example:

| CQI | MCS    |
| --- | ------ |
| 4   | QPSK   |
| 8   | 16QAM  |
| 12  | 64QAM  |
| 15  | 256QAM |

Workflow:

```mermaid
flowchart TD

RIS

SINR

CQI

Scheduler

MCS

Throughput

RIS --> SINR

SINR --> CQI

CQI --> Scheduler

Scheduler --> MCS

MCS --> Throughput
```

---

# 16. KPM-to-RC Mapping

| KPM Observation | xApp Action             |
| --------------- | ----------------------- |
| CQI low         | RIS Optimization        |
| Throughput low  | RIS Optimization        |
| Delay high      | Low-latency RIS profile |
| PRB usage high  | Scheduler optimization  |
| SINR poor       | Beam reconfiguration    |
| KPI improving   | Keep current state      |

---

# 17. Closed-Loop Control Framework

```mermaid
flowchart TD

RIS["RIS Configuration"]

Channel["Channel Quality"]

UE["UE Traffic"]

KPM["KPM Reports"]

RIC["Near-RT RIC"]

xApp["RIS-Aware xApp"]

Control["RC Control"]

RIS --> Channel

Channel --> UE

UE --> KPM

KPM --> RIC

RIC --> xApp

xApp --> Control

Control --> RIS
```

---

# 18. Digital Twin Deployment

Before real RIS hardware:

```mermaid
flowchart TD

Synthetic["Synthetic KPIs"]

xApp["RIS-Aware xApp"]

Decision["RIS Decision"]

Scheduler["Virtual Scheduler"]

Synthetic --> xApp

xApp --> Decision

Decision --> Scheduler
```

Advantages:

* Safe testing
* Fast iteration
* No hardware dependency

---

# 19. Future AI Extension

The rule-based decision engine can later be replaced by AI.

```mermaid
flowchart TD

KPM

Dataset

ML["ML Model"]

Prediction

RISControl

KPM --> Dataset

Dataset --> ML

ML --> Prediction

Prediction --> RISControl
```

Potential AI methods:

* Random Forest
* XGBoost
* Deep Learning
* Reinforcement Learning

---

# 20. Research Implementation Roadmap

```mermaid
flowchart TD

P1["KPM Monitoring"]

P2["KPM Analytics"]

P3["RIS Decision Logic"]

P4["E2SM-RC Integration"]

P5["Scheduler Control"]

P6["AI-Driven RIS Optimization"]

P1 --> P2

P2 --> P3

P3 --> P4

P4 --> P5

P5 --> P6
```

---

# 21. Expected Benefits

The RIS-Aware xApp is expected to provide:

* Improved CQI
* Higher throughput
* Reduced delay
* Better PRB utilization
* Better spectral efficiency
* Adaptive scheduler behavior
* Autonomous radio optimization

---

# 22. Final Architecture

```mermaid
flowchart TD

RIS["RIS Surface"]

CQI["CQI Improvement"]

KPM["KPM Metrics"]

RIC["Near-RT RIC"]

XAPP["RIS-Aware xApp"]

RC["E2SM-RC Control"]

Scheduler["MAC Scheduler"]

Throughput["Higher Throughput"]

RIS --> CQI

CQI --> KPM

KPM --> RIC

RIC --> XAPP

XAPP --> RC

RC --> Scheduler

Scheduler --> Throughput
```

---

# 23. Conclusion

The RIS-Aware xApp represents the first practical step toward integrating programmable radio environments into the O-RAN ecosystem. By combining E2SM-KPM monitoring with E2SM-RC control, the xApp can create a closed-loop optimization framework where RIS configurations are continuously adapted based on real network measurements.

This architecture forms the foundation for future AI-driven RIS control systems and provides a clear path toward 6G intelligent radio environments.
