## System Overview

- **Hostname:** SURICATA
- **Role:** Network IDS (Passive)
- **Environment:** Home Lab 3.0
- **Criticality:** MEDIUM–HIGH

SURICATA provides packet-level visibility across multiple VLANs using SPAN/mirrored traffic.

---

## Operating System

- **OS:** Ubuntu Server 24.04 LTS
- **Architecture:** x86_64
- **Patch Strategy:** Controlled via APT

---

## Network Configuration

### Management Interface (net0)

- **VLAN:** 30 (CLIENTS / MONITORING)
- **IP Address:** 192.168.30.50
- **Subnet Mask:** /24
- **Default Gateway:** 192.168.30.1

### Capture Interface (net1)

- **Mode:** No IP address
- **Source:** Cisco SPAN / mirror port
- **Traffic:** East–West and North–South visibility

---

## DNS & Name Resolution

- **Primary DNS:** KRANG (AD DNS)
- **Upstream Resolver:** Cloudflare (via KRANG)
- **Public DNS locally configured:** ❌ No

---

## Suricata Configuration

- **Mode:** IDS (Passive)
- **Inline Blocking:** ❌ Disabled
- **Ruleset:** Community rules + tuned defaults
- **Configuration File:** `/etc/suricata/suricata.yaml`

---

## Traffic Visibility

Mirrored VLANs include:

- VLAN 10 — INFRA
- VLAN 20 — SERVERS
- VLAN 30 — CLIENTS
- VLAN 40 — WIFI
- VLAN 60 — GUEST / IOT

This provides full lab-wide visibility.

---

## Logging

- **Primary Log:** `eve.json`
- **Log Types:**
  - Alerts
  - DNS
  - HTTP
  - TLS
  - Flow metadata

---

## SOC Integration

- **Wazuh Agent:** Installed
- **Log Forwarding:** `eve.json` → WAZUH
- **Correlation:** Network events + identity + endpoint data

---

## Security Posture

- Passive inspection only
- No packet modification
- No impact on production traffic
- Restricted management access

---

## Known Limitations

- No active response
- Single sensor
- Detection limited to mirrored traffic

---

## Change Control

Any changes to:
- Rulesets
- Interface mapping
- Logging options

must be validated to avoid blind spots.

---

## Status

- IDS operational
- Traffic capture active
- Logs flowing to SOC
- Ready for operational runbook
