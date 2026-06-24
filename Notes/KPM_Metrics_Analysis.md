# KPM Metrics Analysis for RIS-Assisted O-RAN Networks

## Author

Mrigank Jaiswal

## Internship

COMET Foundation – IIIT Bangalore

## Domain

O-RAN • Near-RT RIC • FlexRIC • RIS-Assisted Wireless Networks • 5G/6G

---

# 1. Introduction

The O-RAN architecture enables intelligent and programmable Radio Access Networks (RANs) through the Near-Real-Time RAN Intelligent Controller (Near-RT RIC). One of the most important Service Models defined by O-RAN is the E2SM-KPM (Key Performance Measurement) Service Model.

KPM provides real-time radio performance measurements from the RAN to the Near-RT RIC through the E2 interface. These measurements are consumed by xApps for monitoring, analytics, optimization, and AI-driven decision making.

For RIS-assisted wireless systems, KPM metrics become especially important because RIS directly affects radio propagation conditions. Any improvement in channel quality introduced by RIS is reflected through measurable changes in throughput, delay, spectral efficiency, and resource utilization.

---

# 2. KPM Service Model Architecture

```mermaid
flowchart TD

UE[User Equipment]
gNB[gNB / O-DU]
Agent[E2 Agent]
E2[E2 Interface]
RIC[Near-RT RIC]
KPM[KPM xApp]
AI[AI Analytics Engine]

UE --> gNB
gNB --> Agent
Agent --> E2
E2 --> RIC
RIC --> KPM
KPM --> AI
```

The KPM Service Model continuously exports radio performance metrics from the RAN to the Near-RT RIC.

---

# 3. KPM Data Collection Process

```mermaid
sequenceDiagram

participant UE
participant gNB
participant Agent
participant RIC
participant xApp

UE->>gNB: Data Transmission
gNB->>Agent: Generate Measurements
Agent->>RIC: E2 Indication Message
RIC->>xApp: Forward KPM Report
xApp->>xApp: Analytics Processing
```

---

# 4. KPM Metrics Observed in FlexRIC

During FlexRIC experimentation, the following metrics were successfully collected.

## Throughput Metrics

```text
DRB.UEThpDl
DRB.UEThpUl
```

## Delay Metrics

```text
DRB.RlcSduDelayDl
```

## Volume Metrics

```text
DRB.PdcpSduVolumeDL
DRB.PdcpSduVolumeUL
```

## Resource Metrics

```text
RRU.PrbTotDl
RRU.PrbTotUl
```

These metrics form the primary inputs for future RIS-aware xApps.

---

# 5. Throughput Analysis

## DRB.UEThpDl

Downlink User Throughput

Measures:

* User data rate
* Scheduler efficiency
* Radio channel quality

Observed Values:

```text
DRB.UEThpDl = 5.24 kbps
DRB.UEThpDl = 6.30 kbps
DRB.UEThpDl = 7.06 kbps
```

### Relationship with RIS

```mermaid
flowchart TD

RIS[Reconfigurable Intelligent Surface]
SINR[SINR Increase]
CQI[CQI Increase]
MCS[Higher MCS]
THP[Higher Throughput]

RIS --> SINR
SINR --> CQI
CQI --> MCS
MCS --> THP
```

Expected Result:

```text
DRB.UEThpDl ↑
```

---

## DRB.UEThpUl

Uplink User Throughput

Measures:

* UE transmission efficiency
* Uplink radio quality
* Resource utilization

Observed Values:

```text
DRB.UEThpUl = 5.36 kbps
DRB.UEThpUl = 6.87 kbps
```

RIS can improve uplink propagation by reflecting signals toward the base station.

Expected Effect:

```text
DRB.UEThpUl ↑
```

---

# 6. PDCP Volume Analysis

## DRB.PdcpSduVolumeDL

Measures:

Total downlink PDCP payload successfully delivered.

Observed Example:

```text
1021 kb
971 kb
678 kb
```

### Impact of RIS

```mermaid
flowchart TD

RIS --> Channel[Improved Channel Quality]
Channel --> Retrans[Fewer Retransmissions]
Retrans --> Volume[More Successful Delivery]
Volume --> PDCP[Higher PDCP Volume]
```

Expected Effect:

```text
PDCP Volume DL ↑
```

---

## DRB.PdcpSduVolumeUL

Measures:

Total uplink payload delivered.

Observed Example:

```text
510 kb
446 kb
306 kb
```

Expected Effect:

```text
PDCP Volume UL ↑
```

---

# 7. RLC Delay Analysis

## DRB.RlcSduDelayDl

Measures:

Average RLC delay.

Observed Values:

```text
5.26 µs
5.49 µs
7.33 µs
```

### Without RIS

```mermaid
flowchart TD

Weak[Weak Signal]
Retrans[Retransmissions]
Queue[Queue Build-up]
Delay[Higher Delay]

Weak --> Retrans
Retrans --> Queue
Queue --> Delay
```

### With RIS

```mermaid
flowchart TD

RIS --> Signal[Stronger Signal]
Signal --> Fewer[Fewer Retransmissions]
Fewer --> Fast[Fast Delivery]
Fast --> DelayLow[Lower Delay]
```

Expected Effect:

```text
RLC Delay ↓
```

---

# 8. PRB Utilization Analysis

## RRU.PrbTotDl

Measures:

Total downlink Physical Resource Blocks.

Observed Examples:

```text
652 PRBs
859 PRBs
282 PRBs
```

### RIS Impact

```mermaid
flowchart TD

RIS --> CQI
CQI --> MCS
MCS --> Spectral[Spectral Efficiency]
Spectral --> PRB[Fewer PRBs Needed]
```

Expected Effect:

```text
PRB Efficiency ↑
```

---

## RRU.PrbTotUl

Measures:

Total uplink PRBs.

Observed Examples:

```text
382 PRBs
597 PRBs
420 PRBs
```

Expected Effect:

```text
Resource Efficiency ↑
```

---

# 9. RIS and CQI Relationship

RIS primarily affects the radio channel.

```mermaid
flowchart LR

RIS --> Gain[Signal Gain]
Gain --> SINR[SINR Increase]
SINR --> CQI[CQI Increase]
CQI --> MCS[Higher MCS]
MCS --> Throughput[Higher Throughput]
```

Example:

| Scenario    | SINR  | CQI |
| ----------- | ----- | --- |
| Without RIS | 8 dB  | 5   |
| With RIS    | 15 dB | 11  |

---

# 10. KPM Metrics Used by RIS-Aware xApps

```mermaid
flowchart TD

KPM[KPM Reports]
CQI[CQI Analysis]
AI[AI Decision Engine]
RIS[RIS Configuration]
Scheduler[MAC Scheduler]

KPM --> CQI
CQI --> AI
AI --> RIS
RIS --> Scheduler
```

The xApp receives KPM reports and decides whether RIS reconfiguration is required.

---

# 11. RIS-Aware Optimization Loop

```mermaid
flowchart TD

Monitor[Monitor KPM Metrics]
Analyze[Analyze CQI and Throughput]
Predict[Predict RIS Gain]
Configure[Apply RIS Configuration]
Improve[Improve Radio Channel]
Feedback[Collect New KPM Reports]

Monitor --> Analyze
Analyze --> Predict
Predict --> Configure
Configure --> Improve
Improve --> Feedback
Feedback --> Monitor
```

This forms a closed-loop O-RAN optimization framework.

---

# 12. Integration with MAC Scheduler

The MAC Scheduler does not directly control RIS.

Instead RIS indirectly influences scheduling decisions.

```mermaid
flowchart TD

RIS
CQI
Scheduler[MAC Scheduler]
MCS[Selected MCS]
PRB[PRB Allocation]
THP[Network Throughput]

RIS --> CQI
CQI --> Scheduler
Scheduler --> MCS
Scheduler --> PRB
MCS --> THP
PRB --> THP
```

---

# 13. End-to-End RIS-Aware O-RAN Architecture

```mermaid
flowchart TD

UE
gNB
Agent[E2 Agent]
RIC[Near-RT RIC]
KPM[KPM xApp]
RISxApp[RIS-Aware xApp]
RIS
MAC[MAC Scheduler]

UE --> gNB
gNB --> Agent
Agent --> RIC
RIC --> KPM
KPM --> RISxApp
RISxApp --> RIS
RIS --> MAC
MAC --> UE
```

---

# 14. Key Findings

The FlexRIC KPM xApp successfully collected real-time O-RAN measurements through the E2 interface.

Important observations:

* KPM metrics can monitor radio performance in real time.
* Throughput metrics increase when channel quality improves.
* Delay metrics decrease when retransmissions are reduced.
* PRB utilization becomes more efficient under better CQI conditions.
* RIS effects can be observed indirectly through KPM reports.
* KPM metrics provide the primary feedback mechanism for RIS-aware xApps.

---

# 15. Future Work

The next phase of research will implement a RIS-Aware xApp capable of:

1. Collecting KPM reports.
2. Estimating CQI trends.
3. Predicting RIS gains.
4. Selecting RIS beam configurations.
5. Triggering adaptive MAC scheduling decisions.

Target Workflow:

```mermaid
flowchart TD

RIS
CQI[CQI Improvement]
KPM[KPM Metrics]
RIC[NearRT-RIC]
XAPP[RIS-Aware xApp]
MAC[MAC Scheduler Decision]

RIS --> CQI
CQI --> KPM
KPM --> RIC
RIC --> XAPP
XAPP --> MAC
```

This workflow represents the core objective of RIS-assisted O-RAN optimization.
