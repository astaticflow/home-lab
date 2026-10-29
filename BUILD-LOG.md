# Build Log

Newest entries first. Each entry: what I did, what went wrong, what I learned.


## 2026-10-28: Environment rebuild, firewall and first workstation

**Objective:** Rebuild the lab from a clean state with a segmented network and an incremental deployment approach.

### Actions

- Created VMs for OPNsense, Windows Server 2025, Windows 11 Enterprise (x2), Ubuntu Server LTS, and Security Onion.
- Scoped initial deployment to one Windows 11 workstation; remaining VMs deferred.
- Completed OS image attachment for Windows Server and Windows 11.
- Installed OPNsense (FW01) with three interfaces: WAN, LAN1 (10.20.10.1/24), OPT1 (10.20.11.1/24). Enabled DHCP on LAN1 and OPT1 (.100 - .200).
- Installed Windows Server 2025 (DC01) with a named administrator account rather than the built-in Administrator. No post-install configuration performed.
- Installed Windows 11 Enterprise (WS01) with a local account, using the domain sign-in option in preparation for domain join.

### Verification

- WS01 received a DHCP lease from FW01 on the LAN1 segment.
- WS01 confirmed to have internet connectivity through FW01.

### Issues

- Remote configuration via Tailscale and Windows Remote Desktop from a tablet dropped keyboard input. Work was completed locally on the host instead.

**Next:** Baseline snapshots, then DC01 configuration (static IP, AD DS, DNS) and WS01 domain join.


## 2026-10-26: Repo created
- Set up this repo to document the lab.
- Current state: DC01 (Windows Server 2025, `lab.local`) and LNX01 (Ubuntu Server) running under VMware Workstation Pro. Windows 11 client being joined to the domain.
- Next: finish the client domain join, then update the network diagram.

<!-- Template for new entries:

## YYYY-MM-DD: Short title
- Goal:
- What I did:
- What broke / surprised me:
- Fix / what I learned:
- Next:

-->
