# Lessons Learned

Real errors hit during setup and exactly how they were fixed. No sugar-coating.

---

## 1. SQUASHFS Error When Booting the Proxmox USB

**What happened:**
After flashing the Proxmox ISO with Rufus and trying to boot from it, the install process threw a SQUASHFS error and wouldn't continue.

**Root cause:**
The USB write had corrupted data — some blocks didn't write correctly.

**Fix:**
Re-flash the USB in Rufus with **bad block checking enabled**. In Rufus, before writing, check the option to scan for bad blocks. The re-flash took longer but succeeded, and the installer booted cleanly.

**Takeaway:**
If a bootable USB fails with filesystem errors, re-flash with bad block checking before assuming the ISO is corrupt.

---

## 2. KVM Virtualization Warning During Proxmox Install

**What happened:**
Proxmox showed a warning during installation saying KVM hardware virtualization was not detected or not enabled.

**What I thought:**
Assumed VT-x wasn't enabled and something was wrong with the hardware.

**Reality:**
VT-x *was* already enabled in BIOS. The warning was a false alarm — Proxmox sometimes shows this during install even when virtualization is active.

**Fix:**
Clicked through and continued the install anyway. Proxmox installed and ran KVM VMs without any issue.

**Where VT-x lives on the HP 410-030:**
`BIOS → Security → System Security → Virtualization Technology (VTx) → Enabled`
Access BIOS via: power on → press **Esc** → **F9**

**Takeaway:**
Don't let this warning stop you. Verify VT-x is enabled in BIOS, then proceed regardless of the warning.

---

## 3. Browser SSL Certificate Warning on Proxmox Web UI

**What happened:**
When navigating to `https://<PROXMOX-IP>`, the browser threw a security warning about an untrusted certificate.

**Why it happens:**
Proxmox ships with a self-signed certificate. Browsers don't trust self-signed certs by default.

**Fix:**
Click **Advanced → Proceed to site** (wording varies by browser). This is safe on a local network.

**Longer fix (optional):**
Generate a proper certificate using Let's Encrypt if you expose Proxmox externally — but not necessary for a home lab on a private network.

---

## 4. "No Valid Subscription" Popup in Proxmox

**What happened:**
Every time you log into the Proxmox UI, a popup says "You do not have a valid subscription for this server."

**Why it happens:**
Proxmox is free to use but nags you to buy an enterprise support subscription.

**Fix:**
Run this in the Proxmox shell (Nodes → Shell):

```bash
sed -i.bak "s/data.status !== 'Active'/false/" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
systemctl restart pveproxy
```

Refresh the browser — popup is gone. No subscription needed for home lab use.

> ⚠️ This edit may be overwritten by Proxmox updates. Re-run the command after major updates if the popup returns.

---

*More entries will be added as new issues are encountered.*
