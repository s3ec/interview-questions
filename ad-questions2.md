

# ✅ Top 50 Active Directory Pentesting Interview Questions + Answers

(Including Kerberos Authentication Deep)

---

# 🟢 Kerberos & Authentication (1–15)

---

## **1. What is Kerberos in Active Directory?**

**Answer:**
Kerberos is the default AD authentication protocol that uses tickets instead of sending passwords over the network.

---

## **2. Explain the Kerberos authentication flow.**

**Answer:**

1. User requests TGT from KDC
2. KDC issues TGT
3. User requests TGS for a service
4. KDC issues service ticket
5. User accesses service using ticket

---

## **3. What is a KDC?**

**Answer:**
Key Distribution Center runs on the Domain Controller and issues Kerberos tickets.

---

## **4. What is a TGT?**

**Answer:**
Ticket Granting Ticket proves identity and is used to request service tickets.

---

## **5. What is a TGS?**

**Answer:**
Ticket Granting Service ticket is used to access a specific service (SMB, MSSQL, HTTP).

---

## **6. What is the KRBTGT account?**

**Answer:**
A special AD account that signs all Kerberos TGT tickets. Compromise leads to Golden Ticket attacks.

---

## **7. Difference between NTLM and Kerberos?**

**Answer:**

* NTLM uses hashes
* Kerberos uses tickets + mutual authentication
  Kerberos is stronger and more secure.

---

## **8. What is Kerberoasting?**

**Answer:**
Requesting SPN service tickets and cracking them offline to recover service account passwords.

---

## **9. What is AS-REP Roasting?**

**Answer:**
Attacking users with Kerberos pre-auth disabled by requesting AS-REP responses and cracking offline.

---

## **10. What is Pass-the-Ticket?**

**Answer:**
Reusing stolen Kerberos tickets to impersonate users.

---

## **11. What is Pass-the-Hash?**

**Answer:**
Authenticating using NTLM hashes instead of passwords.

---

## **12. Golden Ticket attack?**

**Answer:**
Forging TGT using KRBTGT hash for full domain persistence.

---

## **13. Silver Ticket attack?**

**Answer:**
Forging service tickets using service account hash to access specific services.

---

## **14. Diamond Ticket attack?**

**Answer:**
Modifying a legitimate TGT (stealthier than Golden Ticket).

---

## **15. What is Kerberos Delegation?**

**Answer:**
Allows services to impersonate users. Misconfigs lead to privilege escalation.

---

---

# 🟢 Enumeration & Recon (16–25)

---

## **16. What are key ports on a Domain Controller?**

**Answer:**
88 Kerberos, 389 LDAP, 445 SMB, 53 DNS, 5985 WinRM.

---

## **17. What is LDAP enumeration?**

**Answer:**
Querying AD objects like users, groups, SPNs.

---

## **18. What is SMB enumeration?**

**Answer:**
Finding shares, users, and lateral movement opportunities.

---

## **19. What is BloodHound used for?**

**Answer:**
Finding privilege escalation paths using AD graph relationships.

---

## **20. What is SharpHound?**

**Answer:**
Collector tool that gathers AD data for BloodHound.

---

## **21. What is SYSVOL?**

**Answer:**
Shared folder storing Group Policy files and scripts.

---

## **22. What is a GPO?**

**Answer:**
Group Policy Object enforces security settings across the domain.

---

## **23. What is SPN?**

**Answer:**
Service Principal Name identifies service accounts for Kerberos.

---

## **24. What is a Trust in AD?**

**Answer:**
Relationship allowing authentication between domains/forests.

---

## **25. What is NetBIOS used for?**

**Answer:**
Legacy name resolution, often abused for enumeration.

---

---

# 🟢 Credential Attacks (26–35)

---

## **26. What is Responder used for?**

**Answer:**
LLMNR/NBT-NS poisoning to capture NTLM hashes.

---

## **27. What is LLMNR poisoning?**

**Answer:**
Spoofing name resolution responses to steal credentials.

---

## **28. What is NTLM Relay?**

**Answer:**
Relaying NTLM authentication to another service instead of cracking hashes.

---

## **29. What is mitm6 attack?**

**Answer:**
IPv6 DNS takeover allowing credential relay to LDAP.

---

## **30. What is PetitPotam?**

**Answer:**
Forces DC authentication outward, enabling NTLM relay.

---

## **31. What is PrinterBug?**

**Answer:**
Forces machine authentication for relay attacks.

---

## **32. What is LSASS dumping?**

**Answer:**
Extracting credentials/tickets from memory using Mimikatz.

---

## **33. What is Credential Guard?**

**Answer:**
Defense that blocks LSASS credential dumping.

---

## **34. What is SAM dumping?**

**Answer:**
Extracting local password hashes from SAM database.

---

## **35. What is DPAPI abuse?**

**Answer:**
Decrypting stored secrets like browser passwords and RDP creds.

---

---

# 🟢 Privilege Escalation (36–45)

---

## **36. What is ACL abuse?**

**Answer:**
Exploiting misconfigured permissions like GenericAll, WriteDACL.

---

## **37. What is AdminSDHolder?**

**Answer:**
Protects privileged accounts; attackers abuse it for persistence.

---

## **38. What is DCSync?**

**Answer:**
Abusing replication permissions to dump domain password hashes.

---

## **39. What is DCShadow?**

**Answer:**
Rogue DC attack pushing stealthy changes into AD.

---

## **40. What is RBCD?**

**Answer:**
Delegation abuse allowing attacker machine to impersonate users.

---

## **41. Unconstrained delegation risk?**

**Answer:**
Caches user TGTs → attacker steals Domain Admin tickets.

---

## **42. Constrained delegation abuse?**

**Answer:**
Misconfigured delegation allows impersonation to specific services.

---

## **43. What is ADCS abuse?**

**Answer:**
Certificate template misconfigs allow privilege escalation.

---

## **44. What is ESC1?**

**Answer:**
Low user can request certificate as Domain Admin.

---

## **45. What is Golden Ticket persistence?**

**Answer:**
Permanent DA access until KRBTGT reset twice.

---

---

# 🟢 Post Exploitation & Defense (46–50)

---

## **46. What is lateral movement?**

**Answer:**
Moving through hosts using SMB, WinRM, WMI, RDP.

---

## **47. What is secretsdump.py used for?**

**Answer:**
Dumping NTLM hashes and Kerberos keys from DC.

---

## **48. How do defenders detect AD attacks?**

**Answer:**
Using SIEM, Windows Event Logs, Defender for Identity.

---

## **49. Key mitigations against relay attacks?**

**Answer:**
Disable NTLM, enable SMB signing, disable LLMNR, enforce LDAP signing.

---

## **50. What is the AD pentest kill chain?**

**Answer:**
Recon → Initial Access → Credential Theft → Lateral Movement → Priv Esc → Domain Compromise → Persistence.

---




# ✅ Latest AD Attacks (2025) — Interview Questions + Answers

---

# **1. What is Kerberos Relay over HTTP and why is it dangerous?**

### ✅ Answer:

Kerberos relay over HTTP is a modern variant where attackers relay Kerberos authentication to HTTP-based services such as:

* ADCS Web Enrollment
* SCCM endpoints
* Internal IIS apps

Even if NTLM relay is blocked, Kerberos can still be relayed using DNS/LLMNR poisoning.

📌 Dangerous because it bypasses classic NTLM protections and enables certificate-based escalation.

---

# **2. What is CVE‑2025‑24054 and how is it exploited?**

### ✅ Answer:

CVE‑2025‑24054 is an NTLM hash disclosure vulnerability.

Attackers send phishing files like:

* `.library-ms`
* `.url`

When opened, Windows automatically connects to an attacker SMB share:

`\\attacker\share`

This leaks NTLMv2 hashes → usable for:

* Pass-the-hash
* NTLM relay

---

# **3. How does phishing lead to NTLM credential leakage in AD?**

### ✅ Answer:

Windows will automatically attempt authentication when accessing:

* UNC paths
* WebDAV shares
* Proxy auto-config files

So phishing can trigger silent NTLM authentication, captured using:

* Responder
* Inveigh
* SMB listener

---

# **4. What is CVE‑2025‑21293 AD DS Privilege Escalation?**

### ✅ Answer:

This vulnerability allows users in the **Network Configuration Operators** group to write into SYSTEM registry locations.

That leads to:

* Local privilege escalation
* SYSTEM access
* Possible DC compromise

📌 Important because low-privileged operators are often ignored.

---

# **5. What is NTLM Reflection / Coercion Attack (2025 trend)?**

### ✅ Answer:

Reflection attacks force authentication from a machine/user and relay it back into the same environment.

This bypasses mitigations like:

* SMB signing
* Channel binding (in some cases)

Often combined with coercion bugs like:

* PetitPotam
* PrinterBug
* DFSCoerce

---

# **6. Why is IPv6 still one of the biggest AD weaknesses?**

### ✅ Answer:

Because IPv6 is enabled by default in Windows, even when admins secure IPv4.

Attackers run:

```bash
mitm6 -d domain.local
```

Victims request IPv6 DNS → attacker becomes DNS server → WPAD poisoning → credential relay.

---

# **7. What is LDAP/LDAPS Relay and why is it critical?**

### ✅ Answer:

NTLM relay to LDAP allows attackers to perform AD actions like:

* Add user to Domain Admins
* Modify ACLs
* Enroll certificates (ADCS)

Even LDAPS can be attacked if protections are weak.

Tool:

```bash
ntlmrelayx.py --target ldap://dc
```

---

# **8. How does LDAP Relay become ADCS escalation?**

### ✅ Answer:

Relay authentication to ADCS enrollment endpoint:

* Attacker requests certificate as victim
* Uses cert for authentication
* Becomes Domain Admin

This is one of the most dangerous modern chains:

**Relay → Cert → DA**

---

# **9. What is Unauthorized DNS Record Creation abuse in AD?**

### ✅ Answer:

By default, authenticated users can create DNS records.

Attackers register records like:

* `wpad.domain.local`
* `dc.domain.local`

Then poison traffic to capture hashes or perform relay attacks.

---

# **10. What is WPAD poisoning and how is it used today?**

### ✅ Answer:

WPAD = Web Proxy Auto Discovery

Windows searches for:

```
http://wpad.domain/wpad.dat
```

Attackers spoof WPAD → victims authenticate → NTLM captured/relayed.

Still very common in 2025 internal pentests.

---

# **11. What is Win-DDoS using Domain Controllers (CVE‑2025‑32724)?**

### ✅ Answer:

Attackers exploit LDAP referral behavior to weaponize Domain Controllers into making outbound connections.

Result:

* Domain Controller becomes DDoS participant
* Massive amplification attack

Not privilege escalation, but high business impact.

---

# **12. What is LDAP Injection in AD-integrated web apps?**

### ✅ Answer:

Apps that query AD via LDAP may be vulnerable like SQL injection.

Example payload:

`*)(memberOf=*)`

This can lead to:

* Auth bypass
* User enumeration
* AD object manipulation

---

# **13. What is the modern attack chain combining IPv6 + Relay + ADCS?**

### ✅ Answer:

🔥 Realistic 2025 chain:

1. mitm6 poisons DNS
2. Victim authenticates via NTLM
3. ntlmrelayx relays to ADCS
4. Attacker requests DA certificate
5. Full domain compromise

---

# **14. Why are certificate-based attacks the biggest modern AD risk?**

### ✅ Answer:

Because certificates allow passwordless authentication.

Once attacker gets a cert → they can authenticate forever even if password resets.

Tool:

* Certipy

```bash
certipy find
certipy req
```

---

# **15. What are the top defenses against these 2025 attacks?**

### ✅ Answer:

* Disable NTLM where possible
* Enforce SMB signing
* Disable LLMNR/NBT-NS
* Monitor ADCS templates
* Restrict DNS dynamic updates
* Enable LDAP signing + channel binding
* Disable IPv6 if unused
* Patch coercion CVEs

---


**“Interview mode”** OR **“HTB walkthrough”** OR **“Cheat sheet”**
