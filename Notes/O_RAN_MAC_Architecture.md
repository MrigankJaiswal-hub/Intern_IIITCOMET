# O-RAN MAC Architecture Study Notes

## 1. Objective

This document explains how the MAC layer fits inside the O-RAN architecture and how it connects with IOS-MCN, OAI, RIS-assisted communication, and future 5G/6G testbed work.

---

# 2. Full Forms

| Term        | Full Form                                 |
| ----------- | ----------------------------------------- |
| O-RAN       | Open Radio Access Network                 |
| RAN         | Radio Access Network                      |
| SMO         | Service Management and Orchestration      |
| RIC         | RAN Intelligent Controller                |
| Near-RT RIC | Near Real-Time RAN Intelligent Controller |
| Non-RT RIC  | Non Real-Time RAN Intelligent Controller  |
| O-RU        | Open Radio Unit                           |
| O-DU        | Open Distributed Unit                     |
| O-CU        | Open Centralized Unit                     |
| MAC         | Medium Access Control                     |
| PHY         | Physical Layer                            |
| RLC         | Radio Link Control                        |
| PDCP        | Packet Data Convergence Protocol          |
| SDAP        | Service Data Adaptation Protocol          |
| RRC         | Radio Resource Control                    |
| CQI         | Channel Quality Indicator                 |
| MCS         | Modulation and Coding Scheme              |
| PRB         | Physical Resource Block                   |
| HARQ        | Hybrid Automatic Repeat Request           |
| RIS         | Reconfigurable Intelligent Surface        |
| UE          | User Equipment                            |
| gNB         | Next Generation NodeB / 5G Base Station   |

---

# 3. High-Level O-RAN Architecture

```text
SMO
 ↓
Non-RT RIC
 ↓
Near-RT RIC
 ↓
O-CU
 ↓
O-DU
 ↓
O-RU
 ↓
UE
```

In a real 5G testbed:

```text
5G Core
 ↑
O-CU
 ↑
O-DU
 ↑
O-RU
 ↑
UE
```

---

# 4. O-RAN Architecture

```mermaid
flowchart TB

SMO["SMO<br/>Service Management and Orchestration"]

NONRT["Non-RT RIC<br/>Non Real-Time RAN Intelligent Controller"]

NEARRT["Near-RT RIC<br/>Near Real-Time RAN Intelligent Controller"]

subgraph ORAN["O-RAN / gNB"]
    OCU["O-CU<br/>Open Centralized Unit"]
    ODU["O-DU<br/>Open Distributed Unit"]
    ORU["O-RU<br/>Open Radio Unit"]
end

CORE["5G Core<br/>AMF / SMF / UPF"]

UE["UE<br/>User Equipment"]

SMO --> NONRT
NONRT --> NEARRT
NEARRT -->|"E2 Interface"| ODU
NEARRT -->|"E2 Interface"| OCU

OCU -->|"F1 Interface"| ODU
ODU -->|"Open Fronthaul"| ORU
ORU -->|"Radio Link"| UE

OCU -->|"N2 / N3"| CORE
```

---

# 5. Where MAC Layer Exists

The MAC layer mainly resides inside the:

```text
O-DU
```

O-DU handles lower-layer real-time radio processing.

Protocol placement:

```text
O-CU:
RRC
PDCP
SDAP

O-DU:
RLC
MAC
Part of PHY

O-RU:
RF
Low PHY
Antenna
Beamforming
```

---

# 6. O-CU, O-DU, O-RU Responsibilities

## 6.1 O-CU: Open Centralized Unit

O-CU handles higher-layer control and user-plane functions.

Main responsibilities:

* RRC signaling
* PDCP processing
* SDAP processing
* Mobility control
* Security management
* Session-related radio control

O-CU connects to:

* 5G Core using N2/N3 interfaces
* O-DU using F1 interface

---

## 6.2 O-DU: Open Distributed Unit

O-DU handles time-sensitive radio functions.

Main responsibilities:

* MAC scheduling
* RLC processing
* HARQ coordination
* PRB allocation
* MCS selection
* CQI handling
* Real-time radio resource management

This is the most important block for MAC-layer study.

---

## 6.3 O-RU: Open Radio Unit

O-RU handles radio transmission and reception.

Main responsibilities:

* RF signal transmission
* RF signal reception
* Antenna interface
* Low PHY processing
* Beamforming support
* RF front-end control

RIS interacts most directly with the O-RU because RIS changes the radio propagation channel.

---

# 7. O-RAN Interfaces

## 7.1 E2 Interface

Connects:

```text
Near-RT RIC ↔ O-DU / O-CU
```

Purpose:

* Near real-time optimization
* Control messages
* Radio performance monitoring
* xApp-based optimization

Example:

A Near-RT RIC xApp may monitor PRB usage, CQI, and throughput from the O-DU and suggest scheduling improvements.

---

## 7.2 F1 Interface

Connects:

```text
O-CU ↔ O-DU
```

Purpose:

* Control-plane and user-plane communication between CU and DU
* Split architecture support

---

## 7.3 Open Fronthaul Interface

Connects:

```text
O-DU ↔ O-RU
```

Purpose:

* Transport radio data
* Synchronization
* Low PHY interaction
* IQ sample transfer depending on functional split

---

# 8. MAC Layer in O-DU

MAC = Medium Access Control

The MAC layer answers these questions:

```text
Who should transmit?
When should they transmit?
How many PRBs should they receive?
Which MCS should be used?
Should retransmission happen?
```

MAC inputs:

* CQI reports
* Buffer status reports
* QoS requirements
* HARQ feedback
* Scheduling requests
* UE priority
* Channel condition

MAC outputs:

* PRB allocation
* MCS decision
* HARQ retransmission decision
* Uplink/downlink grants

---

# 9. MAC Scheduler Flow

```mermaid
flowchart TD

A["UE reports CQI"] --> B["O-DU receives channel information"]

C["UE buffer status"] --> D["MAC Scheduler"]

B --> D

E["QoS requirements"] --> D

F["HARQ feedback"] --> D

D --> G["Select UE"]

G --> H["Allocate PRBs"]

H --> I["Select MCS"]

I --> J["Send scheduling grant"]

J --> K["PHY transmission"]

K --> L["ACK / NACK"]

L --> F
```

---

# 10. CQI, MCS, PRB and HARQ in O-RAN MAC

## 10.1 CQI: Channel Quality Indicator

CQI tells the scheduler how good the radio channel is.

High CQI:

* Better channel
* Higher MCS possible
* Higher throughput

Low CQI:

* Poor channel
* Lower MCS needed
* Lower throughput

---

## 10.2 MCS: Modulation and Coding Scheme

MCS determines how much data can be sent per radio resource.

Low MCS:

* Robust
* Low throughput
* Suitable for poor channel

High MCS:

* Fast
* High throughput
* Requires good channel

---

## 10.3 PRB: Physical Resource Block

PRB is the schedulable frequency-time resource block.

Scheduler allocates PRBs to users.

More PRBs usually means more throughput.

But actual throughput depends on both:

```text
Number of PRBs + MCS level
```

---

## 10.4 HARQ: Hybrid Automatic Repeat Request

HARQ provides reliability using retransmission.

If a packet fails:

```text
NACK
 ↓
Retransmission
```

If packet succeeds:

```text
ACK
 ↓
Continue transmission
```

---

# 11. Near-RT RIC and MAC Optimization

Near-RT RIC can influence MAC-related decisions using xApps.

Examples of xApps:

* Traffic steering xApp
* Load balancing xApp
* Handover optimization xApp
* QoS optimization xApp
* Beam management xApp

Near-RT RIC collects telemetry from the RAN and can optimize:

* PRB usage
* User scheduling
* Load balancing
* Mobility handling
* Beam selection
* Interference control

---

# 12. RIS-Assisted MAC Scheduling

RIS = Reconfigurable Intelligent Surface

RIS improves the wireless channel by steering or shaping radio waves.

Without RIS:

```text
Poor channel
 ↓
Low SNR
 ↓
Low CQI
 ↓
Low MCS
 ↓
Low throughput
```

With RIS:

```text
RIS beam steering
 ↓
Higher SNR
 ↓
Higher CQI
 ↓
Higher MCS
 ↓
More efficient PRB usage
 ↓
Higher throughput
```

---

# 13. RIS to MAC Improvement Chain

```mermaid
flowchart TD

RIS["RIS<br/>Reconfigurable Intelligent Surface"]

SNR["Improved SNR<br/>Signal-to-Noise Ratio"]

CQI["Higher CQI<br/>Channel Quality Indicator"]

MCS["Higher MCS<br/>Modulation and Coding Scheme"]

PRB["Better PRB Utilization<br/>Physical Resource Blocks"]

MAC["MAC Scheduler<br/>Medium Access Control"]

THR["Higher Throughput"]

RIS --> SNR
SNR --> CQI
CQI --> MCS
MCS --> PRB
PRB --> MAC
MAC --> THR
```

---

# 14. RIS + O-RAN + MAC Architecture

```text
UE
 ↓
RIS
 ↓
O-RU
 ↓
O-DU
 ↓
MAC Scheduler
 ↓
O-CU
 ↓
5G Core
 ↓
Internet
```

In this architecture:

* RIS improves the physical radio channel.
* O-RU receives improved signal quality.
* O-DU processes CQI and scheduling.
* MAC scheduler selects MCS and PRB allocation.
* O-CU connects the RAN to the 5G Core.
* 5G Core provides session and Internet connectivity.

---

# 15. RIS-Assisted O-RAN MAC Architecture

```mermaid
flowchart LR

UE["UE<br/>User Equipment"]

RIS["RIS<br/>Reconfigurable Intelligent Surface"]

ORU["O-RU<br/>Open Radio Unit"]

ODU["O-DU<br/>Open Distributed Unit<br/>MAC Scheduler"]

OCU["O-CU<br/>Open Centralized Unit"]

CORE["5G Core<br/>AMF / SMF / UPF"]

INTERNET["Internet"]

UE --> RIS
RIS --> ORU
ORU --> ODU
ODU --> OCU
OCU --> CORE
CORE --> INTERNET
```

---

# 16. How This Connects to the RIS Pilot Project

The RIS pilot includes:

* Targeted coverage
* Secure zones
* Dynamic beam control
* UGV communication
* IoT coverage
* 5G waveform demonstration

MAC-layer relevance:

| RIS Function          | MAC Impact                 |
| --------------------- | -------------------------- |
| Coverage enhancement  | Higher CQI                 |
| Beam steering         | Better MCS                 |
| Null steering         | Reduced interference       |
| Secure zones          | Controlled access          |
| Dynamic beam tracking | Stable scheduling          |
| Multi-user RIS        | Better resource allocation |

---

# 17. Research Direction: RIS-Aware MAC Scheduler

A possible research contribution:

```text
RIS-aware MAC scheduler
```

Inputs:

* UE position
* CQI
* SNR
* RIS phase state
* Traffic demand
* QoS class

Outputs:

* UE scheduling decision
* PRB allocation
* MCS selection
* RIS beam direction

Goal:

Maximize throughput while maintaining coverage and security.

---

# 18. Mentor Discussion Points

You can explain:

1. MAC layer resides inside the O-DU.
2. MAC scheduler controls PRB allocation and MCS selection.
3. CQI is a key input for MAC scheduling.
4. RIS improves SNR, which improves CQI.
5. Higher CQI allows higher MCS.
6. Better MCS improves throughput.
7. Near-RT RIC can potentially control or assist MAC optimization.
8. RIS-aware scheduling can become a research contribution.

---

# 19. Practical Next Steps

For OAI and IOS-MCN study:

1. Identify where MAC scheduler code exists in OAI.
2. Study CQI to MCS mapping.
3. Study PRB allocation logic.
4. Study HARQ feedback processing.
5. Connect logs from gNB/UE to MAC-layer behavior.
6. Later integrate RIS channel gain as an input to scheduling simulation.

---

# 20. Key Takeaways

* O-RAN separates gNB into O-RU, O-DU, and O-CU.
* MAC layer is mainly inside O-DU.
* MAC controls scheduling, PRB allocation, MCS selection, and HARQ.
* Near-RT RIC can support intelligent MAC optimization.
* RIS improves the physical channel.
* Better physical channel improves CQI.
* Better CQI improves MCS.
* Better MCS improves throughput.
* RIS-assisted MAC scheduling is a strong research direction for 5G/6G.
