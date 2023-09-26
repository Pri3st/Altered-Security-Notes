## User/Computer Account persistence by certificate renewal before expiration ##

#### What's running under the hood ####
- Certificate templates have a Validity Period which determines how long an issued certificate can be used, as well as a Renewal period (usually 6 weeks). This is a window of time before the certificate expires where an account can renew it from the issuing certificate authority.

#### What the attack is about ####
- If an attacker compromises a certificate capable of domain authentication through theft or malicious enrollment, the attacker can authenticate to AD for the duration of the certificate’s validity period.
- The attacker, however, can renew the certificate before expiration.
- This can function as an extended persistence approach that prevents additional ticket enrollments from being requested, which can leave artifacts on the CA server itself.

### Execution ###
1. **certreq**
- We will need the certificate's **Serial Number**.
`PS C:\ADCS\Auditor> certreq -enroll -user -q -PolicyServer * -cert 620000001238d3cbef14353a19000000000012 renew reusekeys`

- Renew a certificate and generate a new key
`PS C:\ADCS\Auditor> certreq -enroll -user -q -PolicyServer * -cert 620000001238d3cbef14353a19000000000012 renew`