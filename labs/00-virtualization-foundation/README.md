# Lab 00 — Virtualization Foundation

**Status:** Complete · **Date:** 2026-11-21 · **Time invested:** ~3 hours
**Environment:** VirtualBox 7.2.16 + Extension Pack on Windows 11 Pro (AMD Ryzen 9 5900X host) · Lab network: NAT Network `LABNET` 10.20.0.0/24 · Guest: Ubuntu Desktop 26.04.1 (`UBUNTU-TEST`, 4 GB RAM, 2 vCPU, 40 GB dynamically allocated disk)

## Objective

Built the virtualization bench that every later lab in this repository runs on: a VirtualBox hypervisor on a private NAT network, a first guest operating system provisioned and verified online, and a snapshot baseline that makes destructive experimentation safe to repeat.

## Architecture

```text
Windows 11 Pro host (AMD Ryzen 9 5900X)
└── VirtualBox 7.2.16 + Extension Pack
    └── NAT Network: LABNET  10.20.0.0/24  (DHCP enabled)
        └── UBUNTU-TEST — Ubuntu Desktop 26.04.1 LTS — 10.20.0.3
```

A NAT network gives each guest both internet access and direct reachability to its neighbors — the combination a real small-office network provides. The master network diagram for the whole environment arrives with Lab 05, once a firewall and multiple segments exist to draw.

## What I did

1. Installed VirtualBox 7.2.16 and the Extension Pack on Windows 11 Pro without disabling the Windows hypervisor, which the existing WSL2 environment depends on.
   `![VirtualBox VM list](screenshots/virtualbox-vm-list.png)`
2. Created the NAT network `LABNET` on `10.20.0.0/24` with DHCP enabled, deliberately avoiding VirtualBox's default `10.0.2.0/24` prefix.
   `![LABNET network configuration](screenshots/virtualbox-labnet.png)`
3. Provisioned the `UBUNTU-TEST` VM: 4096 MB RAM, 2 processors, 40 GB dynamically allocated disk.
4. Mounted the Ubuntu Desktop 26.04.1 installer ISO and installed to the 40 GB virtual disk using the "erase disk and install" option.
   `![Ubuntu desktop running inside VirtualBox](screenshots/ubuntu-desktop-in-virtualbox.png)`
5. Verified connectivity: the guest received `10.20.0.3` from LABNET's DHCP, and Firefox loaded a web page through NAT.
6. Took a powered-off snapshot named `2026-11-21-Fresh-Install` as the baseline.
7. Ran a deliberate destructive test — `sudo rm -rf ~/Documents` — then restored the snapshot and confirmed the folder returned intact.

## Problems I hit and how I fixed them

### I downloaded the ARM64 installer for an x86-64 host

- **Symptom:** the first ISO downloaded was `ubuntu-26.04.1-desktop-arm64.iso` — 6 GB of installer that would not have booted.
- **Diagnosis:** checked the host CPU architecture and confirmed it was an AMD Ryzen 9 5900X on an x64-based system. VirtualBox on an x86-64 host has no ARM emulation, so an ARM64 image fails immediately rather than running slowly — an easy failure to misread as a corrupt ISO or a BIOS problem.
- **Fix:** discarded the ARM64 image and downloaded the AMD64 build.
- **Lesson:** confirm the target architecture before downloading an installer. On a hypervisor, the guest architecture must match what the hypervisor can actually execute, not merely what the vendor offers.

## Skills demonstrated

- Hypervisor installation and configuration (VirtualBox 7, Extension Pack)
- Virtual machine provisioning: CPU, memory, and dynamically allocated storage allocation
- Virtual networking: NAT Network creation, DHCP scope, isolated lab subnet design
- Guest OS installation from ISO media
- Snapshot and rollback for change safety — the standard pre-change practice in production IT
- Architecture compatibility validation (x86-64 host versus ARM64 guest images)

## What's next

Lab 01 uses this bench to deploy a Windows Server domain controller and a Windows client, then works through the day-to-day account and Group Policy tasks of a help desk. The snapshot habit established here is what makes a domain promotion safe to attempt and cheap to undo.
