# RIS-KPM Mapping for RIS-Assisted O-RAN Networks

## Author

Mrigank Jaiswal

## Internship

COMET Foundation – IIIT Bangalore

## Domain

RIS • O-RAN • Near-RT RIC • E2SM-KPM • FlexRIC • MAC Scheduling • 5G/6G

---

# 1. Objective

This document explains how Reconfigurable Intelligent Surface (RIS) behavior maps to Key Performance Measurement (KPM) metrics in an O-RAN-based 5G/6G network.

The purpose is to connect:

* RIS configuration
* Radio channel improvement
* CQI improvement
* KPM metric changes
* Near-RT RIC analytics
* RIS-aware xApp decisions
* MAC scheduler optimization

This document supports the research pipeline:

```mermaid
flowchart TD

RIS["RIS Configuration"]
Channel["Radio Channel Improvement"]
CQI["CQI Improvement"]
KPM["KPM Metrics"]
RIC["Near-RT RIC"]
xApp["RIS-Aware xApp"]
MAC["MAC Scheduler Decision"]
Throughput["Improved Throughput"]

RIS --> Channel
Channel --> CQI
CQI --> KPM
KPM --> RIC
RIC --> xApp
xApp --> MAC
MAC --> Throughput
```

---

# 2. Background

RIS is a programmable wireless surface that can modify the propagation behavior of radio waves. By changing the phase response of its reflecting elements, RIS can improve signal quality between the gNB and UE.

O-RAN provides a programmable RAN control framework where the Near-RT RIC collects network measurements through E2SM-KPM and executes optimization logic through xApps.

In this research direction, RIS is treated as a controllable radio environment component. Its effect is evaluated through KPM metrics collected by the Near-RT RIC.

---

# 3. Why RIS Needs KPM Feedback

RIS cannot be optimized blindly. It needs feedback from the network.

Without KPM feedback:

```mermaid
flowchart TD

RIS["RIS Configuration"]
Unknown["Unknown Network Impact"]
NoDecision["No Reliable Decision"]

RIS --> Unknown
Unknown --> NoDecision
```

With KPM feedback:

```mermaid
flowchart TD

RIS["RIS Configuration"]
Metrics["KPM Metrics"]
Analysis["xApp Analysis"]
Decision["Better RIS Decision"]

RIS --> Metrics
Metrics --> Analysis
Analysis --> Decision
```

KPM reports allow the RIS-aware xApp to understand whether a RIS configuration improves or degrades the network.

---

# 4. Core RIS-to-KPM Relationship

RIS primarily affects the physical wireless channel.

```mermaid
flowchart TD

RIS["RIS Phase / Beam Configuration"]
SNR["SNR Improvement"]
SINR["SINR Improvement"]
CQI["CQI Improvement"]
MCS["Higher MCS"]
Throughput["Throughput Increase"]
Delay["Delay Reduction"]
PRB["PRB Efficiency Improvement"]

RIS --> SNR
RIS --> SINR
SNR --> CQI
SINR --> CQI
CQI --> MCS
MCS --> Throughput
MCS --> PRB
Throughput --> Delay
```

---

# 5. Main KPM Metrics Affected by RIS

| RIS Effect                      | KPM Metric              | Expected Change |
| ------------------------------- | ----------------------- | --------------- |
| Better signal strength          | CQI                     | Increase        |
| Better link quality             | DRB.UEThpDl             | Increase        |
| Better uplink reflection        | DRB.UEThpUl             | Increase        |
| Fewer retransmissions           | DRB.RlcSduDelayDl       | Decrease        |
| More successful data transfer   | DRB.PdcpSduVolumeDL     | Increase        |
| More successful uplink transfer | DRB.PdcpSduVolumeUL     | Increase        |
| Higher spectral efficiency      | RRU.PrbTotDl efficiency | Improve         |
| Better uplink scheduling        | RRU.PrbTotUl efficiency | Improve         |

---

# 6. Metric 1: CQI Mapping

CQI means Channel Quality Indicator.

RIS improves propagation quality, which can increase CQI.

```mermaid
flowchart TD

NoRIS["Without RIS"]
LowSINR["Low SINR"]
LowCQI["Low CQI"]
LowMCS["Low MCS"]

WithRIS["With RIS"]
HighSINR["Higher SINR"]
HighCQI["Higher CQI"]
HighMCS["Higher MCS"]

NoRIS --> LowSINR
LowSINR --> LowCQI
LowCQI --> LowMCS

WithRIS --> HighSINR
HighSINR --> HighCQI
HighCQI --> HighMCS
```

Example:

| Scenario    |  SINR | CQI | MCS |
| ----------- | ----: | --: | --: |
| Without RIS |  8 dB |   5 |   6 |
| With RIS    | 15 dB |  11 |  18 |

---

# 7. Metric 2: Downlink Throughput Mapping

KPM Metric:

```text
DRB.UEThpDl
```

RIS improves downlink link quality.

```mermaid
flowchart TD

RIS["RIS Reflection"]
BetterDL["Better Downlink Channel"]
HigherMCS["Higher MCS"]
MoreBits["More bits per PRB"]
DLThroughput["DRB.UEThpDl Increase"]

RIS --> BetterDL
BetterDL --> HigherMCS
HigherMCS --> MoreBits
MoreBits --> DLThroughput
```

Expected behavior:

```text
If RIS configuration improves the gNB-to-UE channel,
then DRB.UEThpDl should increase.
```

---

# 8. Metric 3: Uplink Throughput Mapping

KPM Metric:

```text
DRB.UEThpUl
```

RIS can assist uplink by reflecting UE signals toward the gNB.

```mermaid
flowchart TD

UE["UE Transmission"]
RIS["RIS-Assisted Reflection"]
gNB["gNB Reception"]
ULQuality["Improved Uplink Quality"]
ULThroughput["DRB.UEThpUl Increase"]

UE --> RIS
RIS --> gNB
gNB --> ULQuality
ULQuality --> ULThroughput
```

Expected behavior:

```text
If RIS improves the UE-to-gNB path,
then DRB.UEThpUl should increase.
```

---

# 9. Metric 4: RLC Delay Mapping

KPM Metric:

```text
DRB.RlcSduDelayDl
```

RIS can reduce delay by improving link reliability and reducing retransmissions.

```mermaid
flowchart TD

PoorChannel["Poor Channel"]
Errors["Transmission Errors"]
Retrans["Retransmissions"]
HighDelay["Higher RLC Delay"]

RIS["RIS Improvement"]
Reliable["Reliable Channel"]
FewerRetrans["Fewer Retransmissions"]
LowDelay["Lower RLC Delay"]

PoorChannel --> Errors
Errors --> Retrans
Retrans --> HighDelay

RIS --> Reliable
Reliable --> FewerRetrans
FewerRetrans --> LowDelay
```

Expected behavior:

```text
Better RIS configuration should reduce DRB.RlcSduDelayDl.
```

---

# 10. Metric 5: PDCP Volume Mapping

KPM Metrics:

```text
DRB.PdcpSduVolumeDL
DRB.PdcpSduVolumeUL
```

RIS can increase the amount of successfully transmitted data.

```mermaid
flowchart TD

RIS["RIS-Assisted Link"]
ReliableLink["More Reliable Link"]
SuccessfulData["More Successful Data Delivery"]
PDCPVolume["Higher PDCP SDU Volume"]

RIS --> ReliableLink
ReliableLink --> SuccessfulData
SuccessfulData --> PDCPVolume
```

Expected behavior:

```text
If RIS improves link stability,
then PDCP SDU volume should increase over the same time window.
```

---

# 11. Metric 6: PRB Efficiency Mapping

KPM Metrics:

```text
RRU.PrbTotDl
RRU.PrbTotUl
```

RIS may not always reduce total PRB usage directly. Instead, it improves PRB efficiency.

```mermaid
flowchart TD

CQI["Higher CQI"]
MCS["Higher MCS"]
Bits["More bits per PRB"]
Efficiency["Higher PRB Efficiency"]

CQI --> MCS
MCS --> Bits
Bits --> Efficiency
```

Interpretation:

```text
For the same traffic demand:
Better RIS configuration can achieve higher throughput using equal or fewer PRBs.
```

---

# 12. RIS-KPM Mapping Table

| RIS-Controlled Effect             | Physical Impact           | KPM Observation              | xApp Interpretation          |
| --------------------------------- | ------------------------- | ---------------------------- | ---------------------------- |
| Phase profile improves reflection | SINR increases            | CQI increases                | Keep current RIS state       |
| Beam direction improves UE link   | Downlink quality improves | DRB.UEThpDl increases        | Mark beam as useful          |
| Reflection improves uplink path   | Uplink quality improves   | DRB.UEThpUl increases        | Improve uplink RIS profile   |
| Fewer radio errors                | Retransmissions reduce    | RLC delay decreases          | Link is more stable          |
| Better spectral efficiency        | More bits per PRB         | Throughput per PRB increases | Scheduler can use higher MCS |
| Poor RIS direction                | Channel weakens           | Throughput decreases         | Switch RIS state             |
| Blocked path remains weak         | CQI remains low           | Low throughput / high delay  | Try alternate RIS beam       |

---

# 13. RIS-Aware xApp Decision Logic

The RIS-aware xApp uses KPM metrics to select RIS configurations.

```mermaid
flowchart TD

KPM["KPM Metrics"]
CQI["CQI Check"]
Throughput["Throughput Check"]
Delay["Delay Check"]
Decision["RIS Decision"]
RISState["Selected RIS State"]

KPM --> CQI
KPM --> Throughput
KPM --> Delay
CQI --> Decision
Throughput --> Decision
Delay --> Decision
Decision --> RISState
```

Example rule-based logic:

```python
if cqi < 6 and throughput < 20:
    ris_state = "Beam_A"

elif cqi >= 6 and cqi < 10:
    ris_state = "Beam_B"

elif cqi >= 10:
    ris_state = "Keep_Current_State"

if rlc_delay > threshold:
    ris_state = "Low_Delay_Profile"
```

---

# 14. RIS Beam Selection Concept

```mermaid
flowchart TD

Start["Start"]
BeamA["Test RIS Beam A"]
MetricA["Collect KPM Metrics A"]
BeamB["Test RIS Beam B"]
MetricB["Collect KPM Metrics B"]
Compare["Compare CQI / Throughput / Delay"]
Select["Select Best RIS Beam"]

Start --> BeamA
BeamA --> MetricA
MetricA --> BeamB
BeamB --> MetricB
MetricB --> Compare
Compare --> Select
```

---

# 15. Closed-Loop RIS-KPM Optimization

```mermaid
flowchart TD

RIS["Apply RIS Configuration"]
Channel["Wireless Channel Changes"]
UE["UE Experiences New Channel"]
gNB["gNB Collects Measurements"]
KPM["E2SM-KPM Report"]
RIC["Near-RT RIC"]
xApp["RIS-Aware xApp"]
Decision["New RIS Decision"]

RIS --> Channel
Channel --> UE
UE --> gNB
gNB --> KPM
KPM --> RIC
RIC --> xApp
xApp --> Decision
Decision --> RIS
```

This creates a complete feedback loop.

---

# 16. Connection with MAC Scheduler

The RIS-aware xApp does not directly replace the MAC scheduler.

Instead:

```mermaid
flowchart TD

RIS["RIS Configuration"]
Channel["Improved Channel"]
CQI["Higher CQI"]
Scheduler["MAC Scheduler"]
MCS["Higher MCS"]
PRB["Better PRB Allocation"]
Throughput["Higher Throughput"]

RIS --> Channel
Channel --> CQI
CQI --> Scheduler
Scheduler --> MCS
Scheduler --> PRB
MCS --> Throughput
PRB --> Throughput
```

RIS improves channel quality, and the scheduler reacts by selecting better MCS and PRB allocation.

---

# 17. Research Prototype Mapping

The prototype can be developed in three levels.

```mermaid
flowchart TD

L1["Level 1: KPM Monitoring"]
L2["Level 2: RIS Decision Logic"]
L3["Level 3: Scheduler-Aware Optimization"]

L1 --> L2
L2 --> L3
```

## Level 1: KPM Monitoring

Collect:

* DRB.UEThpDl
* DRB.UEThpUl
* DRB.RlcSduDelayDl
* RRU.PrbTotDl
* RRU.PrbTotUl

## Level 2: RIS Decision Logic

Use KPM values to choose:

* Beam A
* Beam B
* Beam C
* Low-delay profile
* High-throughput profile

## Level 3: Scheduler-Aware Optimization

Map RIS improvement to:

* Higher MCS
* Better PRB efficiency
* Improved throughput
* Lower latency

---

# 18. Digital Twin Approach

Before using real RIS hardware, the system can be tested using a digital twin.

```mermaid
flowchart TD

Input["Synthetic KPM Metrics"]
Logic["RIS-Aware Decision Logic"]
Output["Selected RIS Beam"]
Expected["Expected KPI Improvement"]

Input --> Logic
Logic --> Output
Output --> Expected
```

This allows algorithm testing without physical RIS hardware.

---

# 19. Expected Experimental Results

| Scenario      |    CQI | Throughput | RLC Delay | PRB Efficiency |
| ------------- | -----: | ---------: | --------: | -------------: |
| Without RIS   |    Low |        Low |      High |            Low |
| Random RIS    | Medium |     Medium |    Medium |         Medium |
| Optimized RIS |   High |       High |       Low |           High |

---

# 20. Final Research Flow

```mermaid
flowchart TD

RIS["RIS"]
CQI["CQI Improvement"]
KPM["KPM Metrics"]
RIC["Near-RT RIC"]
xApp["RIS-Aware xApp"]
MAC["MAC Scheduler Decision"]
Result["Higher Throughput / Better QoS"]

RIS --> CQI
CQI --> KPM
KPM --> RIC
RIC --> xApp
xApp --> MAC
MAC --> Result
```

---

# 21. Mentor Discussion Points

You can explain:

1. RIS modifies the wireless channel.
2. Channel improvement increases SINR and CQI.
3. CQI improvement changes MCS selection.
4. MCS affects throughput and PRB efficiency.
5. KPM metrics provide measurable feedback.
6. Near-RT RIC receives KPM reports through E2SM-KPM.
7. A RIS-aware xApp can select RIS configurations based on KPM trends.
8. The MAC scheduler benefits indirectly from RIS-driven CQI improvement.
9. The first implementation can be a digital twin before hardware integration.
10. This creates a closed-loop RIS-assisted O-RAN optimization framework.

---

# 22. Conclusion

RIS-KPM mapping is the core bridge between programmable radio environments and intelligent O-RAN control. RIS improves the physical channel, and the effect is observed through KPM metrics such as throughput, delay, PDCP volume, and PRB utilization. A RIS-aware xApp can use these metrics to decide whether to keep, modify, or optimize the RIS configuration.

This mapping provides the foundation for the next implementation phase: building a RIS-aware xApp that uses KPM metrics to select RIS beam states and guide MAC scheduler optimization.
