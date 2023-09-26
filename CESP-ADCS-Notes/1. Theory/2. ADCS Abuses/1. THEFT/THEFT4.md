## Theft of existing certificates on-disk ##

#### What's running under the hood ####
- Sometimes certificates and their associated private keys are just in the file system, like in file shares or in the Downloads folder, due to careless handling of such sensitive files.

#### What the attack is about ####
- To search for such files, we target recursively searching for common certificate file extensions on disk.
- We can use manual PowerShell and CMD search queries or use tools such as Seatbelt to automate the searching process.
- We can enumerate for the following critical extensions that may help us find and compromise certificates/private keys on disk.
	- `.key`: Contains just the private key
	- `.crt`/`.cer`: Contains just the certificate
	- .`csr`: Certificate signing request file. This does not contain certificates or keys
	- `.jks`/`.keystore`/`.keys`: Java Keystore. May contain certs + private keys used by Java applications
	- `.pem`: Contains certificate and associated private key (unprotected)
	- `.pfx`/`.p12`: Contains certificate and associated private key (protected)

### Enumeration ###
1. **cmd**
`c:\ADCS\Auditor> dir C:\*.pfx C:\*.pem C:\*.p12 C:\*.crt C:\*.cer C:\*.p7b /s /b`

2. PowerShell
`PS C:\ADCS\Auditor> Get-ChildItem -Recurse -Path C:\ -Include *.pfx, *.pem, *.p12, *.crt, *.cer, *.p7b -ErrorAction SilentlyContinue -Force`

### Exfiltration ###
- Convert the certificate (`.pem` format) to `.pfx` or directly use it (`.pfx`/`.p12` format).