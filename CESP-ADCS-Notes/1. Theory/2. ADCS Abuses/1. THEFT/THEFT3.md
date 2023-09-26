## Extracting User certificates and private keys using DPAPI ##

#### What's running under the hood ####
- As seen in [[THEFT2]], DPAPI is used to protect certificate private keys.
- Windows stores machine certificates in the registry key `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\SystemCertificates` and stores private keys in several different places depending on the account.
- Some useful registry entries to note for Machine private keys are:
	- `%ALLUSERSPROFILE%\Application Data\Microsoft\Crypto\RSA\MachineKeys` (CAPI)
	- `%ALLUSERSPROFILE%\Application Data\Microsoft\Crypto\Keys` (CNG)
	- These **private keys** are associated with the **machine certificate** store and Windows encrypts them with the **machine’s DPAPI master keys**. One cannot decrypt these keys using the domain’s DPAPI backup key, 

#### What the attack is about ####
- This attack is similar to [[THEFT2]] except that now we target the Machine certificate store.
- We **CANNOT** use the domain DPAPI backup key to decrypt Machine Keys but rather use the **DPAPI_SYSTEM LSA secret** on the system which is accessible only by the SYSTEM user.

### Enumeration ###
- SharpDPAPI’s `certificates` command will search user encrypted DPAPI certificate private keys.
`.\SharpDPAPI.exe certificates`

### Exfiltration ###
- Using SharpDPAPI's `certificates` command with the `/password` option allows to decrypt users masterkeys required for the corresponding certificate private key decryption.
`.\SharpDPAPI.exe certificates /password:W@rehouse0fdeadCertificates!`