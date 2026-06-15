# Archetype

## About

Archetype is a very easy Windows machine that features a misconfigured Microsoft SQL server, exposed SMB shares and sensitive data exposure. An exposed SMB share can be accessed without authentication in which sensitive files can be found containing plaintext credentials. These credentials can be used to authenticate to MSSQL as the service account user through Impacket's mssqlclient tool. Command execution can then be achieved by enabling xp_cmdshell after which a reverse shell can be uploaded and triggered to get access to the host. Finally, WinPeas can be used to search for vulnerabilities which reveals a Powershell history file containing the password needed to achieve full privilege escalation.

### Reconnaisance
I performed nmap with the -A flag and got the below output:

```
PORT     STATE SERVICE      VERSION
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds Windows Server 2019 Standard 17763 microsoft-ds
1433/tcp open  ms-sql-s     Microsoft SQL Server 2017 14.00.1000.00; RTM
| ms-sql-ntlm-info: 
|   10.129.47.46:1433: 
|     Target_Name: ARCHETYPE
|     NetBIOS_Domain_Name: ARCHETYPE
|     NetBIOS_Computer_Name: ARCHETYPE
|     DNS_Domain_Name: Archetype
|     DNS_Computer_Name: Archetype
|_    Product_Version: 10.0.17763
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2026-06-15T03:51:31
| Not valid after:  2056-06-15T03:51:31
| MD5:     c524 e634 2cf6 9a2b ebe7 005c eb34 d52a
| SHA-1:   02b8 196b ac30 33bc c39e 05c3 c47f 2f95 01cf a851
|_SHA-256: a929 24d5 b659 5d20 f30b 3ea3 83a1 25f4 ae4c 9f92 50c2 5981 d9c1 0f79 b375 8088
|_ssl-date: 2026-06-15T03:53:12+00:00; +23s from scanner time.
| ms-sql-info: 
|   10.129.47.46:1433: 
|     Version: 
|       name: Microsoft SQL Server 2017 RTM
|       number: 14.00.1000.00
|       Product: Microsoft SQL Server 2017
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
5985/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 10|2019|11|2012|2022|2016 (96%)
OS CPE: cpe:/o:microsoft:windows_10 cpe:/o:microsoft:windows_server_2019 cpe:/o:microsoft:windows_11 cpe:/o:microsoft:windows_server_2012:r2 cpe:/o:microsoft:windows_server_2022 cpe:/o:microsoft:windows_server_2016
Aggressive OS guesses: Microsoft Windows 10 1909 - 2004 (96%), Microsoft Windows Server 2019 (95%), Microsoft Windows 11 24H2 - 25H2 (92%), Microsoft Windows Server 2012 R2 (92%), Microsoft Windows Server 2022 (92%), Microsoft Windows 10 1709 - 22H2 (91%), Microsoft Windows 10 1909 (89%), Microsoft Windows Server 2016 (89%), Microsoft Windows 10 1703 or Windows 11 21H2 - 23H2 (89%), Microsoft Windows 11 24H2 (89%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 21 hops
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows Server 2019 Standard 17763 (Windows Server 2019 Standard 6.3)
|   Computer name: Archetype
|   NetBIOS computer name: ARCHETYPE\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2026-06-14T20:53:04-07:00
|_clock-skew: mean: 1h24m24s, deviation: 3h07m52s, median: 23s
| smb2-time: 
|   date: 2026-06-15T03:53:00
|_  start_date: N/A
```
