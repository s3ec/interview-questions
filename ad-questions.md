

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


Great question — this is exactly what interviewers want:

SMB is not just “file sharing”… it is one of the **main paths to Remote Code Execution (RCE)** in Active Directory environments.

Let me explain clearly:

---

# ✅ How SMB Leads to RCE in AD Environments

SMB (Server Message Block) runs on:

* **Port 445**
* Used for:

  * File shares
  * Remote admin
  * Service control
  * Remote execution

In AD pentesting, SMB becomes RCE through **3 main ways**:

---

# ✅ 1. Remote Service Creation (Classic SMB RCE)

If you have admin credentials on a machine, you can execute commands remotely by creating a service.

### Tools:

* `psexec.py`
* CrackMapExec
* Impacket

### Example:

```bash
psexec.py DOMAIN/user:pass@10.10.10.5
```

### What happens internally:

1. SMB authenticates user
2. Uploads payload to `ADMIN$`
3. Creates a Windows service remotely
4. Service runs as **SYSTEM**
5. You get a shell → RCE

📌 Requirement: Local admin privileges

---

# ✅ 2. Scheduled Task Execution via SMB + RPC

Attackers can remotely create tasks:

```bash
atexec.py DOMAIN/user:pass@target "whoami"
```

This uses SMB for authentication and RPC to execute.

---

# ✅ 3. WMI Execution (Authenticated SMB-based)

Even without writing services, WMI can execute remotely:

```bash
wmiexec.py DOMAIN/user:pass@target
```

This gives “semi-interactive” RCE.

---

# ✅ 4. SMB Share Write → Code Execution

If a share is writable, attacker can drop malicious files:

### Example:

* Startup scripts
* Scheduled task scripts
* GPO logon scripts

```bash
\\dc\SYSVOL\domain\scripts\
```

Drop payload → executed by domain users or admins.

---

# ✅ 5. NTLM Relay → RCE via SMB

Even without credentials, attacker captures NTLM authentication and relays it.

### Attack chain:

1. Responder captures NTLM
2. Relay to SMB target
3. Execute payload remotely

Tool:

```bash
ntlmrelayx.py -t smb://target --exec-method smbexec
```

This can lead to RCE if:

* SMB signing is disabled
* Target allows admin relay

---

# ✅ 6. Exploiting SMB Vulnerabilities (Unauthenticated RCE)

Sometimes SMB itself has vulnerabilities:

### Example:

* EternalBlue (MS17-010)

```bash
nmap --script smb-vuln-ms17-010 target
```

Unauthenticated exploit → SYSTEM shell.

📌 Less common today but still asked in interviews.

---

# ✅ 7. SMB Named Pipes → Privilege Abuse

SMB exposes named pipes used by Windows services:

* spoolss
* lsarpc
* samr

Attackers abuse pipes for:

* coercion attacks (PetitPotam)
* credential relay
* service impersonation

---

# ✅ Summary Table: SMB → RCE Methods

| Method                     | Needs Creds?       | Result                     |
| -------------------------- | ------------------ | -------------------------- |
| PsExec / smbexec           | Yes (Admin)        | SYSTEM shell               |
| WMIExec                    | Yes                | Command execution          |
| Scheduled Tasks            | Yes                | Remote execution           |
| Writable Shares            | Sometimes          | Code execution via scripts |
| NTLM Relay                 | No password needed | RCE if misconfigured       |
| SMB Exploits (EternalBlue) | No                 | Unauth SYSTEM RCE          |
| Named Pipes Abuse          | No/Partial         | Relay + escalation         |

---

# ⭐ Interview Answer (Perfect)

**SMB leads to RCE in AD because it supports remote administration.
Attackers use SMB authentication to upload payloads, create services (PsExec), execute commands via WMI, abuse writable shares, or perform NTLM relay when SMB signing is disabled. SMB vulnerabilities like EternalBlue can also provide unauthenticated RCE.**

---



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

Excellent question — delegation is one of the **most important real AD privilege escalation topics**.

Your answer is correct, but interviewers want the **HOW exactly delegation is abused**.

Let me explain deeply and clearly.

---

# ✅ What is Kerberos Delegation? (Full Explanation)

Kerberos delegation is a feature that allows a service (like IIS, MSSQL) to access other services **on behalf of a user**.

Example:

User logs into a web app → web app accesses SQL server as the user.

Without delegation, the web server cannot reuse the user’s identity.

---

# ✅ Why Delegation Exists?

Delegation solves the “double-hop problem”:

User → Web Server → Database

Kerberos delegation allows the web server to forward authentication tickets.

---

# ✅ Types of Delegation (Very Important)

There are **3 main types**, and each has different pentest abuse.

---

# 🟥 1. Unconstrained Delegation (Most Dangerous)

### What it means:

A service account/computer is trusted to delegate to **any service**.

So Domain Controller will send user’s **TGT** to that machine.

### Key Point:

If a Domain Admin logs into that machine → their TGT is cached.

---

## ✅ How Attackers Abuse It

### Attack Steps:

1. Find computers with unconstrained delegation:

```powershell
Get-ADComputer -Filter {TrustedForDelegation -eq $true}
```

or BloodHound:

* “Find computers with unconstrained delegation”

2. Compromise that machine (local admin)

3. Wait for privileged user (DA) to connect

4. Dump cached Kerberos tickets:

```bash
mimikatz "sekurlsa::tickets"
```

5. Reuse the DA ticket → Domain compromise

---

### Real Result:

🔥 Steal Domain Admin TGT → Full domain takeover

---

# 🟧 2. Constrained Delegation

### What it means:

Service can delegate only to **specific services**, not everything.

Example:

WebServer can delegate only to MSSQL.

---

## ✅ How Attackers Abuse It

If attacker compromises the service account, they can impersonate ANY user to the allowed service.

### Tool: Impacket

```bash
getST.py -spn MSSQLSvc/server.domain.local domain/user:pass
```

Then attacker uses the ticket:

```bash
export KRB5CCNAME=ticket.ccache
psexec.py -k -no-pass target
```

---

### Impact:

Impersonate Domain Admin to MSSQL/HTTP → Privilege escalation

---

# 🟨 3. Resource-Based Constrained Delegation (RBCD) (Most Common in HTB)

### What it means:

Instead of account saying “I can delegate”…

The target machine says:

✅ “I trust THIS computer to act as me”

Controlled via attribute:

* `msDS-AllowedToActOnBehalfOfOtherIdentity`

---

## ✅ How Attackers Abuse RBCD

### Attack Chain:

1. Attacker has control over a computer object (GenericWrite)

2. Attacker creates fake machine account:

```bash
addcomputer.py domain/user:pass -computer-name HACK$ -computer-pass Pass123
```

3. Modify delegation rights:

```bash
rbcd.py -delegate-from HACK$ -delegate-to TARGET$
```

4. Request service ticket as Domain Admin:

```bash
getST.py -impersonate Administrator -spn cifs/target.domain.local
```

5. Execute remotely:

```bash
psexec.py -k -no-pass target
```

---

### Result:

🔥 Full RCE as Administrator without knowing password

---

# ✅ Delegation Abuse Summary Table

| Delegation Type | Abuse Method                             | Result                    |
| --------------- | ---------------------------------------- | ------------------------- |
| Unconstrained   | Steal cached TGTs                        | Domain Admin ticket theft |
| Constrained     | Impersonate users to allowed SPNs        | Priv esc to service       |
| RBCD            | Write delegation attribute + impersonate | Full takeover of machine  |

---

# ⭐ Perfect Interview Answer

**Kerberos delegation allows a service to impersonate users.
Attackers abuse unconstrained delegation by stealing cached TGTs when privileged users log in.
Constrained delegation can be abused using S4U2Proxy to request service tickets as another user.
RBCD is abused by modifying msDS-AllowedToActOnBehalfOfOtherIdentity, allowing attackers to impersonate Domain Admin and gain remote execution.**

---

# ✅ Tools Interviewers Expect

* BloodHound
* Mimikatz
* Impacket (`getST.py`, `psexec.py`)
* Rubeus
* PowerView

---

# Next Interview Question (Delegation Advanced)

**Q:** What is the difference between **S4U2Self** and **S4U2Proxy**, and how are they used in constrained delegation attacks?

Reply with your attempt — I will correct and explain fully.

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

  
Perfect! 🔥 Let’s move into **Expert-Level Active Directory Pentesting**. These are real-world scenarios, deep technical, often multi-step, and what interviewers ask to test full AD exploitation knowledge.

---

# ✅ Expert AD Pentesting – 10 Questions & Answers / Scenarios

---

## **1. Domain Admin via ACL Abuse**

**Question:**
How can you escalate to Domain Admin by abusing misconfigured ACLs without knowing any passwords?

**Answer:**

* Use **BloodHound** to map `GenericAll` / `WriteDACL` permissions.
* Identify users or computers with privilege to modify ACLs on Domain Admin accounts.
* Modify ACLs to give yourself `GenericAll` → reset DA password → gain DA access.

---

## **2. Kerberoasting in Large Domains**

**Question:**
How do you efficiently perform Kerberoasting in a large enterprise environment?

**Answer:**

* Enumerate SPNs with **GetUserSPNs.py (Impacket)** or **PowerView**.
* Request TGS for service accounts (offline crack).
* Optimize cracking with **hashcat / John the Ripper** using custom wordlists and GPU acceleration.
* Prioritize high-privilege SPNs (like admin services).

---

## **3. DCSync Stealth**

**Question:**
How would you perform a DCSync attack stealthily without alerting monitoring tools?

**Answer:**

* Only target specific users, not the entire domain.
* Use **Impacket’s secretsdump.py --just-dc**
* Time requests during low activity to avoid anomalies.
* Avoid domain-wide replication enumeration; grab only high-value accounts.

---

## **4. Active Directory Certificate Services Abuse**

**Question:**
How can AD CS be abused to gain domain admin access?

**Answer:**

* Identify users who can enroll for certificates (ESC1–ESC8).
* Request a certificate for a privileged account.
* Use **Rubeus / Certipy** to request TGS using that cert.
* Authenticate as DA without touching passwords.

---

## **5. Lateral Movement Using RBCD**

**Question:**
How do you perform lateral movement using Resource-Based Constrained Delegation?

**Answer:**

* Find a computer with `msDS-AllowedToActOnBehalfOfOtherIdentity` controlled by attacker.
* Set the ACL to allow your machine to act as DA.
* Impersonate DA via **DCSync / Kerberos tickets**.
* Pivot to high-value machines.

---

## **6. Golden Ticket Persistence**

**Question:**
How do you maintain persistence with a Golden Ticket while avoiding detection?

**Answer:**

* Create a TGT with **KRBTGT hash**.
* Limit lifetime (shorter than default) to avoid anomalies in logs.
* Only use on high-value sessions, not every login.
* Monitor logs for Kerberos anomalies before acting.

---

## **7. Cross-Forest Escalation**

**Question:**
How do you compromise a trusted forest from a lower-privileged domain?

**Answer:**

* Enumerate trusts (`nltest /domain_trusts`, BloodHound).
* Target accounts with trust permissions.
* Abuse SID History or Kerberos referrals.
* Gain foothold in external forest and escalate privileges.

---

## **8. Detecting Unconstrained Delegation Abuse**

**Question:**
How do you exploit a host with unconstrained delegation enabled?

**Answer:**

* Capture Kerberos tickets of users connecting to that host.
* Use **Mimikatz / Rubeus** to extract TGTs from memory.
* Impersonate privileged users (including Domain Admin).
* Pivot using tickets to other hosts.

---

## **9. Active Directory Shadow Credentials**

**Question:**
How can an attacker persist without touching normal user accounts?

**Answer:**

* Use **DCShadow attack** to push rogue objects into AD.
* Modify GPOs, computer accounts, or ACLs to create hidden privileges.
* Changes appear legitimate because DCShadow registers the attacker as a fake DC.

---

## **10. Full Domain Compromise Without Passwords**

**Question:**
Explain a multi-step attack path to full domain compromise without initially knowing any user passwords.

**Answer:**

1. **Recon:** Enumerate domain using LDAP queries (PowerView/BloodHound).
2. **Privilege Mapping:** Identify ACL misconfigurations.
3. **Kerberoasting:** Crack high-value service accounts.
4. **DCSync / RBCD:** Escalate privileges.
5. **Golden/Silver Tickets:** Maintain persistence.
6. **Lateral Movement:** Use PsExec, WMI, WinRM, RDP.
7. **Cleanup:** Remove traces from logs, ensure stealth.

---



