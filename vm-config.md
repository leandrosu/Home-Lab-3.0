## System Overview

- **Hostname:** KRANG
- **Role:** Active Directory Domain Controller
- **Environment:** Home Lab 3.0
- **Criticality:** HIGH

KRANG is the authoritative identity and directory services server.
All identity-aware systems depend on this VM.

---

## Operating System

- **OS:** Windows Server 2022
- **Edition:** Standard (Evaluation)
- **Installation Type:** Server with GUI
- **Patch Strategy:** Controlled Windows Update

---

## Network Configuration

- **VLAN:** 20 (SERVERS)
- **IP Address:** 172.16.20.2
- **Subnet Mask:** /24
- **Default Gateway:** 172.16.20.1 (MikroTik)

---

## DNS Architecture

### Internal DNS

- KRANG hosts **Active Directory–integrated DNS**
- All domain records are managed internally
- No public DNS configured directly on servers

### DNS Forwarders

- **Cloudflare DNS** is configured as upstream resolver

### Scope

- DNS services provided to:
  - VLAN 10 (INFRA)
  - VLAN 20 (SERVERS)
  - VLAN 30 (CLIENTS)
  - VLAN 40 (WIFI)
  - VLAN 50 (MGMT)
  - VLAN 100 (LAB / APPS)

- **Excluded VLAN:**
  - VLAN 60 (Guest / IoT)

This design preserves Kerberos stability and prevents DNS leakage.

---

## Active Directory Configuration

- **Forest:** Single forest
- **Domain:** Single-domain model
- **FSMO Roles:** All held by KRANG
- **Installed Roles:**
  - Active Directory Domain Services (AD DS)
  - DNS Server
  - Group Policy Management

---

## DHCP

- **Role:** DHCP Server
- **Status:** Active and AD-authorized
- **Scope:** Lab internal networks (as defined)
- **Integration:** Tight AD integration

---

## Identity & Access Model

- Centralized user and group management
- Authorization based on AD security groups
- No service relies on local authentication
- Linux systems authenticate via Kerberos / Winbind

---

## Group Policy (GPO)

GPOs are used to:

- Enforce security baselines
- Standardize client configuration
- Prepare endpoints for logging and monitoring

Policies are applied intentionally; no unused defaults remain.

---

## Logging & Monitoring

- **Windows Event Logs:** Enabled
- **Wazuh Agent:** Installed
- **Log Destination:** WAZUH (SOC)

Collected events include:
- Authentication and logon activity
- Privilege usage
- AD and DNS operations

---

## Integrations

### Dependent Systems

- **DATABANK:** Authentication and authorization via AD
- **WAZUH:** Identity-aware correlation and alerting
- **Linux Servers:** Kerberos-based authentication

KRANG is a hard dependency for all identity-aware services.

---

## Security Posture

- Kerberos-based authentication
- Minimal local administrator usage
- Centralized policy enforcement
- Continuous SOC visibility

---

## Known Limitations

- Single Domain Controller
- No redundancy
- Backup strategy pending formalization

---

## Change Control

Any changes to:
- AD structure
- DNS configuration
- GPOs
- DHCP scopes

must be documented and validated before implementation.

---

## Status

- Domain operational
- DNS stable
- Integrated with Linux servers
- Integrated with SOC
- Ready for operational runbook
