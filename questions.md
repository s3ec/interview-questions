

---

# ✅ Top 30 Active Directory Pentesting Interview Questions & Answers

---

## **1. What is Active Directory?**

**Answer:**
AD is a Microsoft directory service for managing users, computers, groups, authentication, and policies in a Windows domain.

---

## **2. What is a Domain Controller?**

**Answer:**
A DC hosts AD DS, authenticates logins, enforces policies, and stores the AD database.

---

## **3. What is the difference between Kerberos and NTLM?**

**Answer:**
Kerberos uses tickets and is more secure; NTLM is older and hash-based, used mainly for legacy fallback.

---

## **4. What is LDAP used for?**

**Answer:**
LDAP is the protocol used to query and interact with AD objects like users, groups, computers.

---

## **5. What is SMB in AD environments?**

**Answer:**
SMB provides file sharing, remote execution, and is commonly used for lateral movement.

---

## **6. What is Kerberoasting?**

**Answer:**
Request TGS tickets for SPN accounts and crack them offline to recover service account passwords.

---

## **7. What is AS-REP Roasting?**

**Answer:**
Targets accounts with pre-authentication disabled and cracks AS-REP responses offline.

---

## **8. What is Pass-the-Hash?**

**Answer:**
Using NTLM hashes directly to authenticate without knowing the plaintext password.

---

## **9. What is Pass-the-Ticket?**

**Answer:**
Reusing stolen Kerberos tickets (TGT/TGS) to impersonate users.

---

## **10. What is a Golden Ticket?**

**Answer:**
A forged Kerberos TGT created using KRBTGT hash giving full domain access.

---

## **11. What is a Silver Ticket?**

**Answer:**
A forged service ticket created using service account hash to access specific services.

---

## **12. What is DCSync?**

**Answer:**
Abusing replication permissions to dump password hashes from the Domain Controller.

---

## **13. What is DCShadow?**

**Answer:**
Registering a rogue DC to push stealthy malicious changes into AD.

---

## **14. What is BloodHound used for?**

**Answer:**
Maps AD relationships and privilege escalation paths to Domain Admin.

---

## **15. What is an ACL in AD?**

**Answer:**
Permissions list controlling access to AD objects. Misconfigs lead to escalation.

---

## **16. What is AdminSDHolder?**

**Answer:**
Protects privileged accounts; attackers abuse it for persistence via ACL modification.

---

## **17. What is Kerberos Delegation?**

**Answer:**
Allows services to impersonate users. Misuse leads to ticket theft and escalation.

---

## **18. What is Unconstrained Delegation?**

**Answer:**
A host can cache TGTs of connecting users; attacker can steal DA tickets.

---

## **19. What is Constrained Delegation?**

**Answer:**
Delegation limited to specific services; safer but still exploitable if misconfigured.

---

## **20. What is RBCD?**

**Answer:**
Resource-Based Constrained Delegation allows attacker-controlled computer to impersonate users to a target.

---

## **21. What is AD CS?**

**Answer:**
Active Directory Certificate Services issues certificates; misconfigs allow privilege escalation.

---

## **22. What is ESC1 in AD CS?**

**Answer:**
Low-privileged user can request certificate for any user → become Domain Admin.

---

## **23. What is SMB Relay?**

**Answer:**
Relaying intercepted NTLM authentication to another machine when SMB signing is disabled.

---

## **24. What is NTLMv2 cracking?**

**Answer:**
Capturing challenge-response hashes and cracking offline.

---

## **25. What is LSASS dumping?**

**Answer:**
Extracting credentials/tickets from LSASS memory using Mimikatz.

---

## **26. What is Credential Guard?**

**Answer:**
Windows defense that isolates secrets, blocking LSASS dumping.

---

## **27. What is Lateral Movement?**

**Answer:**
Moving from one machine/account to another using SMB, WinRM, RDP, WMI.

---

## **28. What is a Trust Relationship?**

**Answer:**
Allows authentication between domains/forests; attackers abuse trusts for cross-domain escalation.

---

## **29. What is SID History abuse?**

**Answer:**
Injecting privileged SID into another account to gain elevated access across trusts.

---

## **30. What are top tools in AD pentesting?**

**Answer:**

* BloodHound
* Impacket
* Rubeus
* Mimikatz
* CrackMapExec
* Certipy
* PowerView

---

Absolutely 🔥 Great catch.

Yes — the previous list focused more on Kerberos/ACL/ADCS, but in real AD pentesting interviews, they *expect* you to know classic internal network attacks like:

✅ LLMNR/NBT-NS Poisoning
✅ IPv6 Attacks
✅ WPAD
✅ NetBIOS Enumeration
✅ Responder + NTLM Capture
✅ Relay Chains

So here is an **Updated Expert Add-on Set**:

## **15 Important AD Attacks I Missed (With Q&A)**

---

# ✅ AD Pentesting Attacks Often Asked in Interviews (LLMNR, IPv6, NetBIOS)

---

## **1. What is LLMNR Poisoning?**

**Answer:**
LLMNR (Link-Local Multicast Name Resolution) is used when DNS fails.
Attackers spoof responses to capture NTLM hashes.

Tool: **Responder**

---

## **2. What is NBT-NS Poisoning?**

**Answer:**
NetBIOS Name Service works like LLMNR for older systems.
Attackers poison it to redirect traffic and steal credentials.

---

## **3. Why are LLMNR/NBT-NS dangerous?**

**Answer:**
Because Windows systems broadcast name lookups, attackers can reply first and capture NTLM authentication attempts.

---

## **4. What tool is used for LLMNR + NTLM hash capture?**

**Answer:**
✅ Responder

Example:

```bash
responder -I eth0
```

---

## **5. What is NTLM Relay Attack?**

**Answer:**
Instead of cracking hashes, attacker relays NTLM authentication to another machine to gain access.

Tool: **ntlmrelayx.py**

---

## **6. When does NTLM relay work?**

**Answer:**

* SMB signing disabled
* LDAP signing not enforced
* NTLM allowed in environment

---

## **7. What is WPAD Attack?**

**Answer:**
WPAD auto-configures proxy settings.
Attackers spoof WPAD responses to force victims to authenticate and leak hashes.

---

## **8. What is IPv6 DNS Takeover Attack?**

**Answer:**
Even if IPv4 is secure, Windows prefers IPv6.
Attackers run fake IPv6 router + DNS and redirect traffic.

Tool: **mitm6**

---

## **9. How does mitm6 work?**

**Answer:**

* Advertises attacker as IPv6 gateway
* Victims send DNS queries
* Captures credentials via LDAP/SMB relay

---

## **10. mitm6 + relay chain gives what?**

**Answer:**
Full Domain compromise via:

* LDAP relay
* Adding attacker to Domain Admin
* Dumping secrets

---

## **11. What is NetBIOS Enumeration?**

**Answer:**
NetBIOS reveals hostnames, shares, domain info.

Commands:

```bash
nbtscan 10.0.0.0/24
nmap --script nbstat
```

---

## **12. What is SMB Signing and why important?**

**Answer:**
SMB signing prevents NTLM relay.
If disabled → attacker can relay SMB authentication.

---

## **13. What is LDAP Relay Attack?**

**Answer:**
Relaying NTLM authentication to LDAP allows:

* Adding users to groups
* Creating machine accounts
* Privilege escalation

---

## **14. What is PrinterBug Attack?**

**Answer:**
Forces a machine (like DC) to authenticate outward, enabling relay.

Tool:

```bash
printerbug.py
```

---

## **15. What is PetitPotam Attack?**

**Answer:**
Triggers NTLM authentication from Domain Controller using MS-EFSRPC.

Used with relay → Domain Admin takeover.

---

# ✅ Summary: Classic Internal AD Attack Chain

🔥 Most real-world AD compromises follow:

1. LLMNR/NBT-NS Poisoning (Responder)
2. Capture NTLM hashes
3. NTLM Relay (ntlmrelayx)
4. LDAP Relay → Priv Esc
5. Dump creds → Lateral Movement
6. Kerberos abuse → Domain Admin

---

# ✅ Interview Golden Tip

> “Even if Kerberos is strong, AD environments fall because of legacy protocols like NTLM, LLMNR, NetBIOS, WPAD, and IPv6 misconfigurations.”

---


  

✅ Advanced AD Pentesting – 10 Questions & Answers
=================================================

  

1\. What is DCSync attack and why is it critical?
-----------------------------------------------------

Answer:  
DCSync abuses AD replication rights to simulate a Domain Controller and request password hashes from the DC.

Attackers need privileges like:

   Replicating Directory Changes
   Replicating Directory Changes All

It can dump hashes of Domain Admins and KRBTGT.

  

2\. What is DCShadow attack?
--------------------------------

Answer:  
DCShadow allows an attacker to register a rogue Domain Controller and push malicious changes into AD, such as adding privileges or modifying objects stealthily.

Harder to detect than DCSync.

  

3\. Explain AD Certificate Services (AD CS) abuse (ESC1–ESC8).
------------------------------------------------------------------

Answer:  
AD CS misconfigurations allow attackers to request certificates for other users and authenticate as Domain Admin.

Example:

   ESC1: Low user can enroll certificate for any user → privilege escalation

Tool: Certipy

  

4\. What is Resource-Based Constrained Delegation (RBCD)?
-------------------------------------------------------------

Answer:  
RBCD allows a computer account to impersonate users to a target service if attacker can control:

   msDS-AllowedToActOnBehalfOfOtherIdentity

Attackers can escalate from machine account → Domain Admin.

Tool: Impacket + Rubeus

  

5\. What is Unconstrained Delegation and why is it dangerous?
-----------------------------------------------------------------

Answer:  
With unconstrained delegation, a compromised server can cache Kerberos TGTs of users who connect to it.

If a Domain Admin connects → attacker can steal DA TGT.

  

6\. What is AdminSDHolder and SDProp abuse?
-----------------------------------------------

Answer:  
AdminSDHolder protects privileged groups (Domain Admins).

Attackers can modify AdminSDHolder ACLs to gain persistent privileged access, which is reapplied every 60 minutes by SDProp.

  

7\. What is Skeleton Key attack?
------------------------------------

Answer:  
Skeleton Key injects a malicious master password into LSASS so attackers can log in as any user with a universal backdoor password.

Requires Domain Admin-level compromise.

  

8\. What is the difference between Golden Ticket vs Diamond Ticket?
-----------------------------------------------------------------------

Answer:

   Golden Ticket: Forged TGT signed with KRBTGT hash (persistent access)
   Diamond Ticket: Modified legitimate TGT (harder to detect)

Diamond is stealthier because it starts from a valid ticket.

  

9\. How do attackers abuse Trust Relationships between domains/forests?
---------------------------------------------------------------------------

Answer:  
If two domains trust each other, attackers can escalate across domains using:

   SID History injection
   Kerberos referrals
   Compromising one domain → pivot to another

Known as Cross-domain escalation.

  

10\. What is LSASS dumping and how do modern defenses block it?
-------------------------------------------------------------------

Answer:  
LSASS dumping extracts credentials from memory.

Tools:

   Mimikatz
   Procdump
   HandleKatz

Defenses:

   Credential Guard
   Protected Process Light (PPL)
   EDR detection
   Disable WDigest

  

✅ Advanced Tools Commonly Mentioned
===================================

   Rubeus
   Mimikatz
   Impacket
   Certipy
   BloodHound
   SharpHound
   PowerView

  

🔥 That’s the Advanced AD Pentesting Set.

If you want, I can next provide:  
✅ 10 Expert-Level Real-World AD Attack Scenarios  
or a full AD Pentesting Interview Mock (Q&A style)
