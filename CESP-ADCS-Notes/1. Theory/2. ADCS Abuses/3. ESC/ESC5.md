## Poor Access Control (GenericWrite) on CA Server Computer Object ##

#### What's running under the hood ####
- The web of interconnected ACL based relationships that can affect the security of AD CS is extensive. Several **objects outside of certificate** templates and the certificate authority itself can have a **security impact on the entire AD CS system**. These possibilities include (but are not limited to):
- The **CA server’s AD computer object** (i.e., compromise through S4U2Self or S4U2Proxy)
- The **CA server’s RPC/DCOM server**
- Any **descendant AD object or container in the container** `CN=Public Key Services,CN=Services,CN=Configuration,DC=<DOMAIN>,DC=<COM>` (e.g., the Certificate Templates container, Certification Authorities container, the NTAuthCertificates object, the Enrollment Services Container, etc.)

#### What the attack is about ####
- If a low-privileged attacker can gain control over any of these, the attack can likely **compromise the PKI system**.

### Enumeration ###
1. PowerView
2. BloodHound
3. LDAP Queries

### Execution ###
- Compromising the CA server's computer object using a technique such as RBCD/Shadow Credentials to gain admin access.
- ACLs misconfigured for a descendant AD object (the Certificate Templates container, Certification Authorities container, the NTAuthCertificates object) allowing for Domain Persistence.
- The CA server’s RPC/DCOM server to configure AD CS misconfigurations for later abuse.