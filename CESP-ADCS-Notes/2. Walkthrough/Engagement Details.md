### End Goal ###
- Obtain EA privileges.
- Map all possible attack paths.
- Showcase business impact.

### In-Scope Assets ###
- Initial domain: `certbulk.cb.corp`
- All other networks and domains that are reachable during the engagement.

### Out-of-Scope Assets ###
- Brute-force attacks.

### Resources Provided ###
- Domain foothold
	- Domain-joined Windows machine -> **CB-WS33**
	- Domain low-privileged account -> `certbulk\student33`

### Methodology ###
- Compromise domain users and computers until EA privileges are gained.
- In each compromised computer apply all the THEFT TTPs to extract certificates that could further allow account impersonation and movement to the domain.
- In each compromised computer search for credentials/tickets that could further allow account impersonation and movement to the domain.
- For each compromised account request a certificate from the CA so we can easily impersonate them during the engagement.
- For each compromised account search possible lateral/vertical movement capabilities.
- Discover each possible path that could result in EA privileges.
- Be deliberate.