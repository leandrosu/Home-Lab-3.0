# KRANG — VM-HARDWARE.md

## VM Identity

- **Hostname:** KRANG
- **VM ID (Proxmox):** 102
- **Role:** Active Directory Domain Controller
- **Criticality:** HIGH

KRANG is the core identity service of Home Lab 3.0.
If this VM is unavailable, the environment is considered degraded.

---

## Hypervisor Information

- **Hypervisor:** Proxmox VE 9.x
- **Node:** fandangos
- **Host Hardware:** Beelink SER5
- **Management IP (Proxmox):** 172.16.10.10
- **VLAN:** 10 (INFRA)

---

## Virtual Hardware Allocation

### CPU

- **Sockets:** 1
- **Cores:** 2
- **Total vCPUs:** 2
- **CPU Type:** Default (KVM / Proxmox compatible)

---

### Memory

- **Allocated RAM:** 4.00 GB
- **Ballooning:** Not enabled (static allocation)

---

### Storage

- **Primary Disk:** scsi0
- **Storage Backend:** local-lvm
- **Disk Size:** 80 GB
- **Disk Usage:**
  - Operating System
  - Active Directory database (NTDS)
  - SYSVOL
  - DNS data

---

### Firmware & Machine Type

- **BIOS:** SeaBIOS
- **Machine Type:** pc-i440fx-10.0
- **Display:** Default

---

### Network Interfaces

| Interface | Model | Bridge | VLAN Tag | Purpose |
|--------|------|--------|---------|--------|
| net0 | e1000 | vmbr0 | 20 | AD / DNS / DHCP |

- **IP Address:** 172.16.20.2
- **Subnet:** 172.16.20.0/24
- **Gateway:** 172.16.20.1 (MikroTik)

---

## Availability & Redundancy

- **Deployment Model:** Single Domain Controller
- **High Availability:** ❌ Not implemented
- **Replication:** ❌ Not applicable (single DC)

---

## Hardware Design Notes

- KRANG is intentionally single-role (AD only)
- Conservative resource allocation prioritizes stability
- Hardware profile is appropriate for lab-scale AD workloads

---

## Risks & Constraints

- Single point of failure for identity services
- Recovery depends on:
  - Proxmox snapshots
  - System State backups (to be implemented)

---

## Change Control

Any change to:
- CPU
- RAM
- Disk
- Network configuration

**must be documented and approved**, as it directly impacts authentication, DNS, and the entire lab.

---

## Status

- VM provisioned
- Hardware validated against Proxmox
- Ready for configuration and operational documentation
