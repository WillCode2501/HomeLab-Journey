# Proxmox VE Setup Guide

Step-by-step walkthrough for installing Proxmox VE 9.1 on the HP Slim Desktop 410-030 and accessing it headlessly from a separate machine.

---

## Prerequisites

- A USB drive (8GB or larger)
- The HP Desktop connected to your router via **Ethernet** (Wi-Fi is not supported during install)
- A second computer (laptop) to download files and later manage Proxmox via browser
- A monitor, keyboard, and mouse for the initial install only

---

## Step 1 — Download Proxmox VE ISO

On your admin laptop:

1. Go to [proxmox.com/en/downloads](https://www.proxmox.com/en/downloads)
2. Click **Proxmox VE** → download the latest ISO (currently 9.1)
3. Also download **Rufus** from [rufus.ie](https://rufus.ie) — needed to flash the ISO to USB

---

## Step 2 — Flash USB with Rufus

1. Insert your USB drive into the laptop
2. Open Rufus
3. Under **Device**, select your USB drive
4. Under **Boot selection**, click **SELECT** and choose the Proxmox ISO
5. Leave **Partition scheme** as MBR, **Target system** as BIOS or UEFI
6. Click **START** → when prompted, choose **Write in ISO Image mode**
7. Wait for it to complete — status will say **READY**

> ⚠️ **If you see a SQUASHFS error during boot:** Re-flash the USB using Rufus with bad block checking enabled (check the "bad blocks" option before writing). This resolves corrupt write issues.

---

## Step 3 — Enable Virtualization in BIOS

The HP 410-030 has VT-x (hardware virtualization) disabled by default. You must enable it before installing Proxmox.

1. Power on the HP Desktop
2. Immediately press **Esc** to enter the startup menu
3. Press **F9** for BIOS Setup (or F10 depending on screen prompt)
4. Navigate to **Security → System Security**
5. Find **Virtualization Technology (VTx)** and set it to **Enabled**
6. Save and exit

> **Note:** Proxmox may show a KVM virtualization warning during install even with VTx enabled. This is a false alarm — the install proceeds normally.

---

## Step 4 — Install Proxmox VE

1. Insert the flashed USB into the HP Desktop
2. Power on and press **Esc → F9** (or your boot menu key) to select the USB as boot device
3. Select **Install Proxmox VE (Graphical)**
4. Accept the EULA
5. **Target Harddisk:** Select your internal drive (the 1TB HDD)
6. **Location and timezone:** Set your region
7. **Password:** Set a strong root password — this is the admin password for everything
8. **Network configuration:**
   - Management interface: your Ethernet adapter (e.g., `eno1`)
   - Hostname: something like `proxmox.local`
   - IP address: assign a static IP (e.g., `192.168.xxx.xxx`)
   - Gateway: your router IP (e.g., `192.168.xxx.xxx`)
   - DNS: `8.8.8.8` or your router IP
9. Review the summary and click **Install**
10. When complete, click **Reboot**

Remove the USB drive when the machine restarts.

---

## Step 5 — Access the Web UI (Headless from Laptop)

Once Proxmox boots, you no longer need the monitor. The HP Desktop can be tucked away — power + ethernet only from here on.

On your admin laptop, open a browser and go to:

```
https://192.168.xxx.xxx:xxxx
```

> Your browser will warn about an invalid certificate. This is expected — click **Advanced → Proceed** to continue.

Log in with:
- **Username:** `root`
- **Password:** the password you set during install
- **Realm:** Linux PAM standard authentication

You should see the Proxmox dashboard. Your server is live. ✅

---

## Step 6 — Dismiss the Subscription Notice

Proxmox will show a "No valid subscription" popup on every login. To dismiss it without buying a license:

In the Proxmox shell (Nodes → your node → Shell), run:

```bash
sed -i.bak "s/data.status !== 'Active'/false/" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
systemctl restart pveproxy
```

This removes the popup. Proxmox remains fully functional without a subscription for home lab use.

---

## Network Reference

| Item | Value |
|---|---|
| Proxmox Web UI | `https://192.168.xxx.xxx:xxxx` |
| Login | `root` / your password |
| SSH | `ssh root@192.168.xxx.xxx` |

---

## Next Step

→ [Create your first Ubuntu Server VM](./vm-ubuntu.md)
