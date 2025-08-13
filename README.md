# Active Information Gathering (Scanning & Enumeration)

## Overview
This project demonstrates **active information gathering techniques** for penetration testing, including:
- Network scanning
- OS fingerprinting
- Service enumeration
- Firewall evasion
- SMB and LDAP enumeration

It is based on real lab work using Kali Linux and a controlled pentest lab environment.

## Tools & Technologies
- **Nmap & Zenmap** – Network scanning and service detection
- **Hping3** – Packet crafting and host discovery
- **SMBmap & SMBclient** – SMB share enumeration
- **Enum4Linux** – OS and SMB/Samba host enumeration
- **JXplorer** – LDAP enumeration GUI tool
- **Wireshark** – Packet capture and analysis

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

## ⬛ SMB, LDAP Enumeration, and Null Sessions
### SMBmap
Discover SMB shares and permissions:  
```bash
smbmap -H <target IP>
smbmap -H <target IP> -r <share>
```

![SMBmap](images/024_SMBMap%20scan%20on%20Metasploitable2.%20The%20tmp%20folder%20gives%20us%20read%20and%20write%20permissions..png)

![SMBmap](images/025_SMBMap%20content%20listing.png)

### SMBclient
Enumerate SMB services:
```bash
smbclient -L //target
```

![SMBclient](images/026_Attempting%20to%20extract%20any%20shares%20on%20the%20target%20device%20with%20SMBclient.png)

### Enum4linux
Enumerates:
  - Password policies
  - OS details
  - Shares
  - Domain/group membership
  - Users

```bash
enum4linux <target IP>
```

![Hping3](images/027_Results%20for%20enum4linux%20on%20Windows%20Server%20VM.png)

### LDAP Enumeration
Default ports:
  - LDAP: 389 (plaintext)
  - LDAPS: 636 (encrypted)   
Tools like JXplorer can be used for querying directories.  

![JXplorer Interface](images/028_The%20JXplorer%20interface.png)

Setting Up A filter in JXplorer:  
![JXplorer Interface](images/029_Custom%20JXplorer%20Filter%20to%20search%20for%20Users.png)

Filter Results:  
![JXplorer Results](images/030_Filter%20Results.png)

### Null Sessions
Anonymous login to extract sensitive info.
Example with rpcclient:
```bash
rpcclient -U "" -N <target IP>
```

![Null Session With RPCclient](images/031_Successful%20null%20session%20login.png)

## Skills Demonstrated
- **Network Scanning & Enumeration**
  - Conducting host discovery with Nmap (`-sn`, `-Pn`)
  - Performing TCP, UDP, stealth, and OS/service version scans
  - Using Nmap Scripting Engine (NSE) for vulnerability detection
  - Evading IDS/IPS and firewalls with decoys, fragmentation, and spoofing

- **Firewall & Security Device Testing**
  - Detecting stateful firewalls (`-sA`, `-sX`, `-sF`, `-sN`)
  - Analyzing packet responses with Wireshark

- **GUI & CLI Security Tools**
  - Operating **Zenmap** for graphical Nmap scanning
  - Using **Hping3** for packet crafting, port probing, and ICMP/SYN testing

- **Service Enumeration**
  - Enumerating SMB shares with **SMBmap** and **SMBclient**
  - Enumerating OS, users, groups, and shares with **Enum4linux**
  - Querying LDAP directories with **JXplorer**

- **Exploitation Preparation**
  - Identifying vulnerable services for potential exploitation
  - Establishing null sessions for anonymous information gathering

- **Packet & Traffic Analysis**
  - Capturing and interpreting network traffic with Wireshark
  - Identifying TCP flags, fragmented packets, and spoofed traffic

- **Security Best Practices**
  - Understanding plaintext vs encrypted protocols (LDAP vs LDAPS)
  - Recognizing insecure service configurations

## References
- Glen D. Singh, *Learn Kali Linux 2019: Perform Powerful Penetration Testing Using Kali Linux, Metasploit, Nessus, Nmap, and Wireshark*, Packt Publishing, 2019. ISBN: 978-1789611806.
- EC-Council. *Certified Ethical Hacker (CEH) v13 Official Courseware*. Albuquerque, NM: EC-Council, 2023.

## Author
Tafadzwa Nemukuyu(www.linkedin.com/in/tafadzwa-nemukuyu)
