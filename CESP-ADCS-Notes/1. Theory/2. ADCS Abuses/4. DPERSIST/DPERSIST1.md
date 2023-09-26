## Forge ANY domain certificate using stolen CA Root certificate and private keys ##

#### What's running under the hood ####
- The CA certificate:
	- Exists on the **CA server itself**, with its private key protected by machine DPAPI (unless the OS uses a TPM/HSM/other hardware for protection).
	- The Issuer and Subject for the cert are both set to the distinguished name of the CA.
	- CA certificates (and only CA certs) have a “CA Version” extension.
	- There are no EKUs

#### What the attack is about ####
- Once we have compromised the CA, we can forge valid user/computer certificates by stealing and using the RootCA certificate and private key.
- This is also called the Golden Cert attack and is quite similar to the Golden Ticket Attack (steal KRBTGT hash).
- This attack is a good alternative to the more heavily fingerprinted Golden Ticket attack.
- This forged certificate will be valid until the end date specified and as long as the root CA certificate is valid (usually from 5 to 10+ years).
- It's also valid for machines, so combined with `S4U2Self`, an attacker can maintain persistence on any domain machine for as long as the CA certificate is valid.
- The certificates generated with this method **cannot be revoked** as CA is not aware of them.

### Exploitation ###
1. **ForgeCert**
`PS C:\ADCS\Auditor> ForgeCert.exe --CaCertPath ca.pfx --CaCertPassword Password123! --Subject "CN=User" --SubjectAltName localadmin@theshire.local --NewCertPath localadmin.pfx --NewCertPassword Password123!`

2. **Certipy**
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CESP-ADCS]
└─# certipy forge -ca-pfx RootCA.pfx -upn administrator@cb.corp -subject 'CN=Administrator,CN=Users,DC=CB,DC=CORP'
```