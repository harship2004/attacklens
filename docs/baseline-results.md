# Week 3: Baseline Attack and Detection Results

Testing done with Suricata's **default** Emerging Threats Open ruleset only (no custom rules for these attacks yet). Custom rules are added in Week 4.

| # | Attack | Tool / Command | Detected by default rules? | Alert message | Notes |
|---|---|---|---|---|---|
| 1 | ICMP ping | `ping -c 4 192.168.56.102` | No | LAB ICMP ping detected | Caught by my own custom rule, sid 1000001. Default rules did not alert on plain ping. |
| 2 | Port scan | `nmap -sS 192.168.56.102` | No | LAB possible SYN port scan | Caught by my own custom rule, sid 1000002. Default rules did not alert on the scan itself. |
| 3 | SSH brute force | `hydra -l harshita -P passwords.txt ssh 192.168.56.102` | No | None | auth.log showed repeated failed logins and an srclimit_penalise entry (SSH's own throttling). Suricata default rules did not alert. |
| 4 | SQL Injection | DVWA User ID field, OR-based payload | Pending | | |
| 5 | XSS | DVWA name field, script tag payload | Pending | | |
| 6 | SYN flood | `hping3 -S -p 80 --flood 192.168.56.102` | Pending | | |

## Observations

- Default Suricata rules focus on known malware, scanning tools, and protocol anomalies, not generic lab traffic like a single ping or a single Nmap scan.
- SSH itself has built-in brute-force throttling (srclimit_penalise), independent of Suricata.
- Background noise was observed on the host-only network (Spotify P2P discovery broadcasts from the Windows host), unrelated to the lab attacks.
- This baseline motivates Week 4: writing custom rules for brute force, SQL injection, and XSS detection, since the defaults miss them.