# Week 3: Baseline Attack and Detection Results

Testing done with Suricata's **default** Emerging Threats Open ruleset only (no custom rules for these attacks yet). Custom rules are added in Week 4.

| # | Attack | Tool / Command | Detected by default rules? | Alert message | Notes |
|---|---|---|---|---|---|
| 1 | ICMP ping | `ping -c 4 192.168.56.102` | No | LAB ICMP ping detected | Caught by my own custom rule, sid 1000001. Default rules did not alert on plain ping. |
| 2 | Port scan | `nmap -sS 192.168.56.102` | No | LAB possible SYN port scan | Caught by my own custom rule, sid 1000002. Default rules did not alert on the scan itself. |
| 3 | SSH brute force | `hydra -l harshita -P passwords.txt ssh 192.168.56.102` | No | None | auth.log showed repeated failed logins and an srclimit_penalise entry (SSH's own throttling). Suricata default rules did not alert. |
| 4 | SQL Injection | DVWA User ID field: `1' OR '1'='1` and `1' UNION SELECT user, password FROM users -- -` | Partial | SURICATA HTTP Response excessive header repetition (sid 2221036) | OR payload returned all 5 users instead of 1. UNION payload extracted all usernames and MD5 password hashes directly from the database. No SQLi-specific signature fired; only a generic HTTP anomaly rule triggered, likely incidental. Addressed with a custom rule in Week 4. |
| 5 | XSS (Reflected + Stored) | DVWA: `<script>alert('XSS test')</script>` in Reflected and Stored XSS fields | Partial | SURICATA HTTP Response excessive header repetition (sid 2221036) | Reflected XSS executed immediately on submit. Stored XSS executed on submit and again on every page reload, confirming the payload was saved server-side. No XSS-specific signature fired; only the same generic HTTP anomaly rule triggered. Addressed with a custom rule in Week 4. |
| 6 | SYN flood | `hping3 -S -p 80 --flood 192.168.56.102` | Pending | | |

## Observations

- Default Suricata rules focus on known malware, scanning tools, and protocol anomalies, not generic lab traffic like a single ping or a single Nmap scan.
- SSH itself has built-in brute-force throttling (srclimit_penalise), independent of Suricata.
- Background noise was observed on the host-only network (Spotify P2P discovery broadcasts from the Windows host), unrelated to the lab attacks.
- This baseline motivates Week 4: writing custom rules for brute force, SQL injection, and XSS detection, since the defaults miss them.