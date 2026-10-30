<!-- Template for new entries:

## YYYY-MM-DD: Short title
- Goal:
- What I did:
- What broke / surprised me:
- Fix / what I learned:
- Next:

-->

# Homelab Build Log

Enterprise-style virtual lab for hands-on practice in Windows administration, network segmentation, and security monitoring. This log records each build session: what was configured, how it was verified, and what comes next.

## Environment

| Item | Detail |
|---|---|
| Hypervisor | VMware Workstation Pro |
| Host | Intel Core i7-12700, 32 GB RAM |
| Domain | corp.homelab.internal (NetBIOS: CORP) |
| Firewall / router | OPNsense (FW01) |
| Remote administration | Tailscale to hypervisor host |

## Network Design

| Segment | OPNsense Interface | Gateway | DHCP Scope | Purpose |
|---|---|---|---|---|
| WAN | WAN | n/a | Upstream | Internet access |
| Corporate | LAN1 | 10.20.10.1/24 | 10.20.10.100 - 10.20.10.200 | Domain-joined servers and workstations |
| Reserved | OPT1 | 10.20.11.1/24 | 10.20.11.100 - 10.20.11.200 | Future use (attacker or lab segment) |

*"LAN1" is the OPNsense interface label for the corp.homelab.internal network.*

## System Inventory

| Hostname | OS / Role | Segment | Status |
|---|---|---|---|
| FW01 | OPNsense, 3 NICs (WAN, LAN1, OPT1) | All | Operational |
| DC01 | Windows Server 2025, AD DS, DNS, Global Catalog (10.20.10.10, static) | LAN1 | Operational, forest root DC |
| WS01 | Windows 11 Enterprise | LAN1 | Operational, DHCP lease, pending domain join |
| WS02 | Windows 11 Enterprise | TBD | Deferred |
| LNX01 | Ubuntu Server LTS | TBD | Deferred |
| SO01 | Security Onion (network monitoring) | TBD | Deferred |

## Planned Work

- [x] Capture clean baseline snapshots of all deployed VMs
- [x] Configure DC01: static address, AD DS, DNS, domain promotion
- [ ] Distribute DC01 (10.20.10.10) as DNS server to clients via OPNsense DHCP
- [ ] Join WS01 to the domain; assign fixed addressing; snapshot
- [ ] Build OU structure, user accounts, security groups, and Group Policy
- [ ] Test backup and restore procedures
- [ ] Define inter-segment firewall rules on FW01
- [ ] Deploy Security Onion and connect to the corporate segment
- [ ] Add attacker segment and vulnerable targets; run detection exercises
- [ ] Configure WireGuard remote access to FW01
- [ ] Automate workstation provisioning with free Microsoft tooling

---

## Log

### 2026-10-29: Domain controller deployment (DC01)

**Objective:** Establish the Active Directory forest and DNS infrastructure for corp.homelab.internal.

**Actions**
- Captured baseline snapshots of all existing VMs (FW01, DC01, WS01).
- Assigned DC01 a static address of 10.20.10.10 (outside the 10.20.10.100-200 DHCP scope), gateway 10.20.10.1.
- Installed the AD DS role and promoted DC01 to the first domain controller of a new forest, corp.homelab.internal (NetBIOS CORP), with DNS and Global Catalog.
- Took a snapshot before promotion and a post-promotion snapshot (Post-AD-promotion).

**Verification**
- Services running: ADWS, DNS, Kdc, Netlogon, NTDS.
- SYSVOL and NETLOGON shares present.
- `dcdiag`: Connectivity, Advertising, SysVolCheck, and NetLogons tests passed.
- `nslookup` resolves the internal domain locally and external names through the DNS forwarder.

**Issues and resolutions**

| Issue | Cause | Resolution |
|---|---|---|
| `ping 1.1.1.1` returned "transmit failed" | `route print` showed no default route; gateway not configured | Set the default gateway to 10.20.10.1 on the adapter |
| Promotion prerequisite check failed | AD CS role was installed on the server | Removed AD CS and re-ran the wizard |

**Next:** Set OPNsense DHCP to hand out 10.20.10.10 as the DNS server, create users and security groups, join WS01 to the domain, snapshot WS01.

---

### 2026-10-28: Environment rebuild, firewall and first workstation

**Objective:** Rebuild the lab from a clean state with a segmented network and an incremental deployment approach.

**Actions**
- Created VMs for OPNsense, Windows Server 2025, Windows 11 Enterprise (x2), Ubuntu Server LTS, and Security Onion.
- Scoped initial deployment to one Windows 11 workstation; remaining VMs deferred.
- Completed OS image attachment for Windows Server and Windows 11.
- Installed OPNsense (FW01) with three interfaces: WAN, LAN1 (10.20.10.1/24), OPT1 (10.20.11.1/24). Enabled DHCP on LAN1 and OPT1 (.100 - .200).
- Installed Windows Server 2025 (DC01) with a named administrator account rather than the built-in Administrator. No post-install configuration performed.
- Installed Windows 11 Enterprise (WS01) with a local account, using the domain sign-in option in preparation for domain join.

**Verification**
- WS01 received a DHCP lease from FW01 on the LAN1 segment.
- WS01 confirmed to have internet connectivity through FW01.

**Issues**
- Remote configuration via Tailscale and Windows Remote Desktop from a tablet dropped keyboard input. Work was completed locally on the host instead.

**Next:** Baseline snapshots, then DC01 configuration (static IP, AD DS, DNS) and WS01 domain join.

<!-- Add new entries above this line, newest first. -->



## 2026-10-26: Repo created
- Set up this repo to document the lab.
- Current state: DC01 (Windows Server 2025, `lab.local`) and LNX01 (Ubuntu Server) running under VMware Workstation Pro. Windows 11 client being joined to the domain.
- Next: finish the client domain join, then update the network diagram.


