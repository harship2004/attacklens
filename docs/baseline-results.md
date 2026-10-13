\# Week 3: Baseline Attack and Detection Results



Testing done with Suricata's \*\*default\*\* Emerging Threats Open ruleset only (no custom rules for these attacks yet). Custom rules are added in Week 4.



| # | Attack | Tool / Command | Detected by default rules? | Alert message | Notes |

|---|---|---|---|---|---|

| 1 | ICMP ping | `ping -c 4 192.168.56.102` | No (caught by my own custom rule, sid 1000001) | LAB ICMP ping detected | Default rules did not alert on plain ping |

| 2 | Port scan | `nmap -sS 192.168.56.102` | No (caught by my own custom rule, sid 1000002) | LAB possible SYN port scan | Default rules did not alert on the scan itself |

| 3 | SSH brute force | `hydra -l harshita -P passwords.txt ssh://192.168.56.102` | No | — | auth.log showed repeated failed logins and an `srclimit\_penalise` entry (SSH's own throttling). Suricata default rules did not alert. |

| 4 | SQL Injection | DVWA, `1' OR '1'='1` | Pending | | |

| 5 | XSS | DVWA, `<script>alert('XSS test')</script>` | Pending | | |

| 6 | SYN flood | `hping3 -S -p 80 --flood 192.168.56.102` | Pending | | |



\## Observations



\- Default Suricata rules focus on known malware, scanning tools, and protocol anomalies, not generic lab traffic like ping or a single Nmap scan.

\- SSH itself has built-in brute-force throttling (`srclimit\_penalise`), independent of Suricata.

\- Background noise was observed on the host-only network (Spotify P2P discovery broadcasts from the Windows host), unrelated to the lab attacks.

\- This baseline motivates Week 4: writing custom rules for brute force, SQLi, and XSS detection, since the defaults miss them.

