# Hardware

Full specs and upgrade notes for the home lab server.

---

## Server — HP Slim Desktop 410-030

| Component | Spec |
|---|---|
| CPU | Intel Core i7-4790T, 4 cores / 8 threads, up to 3.9GHz |
| RAM | 16GB DDR3L 1600MHz (2×8GB DIMM) |
| Storage | 1TB HDD |
| GPU | Intel HD Graphics 4600 (integrated) |
| Network | Intel Ethernet (wired) + 802.11b/g/n Wi-Fi |
| RAM Slots | 2 (both occupied after upgrade) |
| RAM Max | 32GB (2×16GB DDR3L) |
| Form Factor | Slim desktop |

---

## RAM Upgrade

The machine shipped with 8GB. This was upgraded to 16GB before installing Proxmox.

**Why upgrade first:**
Running multiple VMs simultaneously requires adequate RAM. 8GB is tight; 16GB gives comfortable headroom for a beginner home lab (2–4 VMs running at once).

**What was installed:**
- 2×8GB DDR3L 1600MHz DIMM (non-ECC)
- DDR3L (low voltage) — required for this motherboard

**Maximum supported:**
32GB (2×16GB) — this is the hardware ceiling. 16GB is sufficient for beginner/intermediate use.

**Note:** The HP 410-030 has only **2 RAM slots**. To upgrade beyond 16GB, both sticks must be replaced.

---

## Admin Machine — HP Laptop

Used exclusively for managing the server. Never runs VMs.

- Connects to Proxmox web UI via browser (`https://192.168.50.133:8006`)
- Connects to VMs via SSH
- Used to flash the Proxmox USB installer with Rufus

---

## Network Setup

| Device | Connection |
|---|---|
| HP Desktop (server) | Wired Ethernet to router — permanent |
| HP Laptop (admin) | Wi-Fi or wired |

The server is connected via Ethernet only. Wi-Fi is not used and not supported by Proxmox during installation.

The server runs **fully headless** — no monitor, keyboard, or mouse attached. All management is done remotely from the laptop.

---

## Physical Setup

The HP Desktop is tucked away permanently. Only two cables attached:
1. Power
2. Ethernet

No display needed after Proxmox is installed. The Proxmox web UI handles everything from the browser.
