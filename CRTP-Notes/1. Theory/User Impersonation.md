#### 1. Pass the Hash (PtH) ####
- Pass the hash is a technique that allows an attacker to authenticate to a Windows service using the NTLM hash of a user's password.

#### 2. Pass the Ticket (PtT) ####
- Pass the ticket is a technique that allows an attacker to add Kerberos tickets to an existing logon session or a new one.
- Accessing a remote resource will then allow that authentication to happen via Kerberos.

#### 3. OverPass the Hash (OPtH) ####
- Overpass the hash is a technique which allows an attacker to request a Kerberos TGT for a user, using their NTLM or AES hash.
- Elevated privileges are required to obtain user hashes, but not to actually request a ticket.
- Using an NTLM hash results in a ticket encrypted using RC4. This is considered a legacy encryption type and therefore often stands out as anomalous in a modern Windows environment.

#### 4. Token Impersonation ####
- Token impersonation allows an attacker to steal the access token of a logged-on user on the system without knowing their credentials and impersonate them to perform operations with their privileges.
- The impersonation technique requires the attacker to gain local admin privileges on the compromised machine to steal its tokens.
- There are two types of tokens related to the token impersonation technique — **Delegation and Impersonation**:
	- **Delegation tokens** are created when users interactively login into a system using their credentials. The interactive logins can be physical or remote as a Remote Desktop with RDP or VNC. Delegation tokens contain authentication credentials.
	- **Impersonation tokens are** created when users non-interactively login into a system, like accessing a shared drive on the network. Users usually don’t get prompted for credentials when accessing the share; instead, they use their tokens for the access.Impersonation tokens are usually generated after the delegation tokens. Non-interactive authentication uses established credentials from an interactive authentication.

#### 5. Pass the Certificates ####
- Pass the Certificate is the fancy name given to the pre-authentication operation relying on a certificate (i.e. key pair) to pass in order to obtain a TGT.
- Keep in mind a certificate in itself cannot be used for authentication without the knowledge of the private key. A certificate is signed for a specific public key, that was generated along with a private key, which should be used when relying on a certificate for authentication.
- The "certificate + private key" pair is usually used in the following manner
	- PEM certificate + PEM private key
	- PFX certificate export (which contains the private key) + PFX password (which protects the PFX certificate export)