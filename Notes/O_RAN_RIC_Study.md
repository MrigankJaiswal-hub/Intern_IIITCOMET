# O-RAN and RIC Study Notes

## 1. Objective

This document explains the Open Radio Access Network architecture and the role of the RAN Intelligent Controller in modern 5G and future 6G networks.

This study connects directly with:

* OAI Core deployment
* UERANSIM gNB and UE deployment
* IOS-MCN research
* MAC scheduler study
* RIS-assisted 5G testbed development
* Future O-RAN and Near-RT RIC integration

---

# 2. Full Forms

| Term        | Full Form                                        |
| ----------- | ------------------------------------------------ |
| O-RAN       | Open Radio Access Network                        |
| RAN         | Radio Access Network                             |
| RIC         | RAN Intelligent Controller                       |
| Near-RT RIC | Near Real-Time RAN Intelligent Controller        |
| Non-RT RIC  | Non Real-Time RAN Intelligent Controller         |
| SMO         | Service Management and Orchestration             |
| O-CU        | Open Centralized Unit                            |
| O-DU        | Open Distributed Unit                            |
| O-RU        | Open Radio Unit                                  |
| E2          | Interface between Near-RT RIC and RAN nodes      |
| A1          | Interface between Non-RT RIC and Near-RT RIC     |
| O1          | Management interface between SMO and O-RAN nodes |
| xApp        | Application running on Near-RT RIC               |
| rApp        | Application running on Non-RT RIC / SMO layer    |
| MAC         | Medium Access Control                            |
| PHY         | Physical Layer                                   |
| RLC         | Radio Link Control                               |
| PDCP        | Packet Data Convergence Protocol                 |
| RRC         | Radio Resource Control                           |
| CQI         | Channel Quality Indicator                        |
| MCS         | Modulation and Coding Scheme                     |
| PRB         | Physical Resource Block                          |
| HARQ        | Hybrid Automatic Repeat Request                  |
| RIS         | Reconfigurable Intelligent Surface               |

---

# 3. Why O-RAN?

Traditional RAN systems are mostly vendor-specific and tightly integrated. O-RAN introduces openness, modularity, interoperability, and intelligence into the radio access network.

Traditional RAN:

```mermaid
flowchart LR
    VendorRAN["Vendor-Specific RAN<br/>Closed Hardware + Closed Software"]
    Core["5G Core"]
    UE["UE"]

    UE --> VendorRAN
    VendorRAN --> Core
```

O-RAN:

```mermaid
flowchart LR
    UE["UE"]
    ORU["O-RU"]
    ODU["O-DU"]
    OCU["O-CU"]
    Core["5G Core"]

    UE --> ORU
    ORU --> ODU
    ODU --> OCU
    OCU --> Core
```

Key benefits:

* Open interfaces
* Multi-vendor interoperability
* Software-defined control
* AI/ML-based optimization
* Programmable RAN behavior
* Better research flexibility

---

# 4. High-Level O-RAN Architecture

```mermaid
flowchart TB
    SMO["SMO<br/>Service Management and Orchestration"]
    NonRT["Non-RT RIC<br/>Policy + AI/ML Training + Long-Term Optimization"]
    NearRT["Near-RT RIC<br/>Near Real-Time Control<br/>xApps"]
    OCU["O-CU<br/>Open Centralized Unit"]
    ODU["O-DU<br/>Open Distributed Unit"]
    ORU["O-RU<br/>Open Radio Unit"]
    UE["UE<br/>User Equipment"]
    Core["5G Core<br/>AMF / SMF / UPF"]

    SMO --> NonRT
    NonRT -->|"A1 Interface"| NearRT
    SMO -->|"O1 Management"| OCU
    SMO -->|"O1 Management"| ODU
    SMO -->|"O1 Management"| ORU

    NearRT -->|"E2 Interface"| OCU
    NearRT -->|"E2 Interface"| ODU

    OCU -->|"F1 Interface"| ODU
    ODU -->|"Open Fronthaul"| ORU
    ORU -->|"Radio Link"| UE
    OCU --> Core
```

---

# 5. O-RAN Functional Split

In O-RAN, a gNB is split into multiple logical blocks.

```mermaid
flowchart TB
    gNB["5G gNB"]

    OCU["O-CU<br/>RRC / PDCP / SDAP"]
    ODU["O-DU<br/>RLC / MAC / High PHY"]
    ORU["O-RU<br/>Low PHY / RF / Antenna"]

    gNB --> OCU
    gNB --> ODU
    gNB --> ORU
```

## O-CU

O-CU handles higher-layer functions:

* RRC
* PDCP
* SDAP
* Mobility control
* Security handling
* Connection to 5G Core

## O-DU

O-DU handles lower-layer real-time functions:

* RLC
* MAC
* HARQ
* PRB allocation
* MCS selection
* Scheduling
* High PHY

## O-RU

O-RU handles radio functions:

* Low PHY
* RF transmission
* RF reception
* Antenna interface
* Beamforming support

---

# 6. Where MAC Layer Exists

The MAC layer mainly resides in the O-DU.

```mermaid
flowchart TB
    OCU["O-CU<br/>RRC / PDCP"]
    ODU["O-DU<br/>RLC / MAC / Scheduler"]
    ORU["O-RU<br/>PHY / RF"]
    UE["UE"]

    OCU --> ODU
    ODU --> ORU
    ORU --> UE
```

MAC layer responsibilities:

* UE scheduling
* PRB allocation
* MCS selection
* HARQ retransmission
* Buffer status handling
* CQI-based resource allocation
* QoS-aware scheduling

---

# 7. What is RIC?

RIC stands for RAN Intelligent Controller.

It introduces intelligence and programmability into the RAN.

There are two main RIC layers:

```mermaid
flowchart TB
    NonRT["Non-RT RIC<br/>> 1 second control loop"]
    NearRT["Near-RT RIC<br/>10 ms to 1 second control loop"]

    NonRT --> NearRT
```

---

# 8. Non-RT RIC

Non-RT RIC operates at a time scale greater than 1 second.

It is usually part of the SMO layer.

Functions:

* Long-term optimization
* AI/ML model training
* Policy generation
* Network analytics
* Traffic prediction
* Energy optimization
* Cell-level planning

Applications running here are called:

```text
rApps
```

Example rApps:

* Traffic prediction rApp
* Energy optimization rApp
* Coverage analysis rApp
* Policy generation rApp

---

# 9. Near-RT RIC

Near-RT RIC operates at a time scale of approximately 10 ms to 1 second.

It controls near real-time RAN behavior.

Functions:

* Traffic steering
* Load balancing
* Mobility optimization
* Interference coordination
* QoS optimization
* Scheduling assistance
* Beam management support

Applications running here are called:

```text
xApps
```

Example xApps:

* Traffic steering xApp
* Handover optimization xApp
* PRB optimization xApp
* Interference management xApp
* QoS control xApp
* Beam management xApp

---

# 10. RIC Control Loop

```mermaid
flowchart TD
    RAN["O-RAN Nodes<br/>O-CU / O-DU"]
    Telemetry["Telemetry<br/>CQI / PRB / Throughput / Latency"]
    RIC["Near-RT RIC"]
    xApp["xApp<br/>Optimization Logic"]
    Control["Control Action<br/>Policy / Scheduling Assistance"]
    RANAction["RAN Behavior Updated"]

    RAN --> Telemetry
    Telemetry --> RIC
    RIC --> xApp
    xApp --> Control
    Control --> RANAction
    RANAction --> RAN
```

---

# 11. O-RAN Interfaces

## A1 Interface

Connects:

```mermaid
flowchart LR
    NonRT["Non-RT RIC"]
    NearRT["Near-RT RIC"]

    NonRT -->|"A1 Interface"| NearRT
```

Purpose:

* Policy transfer
* ML model guidance
* Long-term optimization instructions

## E2 Interface

Connects:

```mermaid
flowchart LR
    NearRT["Near-RT RIC"]
    OCU["O-CU"]
    ODU["O-DU"]

    NearRT -->|"E2 Interface"| OCU
    NearRT -->|"E2 Interface"| ODU
```

Purpose:

* Near real-time telemetry
* Control messages
* xApp interaction with RAN nodes

## O1 Interface

Connects:

```mermaid
flowchart LR
    SMO["SMO"]
    OCU["O-CU"]
    ODU["O-DU"]
    ORU["O-RU"]

    SMO -->|"O1 Interface"| OCU
    SMO -->|"O1 Interface"| ODU
    SMO -->|"O1 Interface"| ORU
```

Purpose:

* Fault management
* Configuration management
* Performance monitoring
* Software lifecycle management

## Open Fronthaul

Connects:

```mermaid
flowchart LR
    ODU["O-DU"]
    ORU["O-RU"]

    ODU -->|"Open Fronthaul"| ORU
```

Purpose:

* Radio sample transport
* Synchronization
* Low PHY data exchange

---

# 12. xApp Architecture

```mermaid
flowchart TB
    xApp["xApp"]
    E2SM["E2 Service Model"]
    E2Term["E2 Termination"]
    NearRT["Near-RT RIC"]
    ODU["O-DU"]
    OCU["O-CU"]

    xApp --> E2SM
    E2SM --> E2Term
    E2Term --> NearRT
    NearRT --> ODU
    NearRT --> OCU
```

xApps use RAN telemetry to make control decisions.

Examples of xApp input:

* CQI
* RSRP
* RSRQ
* SINR
* PRB usage
* UE throughput
* Latency
* Handover statistics

Examples of xApp output:

* Traffic steering decision
* PRB optimization policy
* Mobility recommendation
* Interference control action
* Beam selection guidance

---

# 13. RIC and MAC Scheduler

The MAC scheduler is inside the O-DU. RIC can influence MAC-related behavior using telemetry and control policies.

```mermaid
flowchart TD
    UE["UE"]
    ORU["O-RU"]
    ODU["O-DU<br/>MAC Scheduler"]
    NearRT["Near-RT RIC"]
    xApp["Scheduling / QoS xApp"]

    UE --> ORU
    ORU --> ODU
    ODU -->|"Telemetry<br/>CQI / PRB / Throughput"| NearRT
    NearRT --> xApp
    xApp -->|"Control Policy"| ODU
```

MAC scheduler parameters:

* CQI
* MCS
* PRB allocation
* HARQ feedback
* QoS class
* Buffer status
* UE priority
* Channel condition

---

# 14. RIS Connection with O-RAN and RIC

RIS can improve the physical wireless channel, while RIC can intelligently control or assist optimization decisions.

```mermaid
flowchart LR
    UE["UE"]
    RIS["RIS<br/>Reconfigurable Intelligent Surface"]
    ORU["O-RU"]
    ODU["O-DU<br/>MAC Scheduler"]
    RIC["Near-RT RIC"]
    Core["5G Core"]

    UE --> RIS
    RIS --> ORU
    ORU --> ODU
    ODU --> Core
    ODU -->|"Telemetry"| RIC
    RIC -->|"Optimization Policy"| ODU
```

RIS improves:

* SNR
* CQI
* MCS
* Coverage
* Throughput
* Reliability

---

# 15. RIS-Aware RIC Control Concept

```mermaid
flowchart TD
    UE["UE Measurement"]
    Channel["Channel Quality<br/>SNR / CQI / RSRP"]
    RIC["Near-RT RIC"]
    xApp["RIS Control xApp"]
    RIS["RIS Phase Configuration"]
    MAC["MAC Scheduler"]
    Result["Improved Throughput"]

    UE --> Channel
    Channel --> RIC
    RIC --> xApp
    xApp --> RIS
    RIS --> Channel
    Channel --> MAC
    MAC --> Result
```

This can become a research direction:

```text
RIS-aware RIC-assisted MAC scheduling
```

---

# 16. Relation to Current OAI + UERANSIM Deployment

Current successful setup:

```mermaid
flowchart LR
    UE["UERANSIM UE"]
    GNB["UERANSIM gNB"]
    AMF["OAI AMF"]
    SMF["OAI SMF"]
    UPF["OAI UPF"]
    Internet["Internet"]

    UE --> GNB
    GNB --> AMF
    AMF --> SMF
    SMF --> UPF
    GNB --> UPF
    UPF --> Internet
```

Next evolution:

```mermaid
flowchart LR
    Current["OAI Core + UERANSIM"]
    OAIgNB["OAI gNB"]
    ORAN["O-RAN Split<br/>O-CU / O-DU / O-RU"]
    RIC["Near-RT RIC"]
    RIS["RIS-Assisted Testbed"]

    Current --> OAIgNB
    OAIgNB --> ORAN
    ORAN --> RIC
    RIC --> RIS
```

---

# 17. Practical Study Roadmap

```mermaid
flowchart TD
    A["OAI Core Deployment"]
    B["GNBSIM Test"]
    C["UERANSIM gNB + UE"]
    D["OAI gNB Study"]
    E["O-RAN Split Architecture"]
    F["Near-RT RIC Study"]
    G["xApp Study"]
    H["MAC Scheduler Analysis"]
    I["RIS-Aware Control"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I
```

---

# 18. Mentor Discussion Points

You should be able to explain:

1. O-RAN splits gNB into O-CU, O-DU, and O-RU.
2. MAC scheduler is mainly inside O-DU.
3. Near-RT RIC controls near real-time RAN optimization.
4. xApps run inside Near-RT RIC.
5. Non-RT RIC handles long-term AI/ML policy and rApps.
6. E2 interface connects Near-RT RIC with O-CU/O-DU.
7. O1 interface is used for management and orchestration.
8. A1 interface connects Non-RT RIC with Near-RT RIC.
9. RIS improves channel quality.
10. Better channel quality improves CQI, MCS, and throughput.
11. RIS-aware MAC scheduling can be guided by RIC and xApps.

---

# 19. Key Takeaways

* O-RAN makes the RAN open, modular, and programmable.
* RIC introduces AI/ML-driven control into RAN.
* Near-RT RIC uses xApps for near real-time optimization.
* Non-RT RIC uses rApps for long-term policy and intelligence.
* MAC scheduling is a key control point for performance.
* RIS can improve channel quality before the signal reaches the O-RU.
* A strong research direction is RIS-aware RIC-assisted MAC scheduling.

---

# 20. Conclusion

O-RAN and RIC provide the programmable and intelligent control framework required for advanced 5G and 6G research. After completing OAI Core and UERANSIM deployment, the next logical step is understanding how O-RAN splits the gNB into O-CU, O-DU, and O-RU, and how Near-RT RIC can influence MAC-layer decisions. This directly connects with RIS-assisted communication because RIS improves the radio channel, while RIC and MAC scheduling can use that improved channel quality to enhance throughput, coverage, and reliability.
