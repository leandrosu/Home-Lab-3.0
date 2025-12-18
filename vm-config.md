## System Overview

- **Hostname:** DATABANK
- **Role:** Centralized File Server
- **Environment:** Home Lab 3.0
- **Criticality:** HIGH

DATABANK provides shared storage with centralized authentication and auditing.
It mirrors a real enterprise Linux file server integrated with Active Directory.

---

## Operating System

- **OS:** Debian GNU/Linux
- **Architecture:** x86_64
- **Patch Strategy:** Controlled via APT

---

## Network Configuration

- **VLAN:** 20 (SERVERS)
- **IP Address:** 172.16.20.3
- **Subnet Mask:** /24
- **Default Gateway:** 172.16.20.1 (MikroTik)

---

## DNS & Name Resolution

- **Primary DNS:** KRANG (Active Directory DNS)
- **Upstream Resolver:** Cloudflare (via KRANG forwarders)
- **Public DNS on Server:** ❌ Not configured

This design preserves Kerberos stability and AD integration.

---

## Identity Integration

- **Authentication Source:** Active Directory (KRANG)
- **Integration Stack:**
  - Kerberos
  - Winbind
  - Samba AD member configuration

### Identity Model

- No local Linux users for file access
- Authorization via AD security groups only
- UID/GID mapping handled dynamically

---

## File Services

### Samba

- **Service:** Samba (member server)
- **Authentication:** Kerberos / NTLM via AD
- **Access Control:** Group-based (AD groups)
- **Shares:**
  - Defined in `smb.conf`
  - Permissions enforced via filesystem ACLs and Samba

---

## Auditing & Logging

- **Samba Audit Module:** Enabled
- **Events Logged:**
  - File access
  - File modification
  - Deletions
  - Permission changes

- **Log Destination:** WAZUH (SOC)

Auditing enables full traceability of user activity.

---

## Monitoring & SOC Integration

- **Wazuh Agent:** Installed
- **Logs Forwarded:**
  - Samba audit logs
  - System logs
- **Correlation:** User identity + file activity

---

## Parallel Project — Samba Administration GUI

DATABANK also serves as the base for a **separate administration project**:

> **Samba Administration GUI**
>
> A web-based interface designed to:
> - Manage Samba shares
> - Apply permissions using AD groups
> - Execute privileged operations safely
> - Improve auditability of administrative actions
>
> This project is treated as an independent initiative and does not alter core Samba behavior.

---

## Security Posture

- Kerberos-based authentication
- No local authentication for services
- Centralized permission management
- Full audit trail of file operations

---

## Known Limitations

- Single file server
- No redundancy
- Backup and snapshot strategy pending

---

## Change Control

Any changes to:
- Samba configuration
- Share definitions
- Permission model
- Identity integration

must be documented and validated prior to deployment.

---

## Status

- Joined to Active Directory
- File services operational
- Auditing enabled
- Integrated with SOC
