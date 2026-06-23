# IOS-MCN Architecture Study Notes

## Overview

The Indian Open Source Mobile Communication Network (IOS-MCN) is an open-source 5G ecosystem that integrates O-RAN, 5G Core, cloud-native deployment, AI-driven optimization, and intelligent network management. It aims to provide a flexible, scalable, and interoperable platform for next-generation mobile communication systems.

---

# High-Level IOS-MCN Architecture

```text
                   +----------------+
                   |      SMO       |
                   +----------------+
                           |
                    Management
                           |
      +---------------------------------------+
      |                Near-RT RIC            |
      +---------------------------------------+
               |                      |
               | E2                   | E2
               ↓                      ↓

+---------------------------------------------------+
|                     O-RAN                         |
|                                                   |
|  O-RU  ←→  O-DU  ←→  O-CU                         |
+---------------------------------------------------+

                     ↓

                    gNB

                     ↓

+---------------------------------------------------+
|                    5G Core                        |
|                                                   |
| AMF | SMF | UPF | NRF | AUSF | UDM               |
+---------------------------------------------------+

                     ↓

                 Internet
```

---

# Service Management and Orchestration (SMO)

## Purpose

The Service Management and Orchestration (SMO) framework provides centralized management of the network.

## Responsibilities

* Network deployment
* Configuration management
* Fault management
* Performance monitoring
* Lifecycle management
* Policy enforcement
* Automation

## Importance

SMO acts as the network operations center and manages all O-RAN and 5G Core components.

---

# Near Real-Time RAN Intelligent Controller (Near-RT RIC)

## Purpose

The Near-RT RIC introduces intelligence into the Radio Access Network.

## Response Time

10 ms to 1 second

## Responsibilities

* Radio resource optimization
* Traffic steering
* Handover optimization
* Beam management
* Load balancing
* QoS optimization

## AI/ML Applications

* User scheduling
* Mobility prediction
* Beam selection
* Interference mitigation

## Key Point

The Near-RT RIC functions as the AI engine of the radio network.

---

# O-RAN Architecture

Open Radio Access Network (O-RAN) disaggregates the traditional base station into separate functional blocks.

## Components

### O-RU (Open Radio Unit)

#### Responsibilities

* RF transmission
* RF reception
* Beamforming
* Antenna management

#### Hardware Components

* Antennas
* Power amplifiers
* RF front-end circuitry

#### Relevance to RIS

RIS and advanced antenna systems directly interact with radio propagation managed by the O-RU.

---

### O-DU (Open Distributed Unit)

#### Responsibilities

* MAC Layer
* RLC Layer
* Part of PHY Layer

#### Functions

* Scheduling
* Resource allocation
* HARQ management
* Radio resource control

#### Key Point

Most MAC-layer functionality resides within the O-DU.

---

### O-CU (Open Centralized Unit)

#### Responsibilities

* PDCP Layer
* SDAP Layer
* RRC Layer

#### Functions

* Mobility control
* Security management
* Session control

#### Key Point

The O-CU manages higher-layer protocol processing.

---

# gNB (5G Base Station)

## Purpose

The gNB serves as the access node connecting users to the 5G Core.

## Responsibilities

* Wireless communication
* Scheduling
* Mobility support
* Beamforming
* Radio resource management

## Interfaces

### N2 Interface

Control-plane communication between gNB and AMF.

### N3 Interface

User-plane communication between gNB and UPF.

---

# 5G Core Network

The 5G Core provides authentication, mobility management, session management, and traffic routing.

---

## Access and Mobility Management Function (AMF)

### Purpose

Handles user registration and mobility management.

### Responsibilities

* Registration management
* Mobility management
* Paging
* Reachability management
* Connection management

### Example

When a smartphone powers on and connects to the network, it first communicates with the AMF.

---

## Session Management Function (SMF)

### Purpose

Controls user data sessions.

### Responsibilities

* PDU session creation
* IP address assignment
* UPF selection
* Session modification
* Session termination

### Example

When a user accesses YouTube or a web service, the SMF establishes the required data session.

---

## User Plane Function (UPF)

### Purpose

Handles user traffic forwarding.

### Responsibilities

* Packet forwarding
* Traffic routing
* QoS enforcement
* Traffic steering

### Example

Video traffic from YouTube is forwarded through the UPF before reaching the user.

---

## Network Repository Function (NRF)

### Purpose

Provides service discovery for network functions.

### Responsibilities

* Service registration
* Service discovery
* Network function lookup

### Key Point

The NRF acts as the service directory of the 5G Core.

---

## Authentication Server Function (AUSF)

### Purpose

Provides subscriber authentication.

### Responsibilities

* Authentication procedures
* Security verification
* Subscriber validation

### Key Point

AUSF ensures that only authorized users gain network access.

---

## Unified Data Management (UDM)

### Purpose

Stores subscriber information.

### Responsibilities

* Subscription management
* Authentication data storage
* User profile storage
* Policy information management

### Key Point

UDM functions as the subscriber database of the 5G network.

---

# MAC Layer

## Position in Protocol Stack

```text
Application
↓
Transport
↓
PDCP
↓
RLC
↓
MAC
↓
PHY
```

## Responsibilities

* Scheduling
* Resource allocation
* HARQ
* Multiplexing
* QoS enforcement

---

## Important MAC Concepts

### Physical Resource Block (PRB)

Basic unit of radio resource allocation.

### Channel Quality Indicator (CQI)

Measures channel quality and influences scheduling decisions.

### Modulation and Coding Scheme (MCS)

Determines transmission rate and reliability.

### Hybrid Automatic Repeat Request (HARQ)

Provides error recovery through retransmissions.

---

# RIS Integration with IOS-MCN

## Purpose

Reconfigurable Intelligent Surfaces improve wireless communication by intelligently controlling reflected radio signals.

## Benefits

* Coverage enhancement
* SNR improvement
* Interference reduction
* Secure communication zones
* Dynamic beam steering

---

## RIS and MAC Relationship

```text
RIS
↓
Improved Channel Quality
↓
Higher SNR
↓
Higher CQI
↓
Better MCS Selection
↓
Improved MAC Scheduling
↓
Higher Throughput
```

---

## RIS Use Cases

### Targeted Coverage

Signal steering toward specific users.

### Secure Zones

Creation of nulls and suppression regions.

### Dynamic Beam Tracking

Continuous beam adaptation for mobile users.

### Multi-User Support

Enhanced communication for multiple simultaneous users.

---

# Key Takeaways

1. SMO manages and orchestrates the entire network.
2. Near-RT RIC introduces AI-driven intelligence into the RAN.
3. O-RAN separates the base station into O-RU, O-DU, and O-CU.
4. AMF handles registration and mobility management.
5. SMF manages user sessions and connectivity.
6. UPF forwards user traffic and enforces QoS.
7. MAC controls scheduling and resource allocation.
8. RIS improves channel quality and overall network performance.
9. IOS-MCN combines open-source 5G technologies with intelligent network control.
10. Understanding the interaction between RIS, MAC, O-RAN, and the 5G Core is essential for advanced 5G and 6G systems.
