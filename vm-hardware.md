## VM Identity

- **Hostname:** WAZUH
- **VM ID (Proxmox):** 101
- **Role:** SOC / SIEM Platform
- **Criticality:** HIGH

WAZUH is the central security monitoring platform.
Loss of this VM impacts visibility, detection and incident response across the lab.

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
- **Cores:** 4
- **Total vCPUs:** 4
- **CPU Type:** x86-64-v2-AES

---

### Memory

- **Allocated RAM:** 8.00 GB
- **Ballooning:** Not enabled (static allocation)

---

### Storage

- **Primary Disk:** scsi0
- **Storage Backend:** local-lvm
- **Disk Size:** 120 GB
- **Disk Purpose:**
  - Wazuh Manager data
  - OpenSearch indices
  - Dashboards and metadata
  - Log ingestion buffers

---

### Firmware & Machine Type

- **BIOS:** SeaBIOS
- **Machine Type:** Default (i440fx)
- **Display:** Default

---

### Network Interfaces

| Interface | Model | Bridge | VLAN Tag | Purpose |
|--------|------|--------|---------|--------|
| net0 | virtio | vmbr0 | 20 | SOC ingestion & management |

- **IP Address:** 172.16.20.20
- **Subnet:** 172.16.20.0/24
- **Gateway:** 172.16.20.1 (MikroTik)
- **Proxmox Firewall:** Enabled on interface

---

## Availability & Redundancy

- **Deployment Model:** Single-node Wazuh stack
- **High Availability:** ❌ Not implemented
- **Clustering:** ❌ Not implemented

---

## Hardware Design Notes

- Higher disk allocation to support log retention
- CPU and RAM sized for:
  - Multiple agents
  - Indexing workload
  - Dashboard usage
- Virtio network driver selected for performance

---

## Risks & Constraints

- Single SOC node (single point of visibility)
- Disk growth depends on log retention policy
- Performance tied to indexing workload

---

## Change Control

Any modification to:
- CPU
- RAM
- Disk size
- Network configuration

must be documented and validated due to SOC impact.

---

## Status

- VM provisioned
- Hardware validated against Proxmox
- Ready for configuration documentation
