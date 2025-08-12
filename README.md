# Active Information Gathering (Scanning & Enumeration)

## Overview
This project demonstrates **active information gathering techniques** for penetration testing, including:
- Network scanning
- OS fingerprinting
- Service enumeration
- Firewall evasion
- SMB and LDAP enumeration

It is based on real lab work using Kali Linux and a controlled pentest lab environment.

---

## Tools & Technologies
- **Nmap & Zenmap** – Network scanning and service detection
- **Hping3** – Packet crafting and host discovery
- **SMBmap & SMBclient** – SMB share enumeration
- **Enum4Linux** – OS and SMB/Samba host enumeration
- **JXplorer** – LDAP enumeration GUI tool
- **Wireshark** – Packet capture and analysis

---

## 🟦 Nmap Scanning

### 1. Basic TCP Scan
Nmap scans the **1,000 most common TCP ports** by default.
  ```bash
  nmap <target IP>
  ```

  ![Basic Nmap TCP Scan](images/001_Basic%20Nmap%20Scan.png)

### 2. Ping Sweep
Identify all live hosts on a network:
  ```bash
  nmap -sn <target range>
  ```

  ![Nmap Ping Sweep](images/002_Nmap%20ping%20sweep.png)

### 3. OS & Service Version Detection
  - -A → Aggressive scan
  - -O → OS detection
  - -sV → Service version detection
    ```bash
    nmap -A -O -sV <target IP>
    ```

  ![Nmap OS Detection and Service Versioning](images/004_Screenshot%202025-08-02%20220900.png)

### 4. Scanning Hosts with ICMP Disabled
  ```bash
  nmap -Pn <target IP>
  ```

Skips host discovery and treats the target as online.

### 5. Stealth Scan (Half-Open)
 ```bash
  nmap -sS -p 80 <target IP>
  ```

  ![Nmap Stealth Scan](images/005_Nmap%20Stealth%20Scan.png)

Wireshark shows that no full TCP connection was established.

![Wireshark Results for Stealth Scan](images/006_Wireshark%20Results%20for%20Stealth%20Scan.png)

### 6. UDP Scanning
 ```bash
  nmap -sU <target IP>
  ```

  ![Nmap UDP Scan](images/007_Nmap%20UDP%20Scan.png)

### 7. Evading Detection
  - Decoys: `nmap -D RND:10 <target IP>`
  
    ![Nmap Decoy Scan](images/008_Nmap%20IP%20Spoofing1.png)

  Wireshark Result: 
    ![Nmap Decoy Scan2](images/009_Wireshark%20Results%20for%20IP%20Spoofing.png)
  
  - Fragment packets: `nmap -f <target IP>`

    ![Packet Fregmentation](images/010_Wireshark%20Result%20for%20Packet%20Fregmentation%20(Port%2080).png)
  
  - Spoof source port: `nmap --source-port 123 <target IP>`
  
    ![Source Port Spoofing](images/011_Wireshark%20Results%20for%20source-port%20spoofing.png)
  
  - Spoof MAC: `nmap --spoof-mac 00:11:22:33:44:55 <target IP>`

    ![Nmap MAC Spooffing1](images/012_Nmap%20MAC%20Address%20Spoofing.png)

  Wireshark Result:<br>
    ![Nmap MAC Spooffing2](images/013_Wireshark%20results%20for%20MAC%20address%20spoofing%20on%20port%20389.png)
  
  - Custom MTU: `nmap --mtu 24 <target IP>`

### 8. Detecting Stateful Firewalls
ACK scan:
 ```bash
  nmap -sA <target IP>
  ```

  - No response = Firewall present
  - RST response = No firewall

![Nmap ACK Scan](images/014_The%20result%20indicates%20that%20there%20is%20a%20firewall%20present%20(filtered).png)

### 9. Other Scan Types
  - XMAS scan: `nmap -sX <target IP>`
  - FIN scan: `nmap -sF <target IP>`
  - NULL scan: `nmap -sN <target IP>`

![Nmap Closed Scan Types](images/018_Nmap%20XMAS%2C%20FIN%20and%20NULL%20scan%20on%20port%2080.png)

Wireshark results for Nmap XMAS Scan:
![Wireshark results for Nmap XMAS Scan](images/015_Wireshark%20Results%20for%20XMAS%20scan.png)

Wireshark results for Nmap FIN Scan:
![Wireshark results for Nmap FIN Scan](images/016_Wireshark%20Result%20for%20FIN%20scan.png)

Wireshark results for Nmap NULL Scan:
![Wireshark results for Nmap NULL Scan](images/017_Wireshark%20Result%20for%20NULL%20scan.png)

### 10. Nmap Scripting Engine (NSE)
 ```bash
  nmap --script vuln <target IP>
  ```

![Nmap Scripting Engine](images/019_Nmap%20--script%20scan%20for%20common%20vulnerabilities%20(vuln).png)

## 🟨 Zenmap
GUI version of Nmap — allows creation of custom scan profiles.
![Basic TCP Scan With Zenmap1](images/021_Creating%20a%20new%20profile%20in%20Zenmap.png)

Basic TCP Scan with Zenmap:

![Basic TCP Scan With Zenmap2](images/020_Basic%20TCP%20scan%20with%20Zenmap.png)

## 🟪 Hping3
Used for packet crafting, service probing, and network analysis.
Example: ICMP ping
 ```bash
  ping <target IP> -c 4
  ```

![ICMP Ping](images/022_Ping%20request%20with%20four%20ICMP%20echo%20requests.png)

Using Hping3 to send SYN packets to probe port 80:
 ```bash
  hping3 -S <target IP> -c 4
  ```

![Hping3](images/023_Hping3%20request.png)
