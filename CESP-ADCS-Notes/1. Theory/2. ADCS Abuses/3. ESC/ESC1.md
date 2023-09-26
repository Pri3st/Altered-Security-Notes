## Enrollee can request cert for ANY user (`CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT` + `Client Authentication`/`Smart Card Logon` EKU) ##

#### What's running under the hood ####
- The Enterprise CA grants low-privileged users enrolment rights
- Manager approval is disabled
- No authorized signatures are required
- An overly permissive certificate template security descriptor grants certificate enrollment rights to low-privileged users
- The certificate template defines EKUs that enable authentication:  
	- _Client Authentication (OID 1.3.6.1.5.5.7.3.2)
	- PKINIT Client Authentication (1.3.6.1.5.2.3.4)
	- Smart Card Logon (OID 1.3.6.1.4.1.311.20.2.2)
	- Any Purpose (OID 2.5.29.37.0)
	- no EKU (SubCA).
- The certificate template allows requesters to specify a subjectAltName in the CSR:
	- AD will use the identity specified by a certificate’s **subjectAltName** (SAN) field if it is present.
	- Consequently, if a requester can specify the SAN in a CSR, the requester can **request a certificate as anyone** (e.g., a domain admin user).
	- The certificate template’s AD object specifies if the requester can specify the SAN in its `mspki-certificate-name-flag` property.
	- The `mspki-certificate-name-flag` property is a bitmask and if the `**CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT**` flag is present, a requester can specify the SAN.

#### What the attack is about ####
- These settings allow a **low-privileged user to request a certificate with an arbitrary SAN**, allowing the low-privileged user to authenticate as any principal in the domain via Kerberos or SChannel.
- CBA Patch Bypass makes the AD CS CA insert a `szOID_NTDS_CA_SECURITY_EXT` SID extension value which contains the SID of the requesting user in all certificate requests. The domain controller can use this to compare the SID of the authenticating user (or the SID specified in the SAN) against the SID contained in the `szOID_NTDS_CA_SECURITY_EXT` SID extension.
### Enumeration ###
1. **Certify**
`PS C:\ADCS\Auditor> Certify.exe find`

- `msPKI-Certificate-Name-Flag` is set to `ENROLLEE_SUPPLIES_SUBJECT`
- `pkiextendedkeyusage` is set to `Client Authentication`

### Execution ###
1. **Certify**
- Without CBA Patch Bypass
`PS C:\ADCS\Auditor> .\Certify.exe request /ca:cbp-dc.protectedcb.corp\CBP-CA /domain:protectedcb.corp /template:ProtectedUserAccess /altname:Administrator`

- With CBA Patch Bypass
`PS C:\ADCS\Auditor> .\Certify.exe request /ca:cbp-dc.protectedcb.corp\CBP-CA /domain:protectedcb.corp /template:ProtectedUserAccess /altname:Administrator /sidextension:S-1-5-21-1286082170-882298176-404569034-500`
