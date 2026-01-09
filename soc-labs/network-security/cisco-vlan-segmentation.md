# Cisco VLAN Segmentation

## Objective
Implement VLAN-based network segmentation to reduce lateral movement
and isolate critical systems within a lab network.

## Environment
- Cisco virtual switch (Packet Tracer / GNS3)
- Three network segments:
  - Admin Network
  - Sales Network
  - IT Network

## Steps
1. Created VLANs for each network segment
2. Assigned switch ports to appropriate VLANs
3. Configured trunk ports to allow inter-VLAN routing
4. Applied access control rules between VLANs

## Security Purpose
Network segmentation limits an attacker’s ability to move freely
across the environment after an initial compromise.

## Outcome
Systems were successfully isolated, and only authorized traffic
was permitted between VLANs.
