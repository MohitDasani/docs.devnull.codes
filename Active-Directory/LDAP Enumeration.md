## LDAP Enumeration - ldapsearch

[forest HTB]()

### Nmap LDAP scripts

#### RootDSE Enumeration

```bash
nmap -v -p 389 --script=ldap-rootdse.nse -Pn <IP>
```

```bash
nmap -v -p 389,636 --script=ldap-rootdse.nse -Pn <IP>
```

#### General LDAP Search

```bash
nmap -v -p 389,636 --script=ldap-search.nse -Pn <IP>
```

```bash
nmap -v -sV -sT --script=ldap* -p 389,636 <IP>
```


---

### Basic ldapsearch Syntax

#### Help & Basic Connection

```bash
ldapsearch -h
```

```bash
ldapsearch -x -H ldap://<IP>
```

#### Scope Options

```bash
ldapsearch -x -H ldap://<IP> -s base
```

```bash
ldapsearch -x -H ldap://<IP> -s sub
```

#### Query Naming Contexts

```bash
ldapsearch -x -H ldap://<IP> -s base namingxontexts
```

```bash
ldapsearch -x -H ldap://<IP> -s base -b "DC=infosecwarrior,DC=local" namingcontexts
```

---

### Authentication Methods

#### Anonymous Bind

```bash
ldapsearch -x -H ldap://<IP> -D '' -w '' -b "DC=infosecwarrior,DC=local"
```

```bash
ldapsearch -x -H ldap://<IP> -D '' -w '' -b "DC=infosecwarrior,DC=local" > htb.local-ldapsearch.txt
```

```bash
htb.local-ldapsearch.txt | grep sAMAccountNaem
```

#### Authentication Bind

```bash
ldapsearch -x -H ldap://<IP> -D 'infosecwarrior\jdoe' -w 'Password@123' -b "DC=infosecwarrior,DC=local"
```

```bash
ldapsearch -x -H ldap://<IP> -D 'jdoe@infosecwarrior.local' -w 'Password@123' -b "DC=infosecwarrior,DC=local"
```

---

### LDAP Object Enumeration

#### Full Directory Dump (base/rootDSE)

```bash
ldapsearch -x -H ldap://<IP> -x -s base -b '' "{objectClass=*}" "*" +
```

#### Over LDAPS(Encrypted)

```bash
ldapsearch -x -H ldap://<IP>:636 -x -s base -b '' "{objectClass=*}" "*" +
```

---

### Targeted LDAP Queries

#### Users

```bash
ldapsearch -x -H ldap://<IP> -b "CN=Users,DC=infosecwarriors,DC=local"
```

#### Computers

```bash
ldapsearch -x -H ldap://<IP> -b "CN=Computers,DC=infosecwarriors,DC=local"
```

#### Specific User Info

```bash
ldapsearch -x -H ldap://<IP> -b "CN=Administrator,CN=Users,DC=infosecwarriors,DC=local"
```

#### Group Memberships

```bash
ldapsearch -x -H ldap://<IP> -b "CN=Domain Admins,CN=Users,DC=infosecwarriors,DC=local"
```

```bash
ldapsearch -x -H ldap://<IP> -b "CN=Enterprise Admins,CN=Users,DC=infosecwarriors,DC=local"
```

```bash
ldapsearch -x -H ldap://<IP> -b "CN=Administrators,CN=Builtin,CN=Users,DC=infosecwarriors,DC=local"
```

```bash
ldapsearch -x -H ldap://<IP> -b "CN=Remote Desktop Users,CN=Builtin,CN=Users,DC=infosecwarriors,DC=local"
```

---

### Post-Processing/ Extraction

#### Extract usernames

```bash
cat ldapsearch-output.txt | grep sAMAccountName | cut -d " " -f 2 >usernames.txt
```

```bash
cat ldapsearch-output.txt | grep userPrincipalName | cut -d " " -f 2 | cut -d "@" -f1 >> usernames.txt
```

#### Extract passwords from descriptions

```bash
cat ldapsearch-output.txt | grep -i desc | grep -i password: | cut -d " " -f 6 > passwords.txt
```

---

### Password Hashing & SMB Access

#### Decode base64 hash

```bash
echo -n <base64 hash> | base64 -d
```

#### Hash reference 

```bash
hashcat --example-hashes
```

---

### smbmap with NTLM Hashes

```bash
smbmap -u user -p '<hash>' -H <IP>
```

```bash
smbmap -u user -p '<hash>:NTLM_HASH' -H <IP> -R
```

```bash
smbmap -u user -p '<hash>:NTLM_HASH' -H <IP> --download "data\test.txt
```