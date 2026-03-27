# Ubuntu Server VM Setup

How to create and configure an Ubuntu Server 22.04 virtual machine in Proxmox VE.

---

## VM Specs (What Was Used)

| Setting | Value |
|---|---|
| VM ID | 100 |
| Cores | 2 |
| RAM | 2048 MB (2GB) |
| Storage | 32GB |
| OS | Ubuntu Server 22.04.5 LTS |
| SSH | Enabled during install |

---

## Step 1 — Get the Ubuntu Server ISO into Proxmox

You don't need to download the ISO to your laptop first — Proxmox can pull it directly.

1. In the Proxmox web UI, click your **storage** (e.g., `local`) in the left sidebar
2. Click **ISO Images** → **Download from URL**
3. Paste the Ubuntu Server ISO URL:
   ```
   https://releases.ubuntu.com/22.04/ubuntu-22.04.5-live-server-amd64.iso
   ```
4. Click **Query URL** to auto-fill the filename, then click **Download**
5. Wait for the download to complete (progress shows in the task log)

---

## Step 2 — Create the VM

1. Click **Create VM** (top right of Proxmox UI)

**General tab:**
- Node: your Proxmox node
- VM ID: `100` (or any unused number)
- Name: `ubuntu-server`

**OS tab:**
- ISO Image: select the Ubuntu ISO you just downloaded
- Type: Linux
- Version: 6.x - 2.6 Kernel

**System tab:**
- Leave defaults (BIOS: SeaBIOS, Machine: i440fx)

**Disks tab:**
- Storage: `local-lvm`
- Disk size: `32` GB

**CPU tab:**
- Cores: `2`

**Memory tab:**
- Memory: `2048` MB

**Network tab:**
- Bridge: `vmbr0`
- Model: VirtIO (paravirtualized)

Click **Finish**.

---

## Step 3 — Install Ubuntu Server

1. Select your new VM (`100`) in the left sidebar
2. Click **Start** (top right)
3. Click **Console** to open the VM display
4. The Ubuntu installer will boot — select **Install Ubuntu Server**

**During install:**
- Language: English
- Keyboard: your layout
- Network: leave as DHCP for now (you can set static IP after)
- Storage: **Use an entire disk** → select the 32GB virtual disk
- Profile setup: set your name, server name, username, and password
- **SSH Setup:** ✅ Check **Install OpenSSH server** — this is required to SSH in later
- Featured snaps: skip (press Done without selecting anything)

Wait for the install to complete, then click **Reboot Now**.

---

## Step 4 — SSH Into the VM

After the VM reboots, find its IP address in the Proxmox console (it will show after login):

```
ubuntu-server login: _
```

Log in with the username/password you set, then run:

```bash
ip addr show
```

Look for the `inet` address on `ens18` (something like `192.168.50.x`).

Now from your admin laptop, SSH in:

```bash
ssh youruser@192.168.50.x
```

You're in. The VM is running and accessible from your laptop without touching the server. ✅

---

## Step 5 — Initial Server Hardening (Recommended)

Update all packages:

```bash
sudo apt update && sudo apt upgrade -y
```

Set the timezone:

```bash
sudo timedatectl set-timezone America/New_York
```

Check SSH is running:

```bash
sudo systemctl status ssh
```

---

## Useful Proxmox VM Controls

| Action | How |
|---|---|
| Start VM | Select VM → Start |
| Stop VM | Select VM → Shutdown (graceful) |
| Snapshot | Select VM → Snapshots → Take Snapshot |
| Console access | Select VM → Console |
| VM config | Select VM → Hardware |

> **Tip:** Always take a snapshot before making major changes to a VM. You can roll back instantly if something breaks.

---

## Next Step

→ Check out [lessons learned](./lessons-learned.md) for errors hit during setup and how they were fixed.
