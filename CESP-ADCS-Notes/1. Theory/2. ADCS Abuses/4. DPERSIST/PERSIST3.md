## Backdoor CA server using malicious misconfigurations like ESC4 that can later cause a domain escalation ##

#### What the attack is about ####
- There are many opportunities for persistence via security descriptor modifications of AD CS components.
- Any scenario described in the Domain Escalation section could be maliciously implemented by an attacker with elevated access, as well as addition of “control rights'' (i.e., `WriteOwner`/`WriteDACL`/etc.) to sensitive components.