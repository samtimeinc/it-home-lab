# IT Home Lab — Rainier Trading Co.

**Sam Phin** · Seattle, WA · [LinkedIn](https://www.linkedin.com/in/sam-phin-9bb6a624a/) · [Portfolio](https://www.samnang.work/) · [GitHub](https://github.com/samtimeinc)

I am designing, building, and documenting the complete IT infrastructure for a simulated 45-person company — Rainier Trading Co. — including Active Directory, Microsoft 365, a Linux-hosted ticketing helpdesk, a pfSense firewall, monitoring and SIEM, automation, and documented hardware operations. It all runs on a VirtualBox lab bench on a single physical machine: domain controller, Windows clients, Linux servers, and a firewall that talk to each other over their own private network.

This repository is the evidence trail, not a tutorial collection. Every lab ships with the receipts — screenshots, diagrams, runnable scripts, and a dated work journal that records what broke and how it got fixed. **The status table below is the honest source of truth:** a lab shows complete only when the hands-on work is finished *and* its evidence is published here.

> **Master network diagram** — arrives with Lab 05 (Networking), then gets updated as the environment grows. Editable `.drawio` source and PNG export live in [`/diagrams`](diagrams).

## Lab index

| # | Lab | What it demonstrates | Status | Evidence |
|---|---|---|---|---|
| 00 | Virtualization Foundation | VirtualBox hypervisor, NAT lab network (LABNET `10.20.0.0/24`), VM provisioning, snapshots and rollback | 🔨 In progress | — |
| 01 | Active Directory | Windows Server domain controller, OU and group design, user lifecycle (adds/moves/changes, unlock, disable), Group Policy | ⬜ Not started | — |
| 02 | Microsoft 365 | Tenant administration, Exchange Online, MFA and Conditional Access in Entra ID | ⬜ Not started | — |
| 03 | Linux Fundamentals | A server administered entirely from the command line — users, permissions, services, logs, storage | ⬜ Not started | — |
| 04 | Ticketing System | osTicket self-hosted plus Jira Service Management — intake, triage, SLAs, resolved-ticket queue | ⬜ Not started | — |
| 05 | Networking | pfSense firewall, segmented subnets, DHCP/DNS administration, packet capture analysis | ⬜ Not started | — |
| 06 | Automation & Scripting | PowerShell bulk user provisioning from a spreadsheet, leaver offboarding, reporting; hardened Bash health-check and backup scripts | ⬜ Not started | — |
| 07 | Security Monitoring (SIEM) | Wazuh and Sysmon deployment, detection dashboard, log analysis, a written incident report | ⬜ Not started | — |
| 08 | Data Center Hardware | RAID configuration and rebuild, SMART monitoring, burn-in testing, cabling and rack documentation | ⬜ Not started | — |

⬜ Not started · 🔨 In progress · ✅ Complete (hands-on done and evidence published)

## What's in here

| Folder | Contents |
|---|---|
| [`/labs`](labs) | One folder per lab: a README explaining what was built and why, plus screenshots |
| [`/scripts`](scripts) | PowerShell and Bash, runnable and commented — the comments double as study notes |
| [`/journal`](journal) | Dated work-log entries. Failures and dead ends included — those are the useful part |
| [`/diagrams`](diagrams) | `.drawio` sources next to their PNG exports |
| [`/incident-reports`](incident-reports) | Formal incident write-ups (Lab 07) |
| [`/artifacts`](artifacts) | Supporting evidence: port matrix, burn-in report, health logs |

Every claim above links to evidence. Failures and fixes are in the journal — I consider them the best part.

---

> **Rainier Trading Co. is a simulated environment built for practice — all data is fictional.** Hostnames, users, addresses, and tickets in this repository belong to a lab, not to a real organization. No real personal data, credentials, activation keys, or home-network details appear in these artifacts.
