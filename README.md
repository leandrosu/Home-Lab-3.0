# Home Lab 3.0 — Enterprise-Style Infrastructure & Security Lab

![Status](https://img.shields.io/badge/project-active-brightgreen) ![Lab Type](https://img.shields.io/badge/type-Home--lab-white) ![License](https://img.shields.io/badge/license-MIT-lightgrey) ![Platform](https://img.shields.io/badge/platform-Proxmox-orange) ![Focus](https://img.shields.io/badge/focus-Blue%20Team%20%2B%20OffSec-blue)

> **Status:** Active / Evolving
> **Mindset:** Production-grade, documentation-first, no shortcuts

---

## 🎯 Project Overview

Home Lab 3.0 is an **enterprise-inspired infrastructure and security laboratory** designed to simulate how modern organizations build, operate, and secure their internal environments.

This lab is **not** a collection of random services. It is a deliberately segmented environment focused on:

* Network design and segmentation (VLANs)
* Centralized identity and access management (Active Directory)
* Linux / OS X / Windows interoperability
* File services with auditing and traceability
* Security monitoring (SOC / SIEM)
* Network intrusion detection (IDS)
* Visibility, containment, and recovery

Every decision is documented.
If something is not documented, it does not exist.

---

## 🧠 Design Principles

The lab follows strict engineering principles:

* **Stability over speed**
* **Security over convenience**
* **Documentation over memory**
* **No mixed roles per server**
* **No local users when centralized identity exists**
* **No public DNS directly on servers**
* **Testing happens in isolation, not by weakening core systems**

This lab is treated as if it belongs to a real company and could be audited at any time.

---

## 🏗️ Physical & Core Infrastructure

### Hardware

* **Router:** MikroTik hAP ax3 (RouterOS 7.16.x)
* **Switch:** Cisco Catalyst 2960G-24TC-L (Layer 2 only)
* **Hypervisor:** Proxmox VE

  * Host: Beelink SER5 (Ryzen 5, 32GB RAM, NVMe)
  * Management IP: `172.16.10.10`

### Wireless

* **Access Point:** TP-Link EAP653 (True Wi-Fi 6)
* Trunked to the Cisco switch
* VLANs served:

  * VLAN 40 — Internal Wi‑Fi
  * VLAN 60 — Guest / IoT

---

## 🌐 Network Architecture

### VLAN Segmentation

| VLAN | Name          | Subnet          | Purpose                          |
| ---: | ------------- | --------------- | -------------------------------- |
|   10 | INFRA         | 172.16.10.0/24  | Core infrastructure & hypervisor |
|   20 | SERVERS       | 172.16.20.0/24  | Servers and core services        |
|   30 | CLIENTS       | 192.168.30.0/24 | End‑user clients                 |
|   40 | WIFI          | 192.168.40.0/24 | Internal Wi‑Fi                   |
|   50 | MGMT          | 172.16.50.0/24  | Network management               |
|   60 | GUEST / IOT   | 192.168.60.0/24 | Guest & IoT (isolated)           |
|  100 | LAB / APPS    | Lab-defined     | Applications & testing           |
|  999 | NATIVE-UNUSED | —               | Isolated native VLAN             |

### Routing

* All **inter‑VLAN routing** is handled by the **MikroTik router**
* The Cisco switch operates strictly at **Layer 2**
* Each VLAN has a dedicated gateway on the MikroTik

---

## 🌍 DNS Strategy

DNS is intentionally designed to preserve **Active Directory integrity** while still allowing controlled internet resolution.

* **KRANG (Active Directory)** is the **authoritative internal DNS** server
* **Cloudflare DNS** is used as the **upstream resolver** for all VLANs **except VLAN 60**

### DNS Flow

* Servers and clients → KRANG (AD DNS)
* KRANG → Cloudflare (forwarders)
* VLAN 60 (Guest / IoT): isolated, no AD dependency

This guarantees:

* Stable Kerberos authentication
* Predictable name resolution
* No public DNS configured directly on servers

---

## 🖥️ Virtual Machines Inventory

### 🟦 KRANG — Active Directory Domain Controller

* **OS:** Windows Server 2022
* **IP:** `172.16.20.2`
* **VLAN:** 20 (SERVERS)
* **Roles:**

  * Active Directory Domain Services
  * DNS
  * DHCP

KRANG is the **single source of identity** in the lab. If KRANG is unavailable, the environment is considered degraded.

---

### 🟦 DATABANK — File Server & Identity‑Aware Storage

* **OS:** Debian Linux
* **IP:** `172.16.20.3`
* **VLAN:** 20 (SERVERS)
* **Services:**

  * Samba
  * Winbind
  * Kerberos
  * File access auditing

All access is **group‑based via Active Directory**. There are no local users for file access.

#### Parallel Project: Samba Administration GUI

DATABANK also hosts a **separate, controlled project**:

> A web‑based **Samba Administration GUI** designed to:
>
> * Manage Samba shares
> * Assign permissions using AD groups
> * Execute privileged operations safely
> * Provide audit‑friendly administration

This project is intentionally isolated from core Samba operations and treated as an independent initiative.

---

### 🟦 WAZUH — SOC / SIEM

* **OS:** Ubuntu 24.04 LTS
* **IP:** `172.16.20.20`
* **VLAN:** 20 (SERVERS)
* **Components:**

  * Wazuh Manager
  * Wazuh Indexer (OpenSearch)
  * Wazuh Dashboard

#### Data Sources

* Windows logs (KRANG)
* Linux logs (DATABANK, SURICATA, MORPHEUS)
* Network IDS (Suricata)
* Cloudflare audit logs

WAZUH represents a **real SOC workflow**, with noise filtering, correlation, and identity‑aware visibility.

---

### 🟦 SURICATA — Network Intrusion Detection System

* **OS:** Linux
* **IP:** `192.168.30.50`
* **VLAN:** 30 (CLIENTS / MONITORING)
* **Mode:** Passive IDS

Traffic is captured via a **SPAN/mirror port** on the Cisco switch.

* No inline blocking
* No packet manipulation
* Full east‑west and north‑south visibility

All Suricata logs are forwarded to Wazuh for centralized analysis.

---

### 🟦 MORPHEUS — Application & Testing Server

* **OS:** Ubuntu Linux
* **IP:** `172.16.20.100`
* **VLAN:** 100 (LAB / APPS)
* **Purpose:**

  * Application testing
  * Security validation
  * Controlled event generation for SOC

MORPHEUS is intentionally isolated from core services.

---

## 🔍 Traffic Visibility & Monitoring

* Cisco SPAN mirrors multiple VLANs to the Suricata sensor
* Suricata performs packet‑level inspection
* Wazuh correlates:

  * Identity (AD)
  * File activity (Samba)
  * Network behavior (IDS)
  * Cloud activity (Cloudflare)

This provides **end‑to‑end visibility**.

---

## 📚 Documentation Structure

Each VM has its own documentation set:

* `VM-HARDWARE.md` — CPU, RAM, disks, NICs
* `VM-CONFIG.md` — services, ports, configuration files
* `VM-RUNBOOK.md` — step‑by‑step setup, validation, rollback

Additional documentation:

* Network architecture
* Switching & routing
* Security tuning
* Improvements roadmap

---

## 🚀 Current Status

* Network segmentation complete
* Identity services operational
* File services integrated with AD
* SOC ingesting and correlating logs
* IDS actively monitoring traffic
* Documentation baseline established

---

## 🧭 Roadmap (High Level)

* Secondary Domain Controller
* Backup & restore strategy
* Advanced SOC correlation rules
* MITRE ATT&CK mapping
* Automated response scenarios

---

**Author:** Leandro Stolfo Uehara  
**Project:** Homelab 3.0 — built for learning, testing, and breaking things safely 🔧💻🧩
