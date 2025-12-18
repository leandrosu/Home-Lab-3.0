## System Overview

- **Hostname:** WAZUH
- **Role:** SOC / SIEM Platform
- **Environment:** Home Lab 3.0
- **Criticality:** HIGH

WAZUH centralizes security telemetry, correlates events, and provides visibility across the entire lab.

---

## Operating System

- **OS:** Ubuntu Server 24.04 LTS
- **Architecture:** x86_64
- **Patch Strategy:** Controlled via APT

---

## Network Configuration

- **VLAN:** 20 (SERVERS)
- **IP Address:** 172.16.20.20
- **Subnet Mask:** /24
- **Default Gateway:** 172.16.20.1 (MikroTik)

---

## DNS & Name Resolution

- **Primary DNS:** KRANG (Active Directory DNS)
- **Upstream Resolver:** Cloudflare (via KRANG)
- **Public DNS locally configured:** ❌ No

This ensures consistent name resolution and identity correlation.

---

## Wazuh Stack Components

### Installed Components

- **Wazuh Manager**
- **Wazuh Indexer (OpenSearch)**
- **Wazuh Dashboard**

All components run on the same host (single-node deployment).

---

## Log Sources & Integrations

### Endpoint & Server Logs

- **KRANG**
  - Windows Event Logs
  - Authentication and AD activity

- **DATABANK**
  - Samba audit logs
  - System logs

- **SURICATA**
  - Network IDS logs (`eve.json`)

- **MORPHEUS**
  - Application and system logs

---

### External Sources

- **Cloudflare**
  - HTTP access logs
  - Security-related events
  - Parsed and correlated in Wazuh

---

## Agents & Data Flow

- Wazuh agents installed on:
  - Windows servers
  - Linux servers

- Data flow:

Endpoint → Wazuh Agent → Wazuh Manager → Indexer → Dashboard

## Detection & Correlation

- Default Wazuh ruleset
- Custom rules for:
- Samba activity
- Cloudflare events
- Network IDS alerts
- Identity-aware correlation using AD data

---

## Storage & Retention

- Logs indexed in OpenSearch
- Retention based on disk capacity and tuning
- No cold storage implemented yet

---

## Security Posture

- Restricted network exposure
- Proxmox firewall enabled
- Centralized visibility across infrastructure
- No direct user access to raw logs

---

## Known Limitations

- Single-node SOC
- No index replication
- No off-node backups

---

## Change Control

Any changes to:
- Index retention
- Rulesets
- Integrations
- Agent configuration

must be documented and validated to prevent blind spots.

---

## Status

- SOC operational
- Logs ingesting from all core systems
- Dashboards active
- Ready for operational runbook

