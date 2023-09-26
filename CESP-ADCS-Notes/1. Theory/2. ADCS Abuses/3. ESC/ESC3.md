## Request an enrollment agent certificate (Application Policy - Certificate Request Agent) and use it to request a cert on behalf of ANY user (Certificate Request Agent EKU) ##

#### What's running under the hood ####
- This scenario is like [[ESC1]]  and [[ESC2]].
- Now we are abusing a different EKU (Certificate Request Agent) and 2 different templates. therefore it has 2 sets of requirements.
- The **Certificate Request Agent EKU** (OID 1.3.6.1.4.1.311.20.2.1), known as **Enrollment Agent** in Microsoft documentation, allows a principal to enroll for a certificate on behalf of another user.
- The enrollment agent enrolls in such a template and uses the resulting certificate to co-sign a CSR on behalf of the other user.
- It then sends the co-signed CSR to the CA, enrolling in a template that permits enroll on behalf of, and the CA responds with a certificate belong to the other user.
- Enterprise CAs can **constrain** the **users** who can obtain an enrollment agent certificate, the templates enrollment agents can enroll in, and which accounts the enrollment agent can act on behalf of.
- However, the default CA setting is “Do not restrict enrollment agents”. Even when administrators enable “Restrict enrollment agents”, the default setting is extremely permissive, allowing Everyone access enroll in all templates as anyone.
- **Requirements 1:**
	1. The Enterprise CA allows low-privileged users enrollment rights.
	2. Manager approval is disabled.
	3. No authorized signatures are required.
	4. An overly permissive certificate template security descriptor allows certificate enrollment rights to low-privileged users.
	5. The **certificate template defines the Certificate Request Agent EKU**. The Certificate Request Agent OID (1.3.6.1.4.1.311.20.2.1) allows for requesting other certificate templates on behalf of other principals.
- **Requirements 2:**
	1. The Enterprise CA allows low-privileged users enrollment rights.
	2. Manager approval is disabled.
	3. **The template schema version 1 or is greater than 2 and specifies an Application Policy Issuance Requirement requiring the Certificate Request Agent EKU.**
	4. The certificate template defines an EKU that allows for domain authentication.
	5. Enrollment agent restrictions are not implemented on the CA.

#### What the attack is about ####
- These settings allow a **low-privileged user to request a certificate with an arbitrary SAN**, allowing the low-privileged user to authenticate as any principal in the domain via Kerberos or SChannel.
### Enumeration ###
1. **Certify**
`PS C:\ADCS\Auditor> .\Certify.exe find`
- Agent Template
	- `pkiextendedkeyusage` is set to `Certificate Request Agent`
- Non-Agent Template
	- `Application Policies` is set to `Certificate Request Agent`
	- `pkiextendedkeyusage` is set to `Client Authentication`

### Execution ###
- Request an enrollment agent certificate
`PS C:\ADCS\Auditor> .\Certify.exe request /ca:CORPDC01.CORP.LOCAL\CORP-CORPDC01-CA /template:Vuln-EnrollmentAgent`

- Enrollment agent certificate to issue a certificate request on behalf of another user to a template that allow for domain authentication
`PS C:\ADCS\Auditor> .\Certify.exe request /ca:CORPDC01.CORP.LOCAL\CORP-CORPDC01-CA /template:User /onbehalfof:CORP\itadmin /enrollcert:enrollmentcert.pfx /enrollcertpwd:asdf`