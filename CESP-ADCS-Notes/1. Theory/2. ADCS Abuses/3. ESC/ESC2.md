## Enrollee can request cert for ANY user (`CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT` + `Any Purpose` EKU or no EKU) ##

#### What's running under the hood ####
- The second abuse scenario is a variation of [[ESC1]]:
- The Enterprise CA grants low-privileged users enrollment rights.
- Manager approval is disabled.
- No authorized signatures are required.
- An overly permissive certificate template security descriptor grants certificate enrollment rights to low-privileged users.
- The certificate template defines the Any Purpose EKU or no EKU.
- The Any Purpose EKU allows an attacker to get a certificate for any purpose like client authentication, server authentication, code signing, etc.
- A certificate with no EKUs — a subordinate CA certificate —  can be abused for any purpose as well but could also use it to sign new certificates.

#### What the attack is about ####
- These settings allow a **low-privileged user to request a certificate with an arbitrary SAN**, allowing the low-privileged user to authenticate as any principal in the domain via Kerberos or SChannel.
- In the case of using a subordinate CA certificate, an attacker could specify arbitrary EKUs or fields in the new certificates.
- However, if the **subordinate CA is not trusted** by the `**NTAuthCertificates**` object (which it won’t be by default), the attacker cannot create new certificates that will work for domain authentication. Still, the attacker can create new certificates with any EKU and arbitrary certificate values, of which there’s plenty the attacker could potentially abuse (e.g., code signing, server authentication, etc.) and might have large implications for other applications in the network like SAML, AD FS, or IPSec.
- CBA Patch Bypass makes the AD CS CA insert a `szOID_NTDS_CA_SECURITY_EXT` SID extension value which contains the SID of the requesting user in all certificate requests. The domain controller can use this to compare the SID of the authenticating user (or the SID specified in the SAN) against the SID contained in the `szOID_NTDS_CA_SECURITY_EXT` SID extension.
### Enumeration ###
1. **Certify**
`PS C:\ADCS\Auditor> Certify.exe find`

- `msPKI-Certificate-Name-Flag` is set to `ENROLLEE_SUPPLIES_SUBJECT`
- `pkiextendedkeyusage` is set to `Any Purpose` or it has **No value**

### Execution ###
1. **Certify**
- Without CBA Patch Bypass
`PS C:\ADCS\Auditor> .\Certify.exe request /ca:cbp-dc.protectedcb.corp\CBP-CA /domain:protectedcb.corp /template:Substitute-ProtectedUserAccess /altname:Administrator`

- With CBA Patch Bypass
`PS C:\ADCS\Auditor> .\Certify.exe request /ca:cbp-dc.protectedcb.corp\CBP-CA /domain:protectedcb.corp /template:Substitute-ProtectedUserAccess /altname:Administrator /sidextension:S-1-5-21-1286082170-882298176-404569034-500`
