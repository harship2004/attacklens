\# Lab Setup



\## Machines

| VM | OS | Role | IP (host-only) |

|---|---|---|---|

| ubuntu-victim | Ubuntu 26.04.1 LTS Desktop | Victim: DVWA, Apache, MariaDB, Suricata (Week 2) | 192.168.56.102 |

| kali | Kali Linux | Attacker | 192.168.56.102 |



\## Network

\- VirtualBox host-only network: 192.168.56.0/24 (DHCP enabled)

\- Each VM has two adapters: NAT (internet for installs) and Host-only (private lab)

\- The lab is isolated from other networks



\## VM settings

\- 4 GB RAM, 2 CPUs each

\- Ubuntu disk: 30 GB



\## DVWA

\- Installed on Ubuntu at /var/www/html/DVWA

\- Stack: Apache, MariaDB, PHP

\- Security level: Low

\- Accessed from Kali at http://192.168.56.102/DVWA



\## Verification

\- Kali can ping ubuntu-victim (4/4 replies)

\- DVWA login page loads from Kali



\## Snapshots

\- clean-lab-setup

\- dvwa-installed

