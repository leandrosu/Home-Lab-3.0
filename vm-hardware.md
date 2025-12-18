## VM Identity

- **Hostname:** DATABANK
- **VM Name (Proxmox):** DATABANK-24
- **VM ID (Proxmox):** 100
- **Role:** Centralized File Server
- **Criticality:** HIGH

DATABANK provides centralized storage and identity-aware file services.
Loss of this VM impacts user access to shared data.

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
- **Disk Size:** 80 GB
- **Disk Purpose:**
  - Operating System
  - Samba shares
  - Audit logs
  - Temporary staging for backups

---

### Firmware & Machine Type

- **BIOS:** SeaBIOS
- **Machine Type:** Default (i440fx)
- **Display:** Default

---

### Network Interfaces

| Interface | Model | Bridge | VLAN Tag | Purpose |
|--------|------|--------|---------|--------|
| net0 | virtio | vmbr0 | 20 | File services / AD integration |

- **IP Address:** 172.16.20.3
- **Subnet:** 172.16.20.0/24
- **Gateway:** 172.16.20.1 (MikroTik)

---

## Availability & Redundancy

- **Deployment Model:** Single file server
- **High Availability:** ❌ Not implemented
- **Replication:** ❌ Not implemented

---

## Hardware Design Notes

- Higher CPU and RAM allocation due to:
  - Samba workload
  - File system operations
  - Audit logging overhead
- Virtio network driver selected for performance

---

## Risks & Constraints

- Single point of failure for file services
- Disk growth must be monitored
- Backup strategy required for shared data

---

## Change Control

Any modification to:
- CPU
- RAM
- Disk size
- Network interface

must be documented and validated, as it affects data availability.

---

## Status

- VM provisioned
- Hardware validated against Proxmox
- Ready for configuration documentation
