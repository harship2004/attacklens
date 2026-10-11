\# AttackLens



\*\*Attack Simulation \& Intrusion Detection Lab\*\*



AttackLens is an isolated virtual lab where common network and web attacks are simulated against a victim machine and detected with the open-source IDS \*\*Suricata\*\*. Alerts will be parsed with Python, stored in SQLite, and shown on a custom Flask dashboard.



> \*\*Ethical notice:\*\* All testing is done only inside a self-owned, isolated virtual lab for educational purposes. Never run these tools against systems you do not own.



\## Status



\- \[x] Week 1: Lab network and DVWA victim application

\- \[ ] Week 2: Suricata installation and rule basics

\- \[ ] Week 3: Attack simulation and baseline results

\- \[ ] Week 4: Custom rules and tuning

\- \[ ] Week 5: Python parser, SQLite, MITRE ATT\&CK mapping

\- \[ ] Week 6: Flask dashboard and notifications

\- \[ ] Week 7: Testing, evaluation, screenshots

\- \[ ] Week 8: Report, presentation, demo video



\## Objectives



1\. Build an isolated attacker/victim lab (Kali Linux + Ubuntu)

2\. Simulate five attacks: port scan, SSH brute force, SQL injection, XSS, SYN flood

3\. Deploy Suricata and write custom detection rules

4\. Measure detection rate and false positives before and after tuning

5\. Build a Python parser and Flask dashboard to store and visualize alerts

6\. Send notifications for high-severity alerts



\## Architecture



```

Kali Linux (Attacker)

&#x20;       |

&#x20;       v

Ubuntu (Victim: DVWA + SSH + Suricata)

&#x20;       |

&#x20;  Suricata alerts (eve.json)

&#x20;       |

&#x20;       v

Python Parser -> SQLite -> Flask Dashboard + Alerts

```



All machines run as VirtualBox VMs on a host-only network.



\## Tech Stack



VirtualBox, Kali Linux, Ubuntu 26.04 LTS, DVWA, Apache, MariaDB, PHP, Suricata, Python, Flask, SQLite, Git



\## Lab Setup (Step by Step)



\### 1. Requirements

\- Windows laptop with 16 GB RAM and virtualization (VT-x/AMD-V) enabled

\- VirtualBox 7.x

\- Kali Linux VirtualBox image

\- Ubuntu 26.04 LTS Desktop ISO (64-bit)



\### 2. Create the host-only network

In VirtualBox: \*\*File > Tools > Network Manager > Host-only Networks > Create\*\*.

Use `192.168.56.1/24` with DHCP enabled.



\### 3. Create the VMs



| VM | RAM | CPUs | Disk | Role |

|---|---|---|---|---|

| ubuntu-victim | 4 GB | 2 | 30 GB | Victim: DVWA, Suricata |

| kali | 4 GB | 2 | default image | Attacker |



Set both VMs to:

\- \*\*Adapter 1:\*\* NAT (internet for installing packages)

\- \*\*Adapter 2:\*\* Host-only Adapter (private lab network)



\### 4. Check connectivity

On each VM run `ip a` to find the `192.168.56.x` address. From Kali:

```bash

ping -c 4 <ubuntu-ip>

```



\### 5. Install the web stack on Ubuntu

```bash

sudo apt update

sudo apt install -y apache2 mariadb-server php php-mysql php-gd libapache2-mod-php git

```



\### 6. Install DVWA

```bash

cd /var/www/html

sudo git clone https://github.com/digininja/DVWA.git

cd DVWA/config

sudo cp config.inc.php.dist config.inc.php

```



\### 7. Create the DVWA database

```bash

sudo mariadb

```

```sql

CREATE DATABASE dvwa;

CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'p@ssw0rd';

GRANT ALL PRIVILEGES ON dvwa.\* TO 'dvwa'@'localhost';

FLUSH PRIVILEGES;

EXIT;

```

These are DVWA's default lab credentials. Never reuse them anywhere real.



\### 8. Enable the PHP setting and fix permissions

```bash

sudo sed -i 's/^allow\_url\_include = Off/allow\_url\_include = On/' /etc/php/\*/apache2/php.ini

sudo chown -R www-data:www-data /var/www/html/DVWA

sudo chmod -R 755 /var/www/html/DVWA

sudo systemctl restart apache2

```



\### 9. Finish setup in the browser (from Kali)

1\. Open `http://<ubuntu-ip>/DVWA/setup.php`

2\. Click \*\*Create / Reset Database\*\*

3\. Log in with `admin` / `password`

4\. Go to \*\*DVWA Security\*\*, set the level to \*\*Low\*\*, and click \*\*Submit\*\*



The reCAPTCHA, mod\_rewrite, and API vendor warnings on the setup page can be ignored, because they only affect optional labs.



\### 10. Take snapshots

Take a VirtualBox snapshot of each VM (`clean-lab-setup`, `dvwa-installed`) so you can roll back if something breaks.



\## Verification



\- Kali pings the Ubuntu VM (4/4 replies)

\- DVWA login page loads from Kali's browser

\- DVWA security level set to Low



\## Repository Structure



```

attacklens/

├── README.md

├── docs/          Lab notes, report, screenshots

├── suricata/      Custom rules (rules/)

├── parser/        Alert log parser (planned)

├── dashboard/     Flask dashboard (planned)

└── attacks/       Documented attack commands for the lab

```



\## Roadmap



Next: install Suricata on the Ubuntu VM, learn rule syntax, and run the first attacks from Kali.



\## Author



Your Name | Harshita

GitHub: harship2004

