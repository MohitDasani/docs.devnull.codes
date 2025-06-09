## Windows Registry

The windows registry is a hierarchical database that contains configuration settings and options for thw Windows OS applications, hardware and user preferences. It acts as a central repository for system setting, replacing older configuration files like ```INI``` files .



### Key features of windows registry

*   stores __system-wide__ and __user-specific__ settings.
*   used by the __Windows OS, drivers, services and applications__
*   helps __configure and control hardware and software behaviour__.
*   Enables __automatic startup settings for programs__.


### Windows Registry Structure 

The registry is organiazed into hierarchical structure, similar to folders in a file system.

1.  __Hive__ - A root-level container storing registry data(like a drive)
2.  __Key__ - A subfolder inside a hive (can contain more keys or values).
3.  __Value__ - A specific settings stored inside a key( like a file with data).


__Example :__

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion
```

Here:

*   ```HKEY_LOCAL_MACHINE``` &rarr; Hive
*   ```SOFTWARE``` &rarr; Key
*   ```Microsoft``` &rarr; Subkey
*   ```Windows``` &rarr; Subkey
*   ```CurrentVersion``` &rarr; Contains various values related to the windows version

<hr>

### Main Registry Hives

| Hive | Description | 
| ----- | ---------- |
| HKEY_CLASSES_ROOT(HKCR) | Stores the associations and COM objects.|
| HKEY_CURRENT_USER (HKCU) | Stores settings for the logged in users. |
| HKEY_LOCAL_MACHINE (HKLM) | Stores system-wide settings (hardware,drivers,software).|
| HKEY_USERS (HKU) | Stores information for all user accounts.
|  HKEY_CURRENT_CONFIG (HKCC) | Stores current system configurations.|
<hr>

The registry file located in directory:-

```cmd
%SystemRoor%\System32\Config
```

This Directory includes the following files

### Registry Files

*   __Default__ - The default registry file (imp).
*   __SAM__ - The Security ACcounts Manager registry file (imp).
*   __Security__ - The security registry file (imp).
*   __Software__ - The application software registry file (imp).
*   __System__ - The system registry file (imp).
*   __Default.sav__ - A backup copy of the default registry file.
*   __Software.sav__ - A backup copy of application software registry file (imp).
*   __System.sav__ - A backup copy of system registry file (imp).
*   __autoexec.nt and config.nt__ - Files that initialize MS-DOS enviornment (old windows)
*   __AppEvent.evt__ - The application event log file (old windows).
*   __SecEvent.evt__ - The security event file (old windows)
*   __SysEvent.evt__ - The system event log file (old windows)
*   __Userdiff__ - the file that migrates preexisting user profile from previous version of windows.


### Opening The Registry editor


```cmd
C:\Windows\regedit.exe
```

<hr>

##  Managing the Windows registry from command prompt

### Viewing Registry Command Options

```cmd
reg /?
```

### Common Registry Commands

####    Query Registry

```cmd
reg QUERY hkcu\software\microsoft\windows\currentversion\policies\exporer /v nodesktop
```

####    Add a Registry Entry

```cmd
reg ADD hkcu\software\microsoft\windows\currentversion\policies\explorer /v nodesktop /t reg_dword /f /d 1
```

__nodesktop__ means there will be no icons on desktop

####    Delete a Registry Entry

```cmd
reg DELETE hkcu\software\microsoft\windows\currentversion\policies\exploror /v nodesktop \f
```

####    Save Registry Key

save in binary form 

```cmd
reg SAVE hkcu\software\microsoft\windows\currentversion\policies\explorer hkcu_explorer.hiv
```

####    Restore Registry Key

```cmd
reg restore hkcu\software\microsoft\windows\currentversion\policies\explorer hkcu_explorer.hiv
```

####    Export Registry Key

save in human readable form

```cmd
reg EXPORT hkcu\software\microsoft\windows\currentversion\policies\explorer hkcu_explorer.reg
```


####    Import Registry Key

```cmd
reg IMPORT hkcu_explorer.reg
```


####    Copy Registry Key

```cmd
reg COPY hkcu\software\microsoft\windows\currentversion\policies\explorer hklm\software\microsoft\windows\currentversion\policies\explorer /f
```

####    Compare Registry Keys


```cmd
reg COMPARE hkcu\software\microsoft\windows\currentversion\policies\explorer hklm\software\microsoft\windows\currentversion\policies\explorer
```

<hr>

## Useful Registry Modification

### Disable USB Device


```cmd
reg ADD hklm\system\currentcontrolset\services\usbstor /v Start /t reg_dword /f /d 4
```

### Add Programs to Windows Startup


```cmd
reg ADD hkcu\software\Microsoft\windows\currentversion\run /v program_name /t reg_sz /f /d "C:\\path\\to\program.exe
```

### Query USB Devices

```cmd
reg QUERY HKEY_LOCAL_MACHINE\SYSTEM\controllerset002\Enum\USBSTOR\
```

### Query Mounted Devices

```cmd
reg QUERY HKEY_LOCAL_MACHINE\SYSTEM\mounteddevices
```

### Query Recent Application

```cmd
reg QUERY HKEY_CURRENT_USER\software\microsoft\windows\currentversion\search recentapps
```


###    Find Windows Install Time and Date

the output will be UNIX hex so convert it.

```cmd
reg QUERY "HKLM\SOFTWARE\Microsoft\windows NT\currentversion" /v InstallDate
```

###    Modify Utilman.exe to cmd.exe

```cmd
REG ADD "HKLM\SOFTWARE\Microsoft\Windows NT\currentversion\Image File Execution Option\Utilman.exe /t REG_SZ /v Debugger /d "%systemroot%\system32\cmd.exe" /g
```

### Password Mining

```cmd
reg QUERY HKLM /f passw /t REG_SZ /s
reg QUERY HKCU /f passw /t REG_SZ /s
```

### Saving Windows Passwords

```cmd
mkdir c:\pass
reg SAVE hklm\sam c:\pass\sam
reg SAVE hklm\system c:\pass\system
```

##  Windows Explorer Policy Settings (Registry Keys)


These registry keys can be found at:

- *For Current User:*  
  HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer

- *For All Users:*  
  HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer

Each key should be set as a REG_DWORD (1 to Enable, 0 to Disable).

---

## 🧷 List of Registry Restrictions in Windows 11

| Registry Key             | Effect in Windows 11                                      | Enable (1) ✅ | Disable (0) ❌ |
|--------------------------|-----------------------------------------------------------|---------------|----------------|
| NoDesktop                | Hides all desktop icons & right-click                     | ✅            | ❌             |
| NoRun                    | Removes “Run” from Start Menu                             | ✅            | ❌             |
| NoPower                  | Hides Shutdown, Restart, Sleep in Start Menu              | ✅            | ❌             |
| NoClose                  | Disables the Shut Down option in Start Menu               | ✅            | ❌             |
| NoTaskManager            | Disables Task Manager (Ctrl + Shift + Esc)                | ✅            | ❌             |
| NoControlPanel           | Blocks Control Panel & Settings app                       | ✅            | ❌             |
| NoFolderOptions          | Hides Folder Options in File Explorer                     | ✅            | ❌             |
| NoDrives                 | Hides specific drives (C:, D: etc.)                       | ✅            | ❌             |
| NoSearchCommands         | Disables Windows Search in Start & Taskbar                | ✅            | ❌             |
| NoRecentDocsMenu         | Removes Recent Documents from File Explorer               | ✅            | ❌             |
| NoTrayContextMenu        | Disables System Tray right-click menu                     | ✅            | ❌             |
| NoNetworkConnections     | Blocks Network Settings                                   | ✅            | ❌             |
| HideFastUserSwitching    | Hides Switch User option                                  | ✅            | ❌             |