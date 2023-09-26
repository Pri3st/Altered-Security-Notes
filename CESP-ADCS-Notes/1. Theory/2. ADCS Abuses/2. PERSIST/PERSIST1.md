## User account persistence using new certificate requests ##

#### What's running under the hood ####
- A certificate can be used for authentication as a user as long as the certificate is valid, even if the user changes their password.
- The **`User`** template allows domain users to request certificates that allow domain authentication and comes by default.
#### What the attack is about ####
- If we compromise a user who has enrollment rights to an AD CS template that has the Client Authentication EKU enabled, we can request and use a certificate that will be valid until the expiry specified in the template.

### Execution ###
- **Certify**
`PS C:\ADCS\Auditor> .\Certify.exe request /ca:cb-ca.cb.corp\CB-CA /template:User /user:studentadmin`