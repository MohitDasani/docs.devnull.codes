# 🔐 Kerberoasting vs AS-REP Roasting

## 🧾 Overview

Both Kerberoasting and AS-REP Roasting are offline Kerberos-based attacks in Active Directory environments aimed at retrieving password hashes and cracking them offline. Here's a detailed comparison:

---

## 🔄 Comparison Table

| Feature/Aspect                  | **Kerberoasting**                                                                 | **AS-REP Roasting**                                                              |
|--------------------------------|-----------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **Purpose**                    | Crack service account passwords by requesting service tickets (TGS)              | Crack user account passwords by requesting AS-REP responses                      |
| **Target Accounts**            | Domain accounts with **Service Principal Names (SPNs)**                         | User accounts with **“Do not require Kerberos pre-authentication”** setting     |
| **Requires Domain Login**      | ✅ Yes – attacker must be an **authenticated user** in the domain                | ❌ No – **unauthenticated attacker** can perform this attack                     |
| **Attack Mechanism**           | Requests a **TGS** for an SPN; ticket is encrypted with the service account’s password hash | Sends an AS-REQ without pre-auth; receives **AS-REP** encrypted with user’s hash |
| **Data Retrieved**             | **TGS ticket**, encrypted using the service account’s NTLM hash                 | **AS-REP**, encrypted using the user's NTLM hash                                 |
| **Hash Cracking Goal**         | Recover **service account password** offline via brute-force or wordlist        | Recover **user password** offline                                                |
| **Vulnerable Setting**         | Weak passwords for SPN-enabled accounts                                         | User accounts with **pre-auth disabled** and weak passwords                     |
| **Common Tools Used**          | - `GetUserSPNs.py` (Impacket) <br> - Rubeus <br> - PowerView                    | - `GetNPUsers.py` (Impacket) <br> - Rubeus                                      |
| **Protocol Used**              | Kerberos TGS request                                                             | Kerberos AS-REQ/AS-REP                                                           |
| **Encryption Algorithm**       | RC4-HMAC (default), or AES if configured                                        | RC4-HMAC                                                                         |
| **Ease of Attack**             | Medium – needs access to domain credentials                                     | Easy – no login required if vulnerable users exist                               |
| **Detection Difficulty**       | Medium – detectable through large numbers of TGS requests                       | Low – fewer logs; but can detect AS-REQ without pre-auth                         |
| **Mitigation Strategies**      | - Strong passwords <br> - Use gMSA/MSA accounts <br> - Ticket monitoring         | - Avoid disabling pre-auth <br> - Strong passwords <br> - Audit account settings |

---

## 🔍 Descriptive Explanation

### 🧨 Kerberoasting – In Detail

- An attacker with **any domain account** can query for **service accounts with SPNs**.
- The attacker then requests a **TGS** for the service using Kerberos.
- The **TGS is encrypted with the NTLM hash** of the service account’s password.
- The attacker dumps the TGS and **brute-forces the hash offline** using tools like Hashcat.
- If the password is weak, it can be cracked, revealing the service account’s credentials.

> 📘 **Example**: A backup service account (`svc_backup`) has an SPN like `MSSQLSvc/db.srv.domain.com`. An attacker gets the TGS for that SPN and cracks it offline.

---

### 🧨 AS-REP Roasting – In Detail

- Some user accounts in AD might have the **“Do not require Kerberos pre-authentication”** option enabled (bad practice).
- An attacker doesn’t need credentials – they just **enumerate users** with that setting.
- The attacker sends a **fake AS-REQ** pretending to be that user.
- AD responds with an **AS-REP message**, encrypted with the user’s password hash.
- The attacker **cracks this AS-REP offline** to recover the user’s password.

> 📘 **Example**: A test user (`test1`) has no pre-auth enabled. An attacker sends an AS-REQ and receives a response encrypted with the NTLM hash, then cracks it.

---

## 🛡️ Summary Comparison

| Attack Type         | Privileged Access   | Account Type Targeted     | Offline Cracking | Ease of Exploitation |
|---------------------|---------------------|----------------------------|------------------|-----------------------|
| **Kerberoasting**   | ✅ Yes               | Service Accounts (SPNs)   | ✅ Yes           | 🟡 Medium              |
| **AS-REP Roasting** | ❌ No (Anonymous)    | Users without pre-auth     | ✅ Yes           | 🟢 Easy                |

---

## 🧰 Recommended Mitigations

### For Both Attacks:
- Enforce **strong, complex passwords**.
- Regularly rotate passwords for service accounts.
- Monitor **Kerberos ticket request anomalies** (e.g., volume of TGS or AS-REQ requests).
- Use **Managed Service Accounts (MSAs/gMSAs)** for automation instead of static service accounts.
- Disable the “Do not require Kerberos preauthentication” flag for all users unless explicitly required.


---

## 🎭 Real-World Analogies

### 🔐 Kerberoasting Analogy

> Imagine you work in a company (domain) and have a valid employee badge (credentials). You know that there's a **locked file cabinet (service)** and anyone can request the **key** from the receptionist — but this key is **encrypted using the cabinet owner’s password**.

As an insider, you request that key, take it home, and try every possible combination (offline cracking) to figure out the cabinet owner's password. If it's weak, you'll break in and now you have access to the **service account's password**.

---

### 🔓 AS-REP Roasting Analogy

> Now imagine someone **outside the company** (unauthenticated attacker) sends a request to the receptionist pretending to be "John from accounting" — who, due to poor policy, doesn't require any ID (no pre-authentication).

The receptionist **hands over an encrypted key**, assuming it's for John. But you, the attacker, use it to try and **crack John's password offline**. If it's weak, you're in — without ever having stepped inside the company.

---

## 🧠 When to Use Which?

| Scenario                                | Recommended Attack        | Why                                                                 |
|-----------------------------------------|----------------------------|----------------------------------------------------------------------|
| You have **domain user credentials**    | ✅ **Kerberoasting**       | You can query SPNs and request TGS tickets with minimal privileges. |
| You have **no credentials at all**      | ✅ **AS-REP Roasting**     | AS-REP roasting can be done anonymously if misconfigured users exist.|
| Target has **many SPN accounts**        | ✅ **Kerberoasting**       | Large ADs often have many exposed service accounts.                 |
| You want **low-noise, quick access**    | ✅ **AS-REP Roasting**     | Fast, easy, no domain access needed, good for stealthy enumeration. |
| Red team in **initial recon phase**     | ✅ **AS-REP Roasting**     | Useful for password spraying with zero credentials.                 |
| Already have **foot in the door**       | ✅ **Kerberoasting**       | Elevate privileges by cracking service accounts.                    |

---

## Quick Detection Command

### Detect AS-REP Roasting Vulnerable Users

```powershell
Get-ADUser -Filter {DoesNotRequirePreAuth -eq $true} -Properties DoesNotRequirePreAuth
```

### List Accounts with SPNs (Kerberoastable)

```powershell
Get-ADUser -Filter {ServicePrincipalName -eq $null} -Properties ServicePrincipalName
```

## ✅ Red Team Strategy Tip

- 🔎 **Start with AS-REP Roasting** if you're outside the domain or doing recon.
- 🛠️ **Use Kerberoasting** once you're inside to target valuable service accounts.

