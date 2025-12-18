## VM Identity

- **Hostname:** SURICATA
- **VM ID (Proxmox):** 103
- **Role:** Network Intrusion Detection System (IDS)
- **Criticality:** MEDIUM–HIGH

SURICATA provides passive network visibility.
Loss of this VM reduces detection capability but does not interrupt services.

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
- **Disk Size:** 50 GB
- **Disk Purpose:**
  - Operating System
  - Suricata binaries and rules
  - Local log buffers (`eve.json`)

---

### Firmware & Machine Type

- **BIOS:** SeaBIOS
- **Machine Type:** Default (i440fx)
- **Display:** Default

---

### Network Interfaces

| Interface | Model  | Bridge | VLAN Tag | Purpose |
|----------|--------|--------|----------|---------|
| net0 | virtio | vmbr0 | 30 | Management / log forwarding |
| net1 | virtio | vmbr0 | none | SPAN / mirrored traffic |

- **Management IP:** 192.168.30.50
- **Subnet:** 192.168.30.0/24
- **Gateway:** 192.168.30.1 (MikroTik)

---

## Attached Devices

- **USB Device:** host=0bda:8156  
  (Dedicated capture / NIC passthrough support)

---

## Availability & Redundancy

- **Deployment Model:** Single IDS sensor
- **High Availability:** ❌ Not implemented
- **Inline Mode:** ❌ Not enabled (IDS only)

---

## Hardware Design Notes

- Dual-NIC design separates:
  - Management traffic
  - Mirrored packet capture
- Resource sizing optimized for passive inspection
- No inline risk to production traffic

---

## Risks & Constraints

- Single sensor (visibility gap if offline)
- Disk usage depends on log retention
- Packet drops possible under high load

---

## Change Control

Any modification to:
- NIC configuration
- Disk size
- CPU/RAM allocation

must be documented, as it affects detection reliability.

---

## Status

- VM provisioned
- Hardware validated against Proxmox
- Ready for configuration documentation
