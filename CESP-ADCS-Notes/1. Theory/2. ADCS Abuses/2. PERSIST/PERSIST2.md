## Computer account persistence using new certificate requests ##

#### What's running under the hood ####
- With SYSTEM rights on a domain joined machine and enrollment rights to a certificate template with Client Authentication EKU, we can request a certificate for the machine account that will be valid even if there is a password change, system wipe etc.
- The `Machine` template allows domain computers to request certificates that allow domain authentication and comes by default.
#### What the attack is about ####
- If an attacker elevates privileges on compromised system, the attacker can use the SYSTEM account to enroll in certificate templates that grant enrollment privileges to machine accounts.
- With access to a machine account certificate, the attacker can then authenticate to Kerberos as the machine account. Using **S4U2Self**, an attacker can then obtain a Kerberos service ticket to any service on the host (e.g., CIFS, HTTP, RPCSS, etc.) as any user.

### Execution ###
- **Certify**
`PS C:\ADCS\Auditor> .\Certify.exe request /ca:cb-ca.cb.corp\CB-CA /template:Machine /machine`