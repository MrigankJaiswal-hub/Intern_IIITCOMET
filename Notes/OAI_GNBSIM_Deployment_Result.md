# OAI 5G Core + GNBSIM Deployment Result

## Status
Successfully deployed minimal OAI 5G Core and GNBSIM.

## Containers
- mysql
- oai-amf
- oai-smf
- oai-upf
- oai-ext-dn
- gnbsim

## Successful Result
- OAI Core containers are healthy.
- GNBSIM started successfully.
- PDU Session Establishment Accept received.
- UE received IP address: 12.1.1.3.

## Meaning
This confirms simulated UE registration and PDU Session Establishment with OAI 5G Core.

## Next Step
Validate user-plane traffic using ping/iperf and study AMF, SMF, UPF logs.