# 🖥️ Home Lab — From Zero to Proxmox

A complete beginner's journey building a home virtualization lab from scratch — no prior server experience required.

This repo documents everything: hardware choices, setup steps, lessons learned, and scripts collected along the way. If you're starting from zero, this is the guide I wish I had.

---

## 📦 Hardware

| Component | Details |
|---|---|
| **Server** | HP Slim Desktop 410-030 |
| **CPU** | Intel Core i7-4790T (4 cores / 8 threads) |
| **RAM** | 16GB DDR3L 1600MHz (2×8GB DIMM) |
| **Storage** | 1TB HDD |
| **Network** | Wired Ethernet (required for Proxmox) |
| **Admin Machine** | HP Laptop (used for all management via browser/SSH) |

> **Why this hardware?** The i7-4790T has 8 threads, which comfortably supports running 4–5 VMs simultaneously. RAM was upgraded from 8GB to 16GB to support multiple concurrent VMs.

---

## 🧱 Stack

| Layer | Technology |
|---|---|
| **Hypervisor** | Proxmox VE 9.1 |
| **Guest OS** | Ubuntu Server 22.04.5 LTS |
| **Management** | Proxmox Web UI · SSH |
| **USB Flashing** | Rufus (Windows) |

---

## 🚀 Quick Start Order

If you're following along, go in this order:

1. [Hardware setup & RAM upgrade](./hardware.md)
2. [Install Proxmox VE](./proxmox-setup.md)
3. [Create Ubuntu Server VM](./vm-ubuntu.md)
4. [Gotchas & lessons learned](./lessons-learned.md)

---

## 💡 Why Build a Home Lab?

- Practice Linux, networking, and virtualization hands-on
- Safe sandbox to break things and learn from mistakes
- Build skills relevant to real IT/DevOps/SysAdmin work
- Self-host services without relying on cloud subscriptions

---

*Built and documented by a complete beginner. All mistakes included for free.*
