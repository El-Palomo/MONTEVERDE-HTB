# MONTEVERDE-HTB

Desarrollo de la VM MONTEVERDE de HACK THE BOX (HTB)

## 1. Configuración de la VM

- La VM se encuentra en estado de retirada de HTB
- Se debe activar la VM para poder usarla, se requiere una suscripción PREMIUM.

## 2. Escaneo de Puertos

```
┌──(root㉿kali)-[~/HT/MONTEVERDE]
└─# nmap -n -P0 -sS -sC -vv -oA full 10.129.127.11 
Warning: The -P0 option is deprecated. Please use -Pn
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-01 13:05 EDT
NSE: Loaded 125 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 13:05
Completed NSE at 13:05, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 13:05
Completed NSE at 13:05, 0.00s elapsed
Initiating SYN Stealth Scan at 13:05
Scanning 10.129.127.11 [1000 ports]
Discovered open port 135/tcp on 10.129.127.11
Discovered open port 445/tcp on 10.129.127.11
Discovered open port 139/tcp on 10.129.127.11
Discovered open port 53/tcp on 10.129.127.11
Discovered open port 3269/tcp on 10.129.127.11
Discovered open port 389/tcp on 10.129.127.11
Discovered open port 464/tcp on 10.129.127.11
Discovered open port 593/tcp on 10.129.127.11
Discovered open port 3268/tcp on 10.129.127.11
Discovered open port 88/tcp on 10.129.127.11
Discovered open port 636/tcp on 10.129.127.11
Completed SYN Stealth Scan at 13:05, 20.06s elapsed (1000 total ports)
NSE: Script scanning 10.129.127.11.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 13:05
NSE Timing: About 96.44% done; ETC: 13:06 (0:00:01 remaining)
Completed NSE at 13:06, 42.05s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 13:06
Completed NSE at 13:06, 0.00s elapsed
Nmap scan report for 10.129.127.11
Host is up, received user-set (0.29s latency).
Scanned at 2022-04-01 13:05:38 EDT for 62s
Not shown: 989 filtered tcp ports (no-response)
PORT     STATE SERVICE          REASON
53/tcp   open  domain           syn-ack ttl 127
88/tcp   open  kerberos-sec     syn-ack ttl 127
135/tcp  open  msrpc            syn-ack ttl 127
139/tcp  open  netbios-ssn      syn-ack ttl 127
389/tcp  open  ldap             syn-ack ttl 127
445/tcp  open  microsoft-ds     syn-ack ttl 127
464/tcp  open  kpasswd5         syn-ack ttl 127
593/tcp  open  http-rpc-epmap   syn-ack ttl 127
636/tcp  open  ldapssl          syn-ack ttl 127
3268/tcp open  globalcatLDAP    syn-ack ttl 127
3269/tcp open  globalcatLDAPssl syn-ack ttl 127

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 24710/tcp): CLEAN (Timeout)
|   Check 2 (port 58125/tcp): CLEAN (Timeout)
|   Check 3 (port 23044/udp): CLEAN (Timeout)
|   Check 4 (port 59026/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: 0s
| smb2-time: 
|   date: 2022-04-01T17:06:03
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required

```




