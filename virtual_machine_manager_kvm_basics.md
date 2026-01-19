# Virtual Machine Manager (KVM) — Windows 10 Pro Guest on Ubuntu Host  
Developer Commands, Tips, and Fixes

---

## Assumptions
- Host OS: Ubuntu 22.04+ / 24.04
- CPU supports virtualization (AMD-V / Intel VT-x) and enabled in BIOS
- Guest OS: Windows 10 Pro (ISO available)
- Hypervisor: KVM + QEMU
- UI: virt-manager
- User has sudo access

---

## Core Stack (Why it matters)
- KVM: Hardware acceleration
- QEMU: Emulator + device model
- libvirt: VM lifecycle + networking
- virt-manager: GUI frontend
- SPICE/QXL: Graphics + clipboard
- VirtIO: High-performance disk/network drivers

---

## Install Required Packages (Host)
sudo apt update  
sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients virt-manager bridge-utils cpu-checker

Verify KVM:
kvm-ok  
Expected: KVM acceleration can be used

---

## User Permissions
sudo usermod -aG libvirt,kvm $USER  
newgrp libvirt

Verify:
groups

---

## Enable libvirt
sudo systemctl enable --now libvirtd  
sudo systemctl status libvirtd

---

## Launch virt-manager
virt-manager  
Connection should show: QEMU/KVM – Connected

---

## Create Windows 10 VM (Correct Defaults)
- Local install media → Windows 10 ISO
- OS type: Windows 10
- RAM: 8 GB minimum
- CPU: 4 cores minimum
- Storage: 80–120 GB (qcow2)
- Network: NAT
- Firmware: UEFI
- Chipset: Q35
- Display: SPICE
- Video: QXL

Do not start yet.

---

## Attach VirtIO Drivers
Download virtio-win ISO from Fedora project.  
VM → Settings → Add Hardware → Storage → attach virtio-win.iso

---

## Windows Install Disk Fix
During disk selection:
- Load Driver
- Browse virtio ISO → viostor/w10/amd64
- Disk appears → continue install

---

## Install VirtIO Tools Inside Windows
After first boot:
- Open virtio ISO
- Run virtio-win-guest-tools.exe
- Reboot

---

## Clipboard and Mouse Integration
- Display: SPICE
- Channel: Spice agent
- Clipboard works after reboot

---

## Performance Tuning (Mandatory)
CPU:
- Mode: Host passthrough

Memory:
- Enable ballooning unless low-latency required

Disk:
- Bus: VirtIO
- Cache: Writeback
- I/O: Native

---

## Networking
Find guest IP:
virsh domifaddr vm-name

NAT works by default for internet access.

---

## Snapshots
Create:
virsh snapshot-create-as vm-name pre-change

List:
virsh snapshot-list vm-name

Revert:
virsh snapshot-revert vm-name pre-change

---

## Autostart VM
Enable:
virsh autostart vm-name

Disable:
virsh autostart --disable vm-name

---

## Common Failures and Fixes
VM slow:
- CPU not passthrough
- VirtIO drivers missing
- KVM not loaded

Check:
lsmod | grep kvm

Black screen:
- Video not QXL
- Display not SPICE

No network:
- Install NetKVM from virtio ISO

libvirt connection error:
sudo systemctl restart libvirtd

---

## virsh CLI Cheatsheet
virsh list --all  
virsh start vm-name  
virsh shutdown vm-name  
virsh destroy vm-name  
virsh reboot vm-name  
virsh edit vm-name

---

## Backup VM
Shutdown first:
virsh shutdown vm-name

Copy disk:
cp ~/.local/share/libvirt/images/vm-name.qcow2 /backup/

---

## When to Use This Setup
Good for:
- Windows-only tools
- App testing
- Cross-platform builds
- Isolated dev environments

Not ideal for:
- Gaming
- Heavy GPU workloads
- Real-time audio/video

---

## Verification Checklist
- kvm-ok passes
- CPU mode = host passthrough
- Disk and network = VirtIO
- Clipboard works
- Expected CPU cores visible in Windows Task Manager

---
