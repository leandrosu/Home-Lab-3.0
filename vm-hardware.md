## VM Identity

- **Hostname:** MORPHEUS
- **VM Name (Proxmox):** Webserver
- **VM ID (Proxmox):** 104
- **Role:** Application / Test Server
- **Criticality:** MEDIUM

MORPHEUS is a non-core server used for applications, testing, and controlled event generation.
Its unavailability does not impact core infrastructure services.

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
- **CPU Type:** x86-64-v2-AES

---

### Memory

- **Allocated RAM:** 4.00 GB
- **Ballooning:** Not enabled (static allocation)

---

### Storage

- **Primary Disk:** scsi0
- **Storage Backend:** local-lvm
- **Disk Size:** 40 GB
- **Disk Purpose:**
  - Operating System
  - Application files
  - Logs and test data

---

### Firmware & Machine Type

- **BIOS:** SeaBIOS
- **Machine Type:** Default (i440fx)
- **Display:** Default

---

### Network Interfaces

| Interface | Model  | Bridge | VLAN Tag | Purpose |
|----------|--------|--------|----------|---------|
| net0 | virtio | vmbr0 | 20 | Application access / logging |

- **IP Address:** 172.16.20.100
- **Subnet:** 172.16.20.0/24
- **Gateway:** 172.16.20.1 (MikroTik)

---

## Availability & Redundancy

- **Deployment Model:** Single application server
- **High Availability:** ❌ Not implemented
- **Redundancy:** ❌ Not required (non-core)

---

## Hardware Design Notes

- Conservative resource sizing
- Intended for flexibility, not performance
- Easy to rebuild or replace

---

## Risks & Constraints

- Limited disk space for large datasets
- Not suitable for production workloads
- No redundancy

---

## Change Control

Any changes to:
- CPU
- RAM
- Disk
- Network configuration

must be documented, even though the VM is non-critical.

---

## Status

- VM provisioned
- Hardware validated against Proxmox
- Ready for configuration documentation
