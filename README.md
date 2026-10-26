# Home Lab

A small enterprise-style network I'm building to learn Windows/Linux administration, Active Directory, and security monitoring. The goal is to run it the way a sysadmin would at a real business (scripted and repeatable where possible, with backups and rollback), then attack it and practice detecting that activity.

> Part of my portfolio: [github.com/astaticflow](https://github.com/astaticflow)

## Hardware
| Role | Machine |
|------|---------|
| Lab host | Main workstation running VMware Workstation Pro |
| Attack machine | Laptop (planned: Parrot OS or Kali) |
| Spare | Raspberry Pi 3B+, spare tower PCs |

## Virtual machines
| Host | OS | Role | Status |
|------|----|------|--------|
| DC01 | Windows Server 2025 | Domain controller for `lab.local` (AD DS, DNS) | Built |
| LNX01 | Ubuntu Server LTS | Linux server | Built |
| WIN11-01 | Windows 11 | Domain-joined client | In progress |
| SO01 | Security Onion | Network monitoring / detection | Planned |
| Targets | Metasploitable2, DVWA | Intentionally vulnerable systems | Planned |
| GNS3 | n/a | Network simulation | Planned |

## Network diagram
<img width="4128" height="2124" alt="image" src="https://github.com/user-attachments/assets/8105bbe0-dee3-407a-bbcc-05cc9155547f" />

## What I'm practicing
- **Active Directory:** multiple users, groups, security groups, and Group Policy
- **Change safety:** snapshots and backup/restore to roll back mistakes
- **Automated workstation setup:** learning free Microsoft tooling for configuring new clients without manual steps
- **Detection (next):** run attacks from the laptop, then find them in Security Onion

## Build log
See [BUILD-LOG.md](BUILD-LOG.md) for dated entries covering what I built, what broke, and how I fixed it.

## Conventions
- No credentials, keys, or real public IPs in this repo. Lab passwords are throwaway and never reused.
- Screenshots are cropped to omit anything about my real home network.
