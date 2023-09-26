## Extracting User certificates and private keys using DPAPI ##

#### What's running under the hood ####
- Windows stores certificate private keys using **DPAPI**.
- DPAPI performs symmetric encryption of asymmetric private keys, using a user or system secret as a significant contribution of entropy.
- DPAPI allows developers to encrypt keys using a symmetric key derived from the user's logon secrets, or in the case of system encryption, using the system's domain authentication secrets.
- DPAPI is useful protecting data like Browser Cookies, Login Data, Windows Credential Manager, Vault and certificates/private keys.
- DPAPI is also used to protect certificate private keys.
- Different storage locations are used for user and machine private keys.
- Windows most commonly stores user certificates in the registry in the key `HKEY_CURRENT_USER\SOFTWARE\Microsoft\SystemCertificates`, though some personal certificates for users are also stored in `%APPDATA%\Microsoft\SystemCertificates\My\Certificates`.
- The associated user private key locations are primarily at `%APPDATA%\Microsoft\Crypto\RSA\User SID\` for CAPI keys and `%APPDATA%\Microsoft\Crypto\Keys\` for CNG keys.

#### What the attack is about ####
- To obtain a user certificate and its private key using DPAPI manually, we need to:
	- Map the target certificate in the user’s certificate store and get the key store name.
	- Find and Extract the DPAPI masterkey needed to decrypt the associated private key. To obtain a specific DPAPI masterkey in plaintext using mimikatz we can perform one of the following:
		- Use a domain’s DPAPI backup key. This key can decrypt any domain user’s masterkey file. If an adversary obtains domain admin (or equivalent) privileges, the domain backup key can be stolen and used to decrypt any domain user masterkey in plaintext.
		- Decrypt the masterkey using the corresponding user's password
	- Obtain the plaintext DPAPI masterkey and use it to decrypt the private key.

### Enumeration ###
- SharpDPAPI’s `certificates` command will search user encrypted DPAPI certificate private keys.
`.\SharpDPAPI.exe certificates`

### Exfiltration ###
- Using SharpDPAPI's `certificates` command with the `/password` option allows to decrypt users masterkeys required for the corresponding certificate private key decryption.
`.\SharpDPAPI.exe certificates /password:W@rehouse0fdeadCertificates!`