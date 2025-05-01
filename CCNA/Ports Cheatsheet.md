# Ports

Created by: Tobias
Created time: March 26, 2025 9:47 AM
Category: Cybersecurity, Netværk, Notes
Last updated time: April 2, 2025 8:53 AM

# 🔌 Common Ports Cheat Sheet

## 🌐 Most Common Ports (For Certifications like CCNA, CompTIA)

| Port | Service/Protocol | Description |
| --- | --- | --- |
| 7 | Echo | Echo test |
| 20 | FTP-Data | File Transfer Protocol – data |
| 21 | FTP | File Transfer Protocol – control |
| 22 | SSH/SCP/SFTP | Secure Shell, secure file transfer |
| 23 | Telnet | Remote login, unencrypted |
| 25 | SMTP | Email routing |
| 53 | DNS | Domain Name System |
| 67/68 | DHCP/BOOTP | Dynamic Host Configuration Protocol |
| 69 | TFTP | Trivial File Transfer Protocol |
| 80 | HTTP | Hypertext Transfer Protocol |
| 88 | Kerberos | Authentication protocol |
| 110 | POP3 | Email retrieval protocol |
| 123 | NTP | Time synchronization |
| 137–139 | NetBIOS | Windows network file sharing |
| 143 | IMAP | Internet Message Access Protocol |
| 161/162 | SNMP | Network management |
| 194 | IRC | Internet Relay Chat |
| 389 | LDAP | Directory services |
| 443 | HTTPS | Secure HTTP |
| 445 | Microsoft-DS/SMB | Windows file sharing |
| 464 | Kerberos (password) | Kerberos password changes |
| 547 | DHCPv6 | IPv6 DHCP |
| 636 | LDAPS | LDAP over SSL/TLS |
| 1720 | H.323 | Video conferencing |
| 3389 | RDP | Remote Desktop |
| 5060 | SIP | VoIP signaling |
| 5061 | SIP-TLS | Secure SIP |

---

## 📡 Other Notable Services by Port

| Port(s) | Service | Notes |
| --- | --- | --- |
| 443 | HTTPS | Secure HTTP |
| 587 | SMTP (submission) | Email submission by clients |
| 993 | IMAP over SSL | Secure IMAP |
| 995 | POP3 over SSL | Secure POP3 |
| 3306 | MySQL | Database |
| 5432 | PostgreSQL | Database |
| 5900+ | VNC | Remote desktop viewer |
| 8080 | HTTP Proxy | Alternative web port |
| 8443 | HTTPS Alt | HTTPS for admin panels |

---

## 🧪 Nmap Scanning Commands

| nmap -sT --top-ports 1000 -v -oG - | # Top 1000 TCP ports |
| --- | --- |
| nmap -sU --top-ports 1000 -v -oG - | # Top 1000 UDP ports |
| sort -r -k3 /usr/share/nmap/nmap-services | # Ports by frequency |
| nmap -sT -p*telnet* -v -oG - | # Scan specific services |

---

## 📁 Port Ranges

- **Well-known Ports:** `0–1023`
- **Registered Ports:** `1024–49151`
- **Dynamic/Private Ports:** `49152–65535`