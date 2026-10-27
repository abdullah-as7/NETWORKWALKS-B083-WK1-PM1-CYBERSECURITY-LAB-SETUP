# Networkwalks Week 1: Cybersecurity Lab Setup

This repository documents the setup of a cybersecurity testing lab on a Windows 11 host using Oracle VirtualBox and Kali Linux. It covers the Week 1 practical task (WK1-PM1) from the Networkwalks Academy course.

The full write-up is in [Report.pdf](Report.pdf). Screenshots of each configuration step are in the [screenshots](screenshots/) folder.

## Lab Summary

| Requirement | Configuration | Status |
|---|---|---|
| VirtualBox as the base hypervisor | Oracle VirtualBox Manager, latest recommended version | Done |
| Kali Linux as attacking machine | Kali Linux 2026.3 VM (`kali-linux-2026.3-virtualbox-amd64`) | Done |
| Subnet 10.0.0.0/24 | NAT Network `NatNetwork`, IPv4 prefix `10.0.0.0/24` | Done |
| NATNetwork with 10.0.0.0/24 | DHCP enabled, IPv6 disabled | Done |
| Clipboard and drag-and-drop enabled | Both set to Bidirectional | Done |
| Shared folder from host | `E:\Shared Folder` shared as `Shared_Folder` (auto-mount, full access) | Done, see note below |
| Kali IP address 10.0.0.2/24 | `eth0` = `10.0.0.2/24` | Done |
| Full internet access | `ping google.com`: 4 of 4 packets received, 0% loss | Done |
| Snapshot of the VM | `Fresh Snapshot` taken 27 Oct 2026 | Done |

## Repository Structure

```
Networkwalks-Week1/
├── README.md
├── Report.pdf
└── screenshots/
    ├── 01_Kali_Linux_Running.png
    ├── 02_NAT_Network_Settings.png
    ├── 03_Kali_Network_Configuration.png
    ├── 04_Internet_Connectivity.png
    ├── 05_Clipboard_DragDrop.png
    ├── 06_Shared_Folder.png
    └── 07_Kali_Snapshot.png
```

## Environment

| Component | Details |
|---|---|
| Host OS | Windows 11 |
| Hypervisor | Oracle VirtualBox |
| Guest OS | Kali Linux 2026.3 (64-bit) |
| Guest memory | 2048 MB |
| Network mode | NAT Network (`NatNetwork`, 10.0.0.0/24) |
| Kali IP address | 10.0.0.2/24 on `eth0` |

## Setup Steps

1. Install 7-Zip and VirtualBox on the host.
2. Create a NAT Network named `NatNetwork` with the IPv4 prefix `10.0.0.0/24` and DHCP enabled.
3. Import the Kali Linux VirtualBox image and attach its network adapter to `NatNetwork`.
4. Enable Shared Clipboard and Drag-and-Drop (both Bidirectional) in the VM's General settings.
5. Create a shared folder on the host and mount it in the guest.
6. Set the static IP `10.0.0.2/24` on Kali, then verify with `ip -4 addr show eth0`.
7. Verify internet access with `ping -c 4 google.com`.
8. Take a snapshot of the VM once everything above is confirmed.

## Verification Commands

```bash
# Confirm the static IP on the Kali interface
ip -4 addr show eth0

# Confirm outbound internet connectivity
ping -c 4 google.com
```

Expected results: `eth0` shows `inet 10.0.0.2/24`, and the ping returns 4 replies with 0% packet loss.

## Troubleshooting Reference

If the Kali VM has no internet access on VirtualBox 7 with Kali 2026.1 or newer, the course recommends running the following commands inside the guest, then restarting Kali:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

Other checks: confirm the NAT Network was created correctly, confirm no other VM on the same NAT Network is using `10.0.0.2`, and restart the VMs and the host.

## Notes

- **Shared folder path.** The task asks for the host's `/downloads` folder to be shared. This lab shares `E:\Shared Folder` instead, mounted as `Shared_Folder`. The sharing mechanism is the same, but the path differs from the task brief.
- **Snapshot description.** The description attached to the snapshot reads "Set the ip 10.0.0.1". The address verified on the VM is `10.0.0.2/24`, which is the value the task requires.

## Author

M. Abdullah Ashraf

Internship: Networkwalks Academy

Instructor: Waqas Karim, CCIE

Week 1 Project Module 1 (WK1-PM1)
