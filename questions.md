Absolutely 🔥 Here are 10 Advanced Active Directory Pentesting Interview Questions with Answers.

  

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
