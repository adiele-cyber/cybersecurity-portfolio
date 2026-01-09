# Cisco VLAN & Access Control Policy

---

## Objective
Implement VLAN segmentation and access-control rules to enforce
least-privilege network access and protect sensitive business systems.

---

## Network Segments

| VLAN | Department |
|------|-----------|
| VLAN 10 | Administration |
| VLAN 20 | Sales |
| VLAN 30 | IT |

Each VLAN represents a separate security zone.

---

## Access Control Policy

| Source → Destination | VLANs | Purpose | Security Rationale |
|---------------------|--------|---------|------------------|
Admin → Sales | 10 → 20 | Allow reporting access | Enables controlled access to sales data without exposing sensitive admin systems |
Admin → IT | 10 → 30 | Denied | Prevents unnecessary access to IT infrastructure |
Sales → Admin | 20 → 10 | Denied | Protects HR, payroll, and executive data from exposure |
Sales → IT | 20 → 30 | Allowed | Allows technical support and troubleshooting |
IT → All VLANs | 30 → All | Allowed | Required for system administration, updates, and security monitoring |
Management access | Network devices | SSH only | Prevents credential sniffing and unauthorized access via Telnet |

---

## Security Architecture

This VLAN and ACL design implements:

- **Network segmentation**
- **Least privilege**
- **Controlled lateral movement**
- **Secure administrative access**

Departments can only reach what they are required to access.

---

## Security Risk Analysis

If VLANs or ACLs are misconfigured, the following risks arise:

| Risk | Impact |
|------|-------|
Sales accessing Admin VLAN | Exposure of payroll, HR, and executive data |
Admin accessing IT systems | Increased attack surface for critical infrastructure |
Telnet enabled | Credentials can be captured via network sniffing |
No inter-VLAN routing | Departments become isolated and business operations fail |

---

## SOC Value

This configuration provides:

- Reduced lateral movement after compromise
- Controlled data access
- Clear trust boundaries between departments
- Improved visibility and security enforcement

This is a foundational defensive control in enterprise networks and a
critical layer in defense-in-depth security architecture.
