
## 1. Hostname 

Display the current computer's hostname

```cmd
hostname
```

---

## 2.   Windows Version and configuration

Retrieves detailed information about the operating system version and confifuration

```cmd
systeminfo
```

you can filter specific details using ```findstr``` :

```cmd
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
```

*   /B &rarr; Match at the beginning of the line.
*   /C &rarr; Match a specific string

---

## 3. Installed Patches and Updates

list all installed patches and updates:

```cmd
wmic qfe
```

get specific update details

```cmd
wmic qfe get Caption,Descripion,HotFixID,InstalledOn
```

search for specific updates

```cmd
wmic qfe get Caption,Descripion,HotFixID,InstalledOn | findstr /C:"KB5000802"
```

---

## 4. OS Architecture

identifies the system archiecture

```cmd
wmic os get osarchitecture
```

alternate method

```cmd
echo '%PROCESSOR_ARCHITECTURE%'
```

---

## 5. Environment Variables

```cmd
set
```

using powershell:

```powershell
Get-ChildItem Env: | ft Key,Value
```

---

## 6. Disk and Drive information

Retrieves information about logical disks and physical drives.

```cmd
wmic logicdisk
```

get detailed info 

```cmd
wmic logicdisk get caption,description,providername
```

Using Powershell

```powershell
Get-PSDrive | where {$_.Provider -like "Microsoft.Powershell.Core\FileSystem"} | ft Name,Root
```

---

## 7. Detailed OS Configuration

```cmd
wmic os get /format:list
```

---

## 8. Get Exact OS Version

not found in latest OS

```cmd
type C:\Windows\System32\eula.txt
```