## 1.Get Current Username

```cmd
echo %USERNAME%
```

```cmd
whoami
```

using powershell

```powershell
$env:username
```

---

## 2. List User Privileges

```cmd
whoami /priv
```

```cmd
whoami /all
```

---

## 3. List user groups

```cmd
whoami /groups
```

---

## 4. Copy ```whoami.exe``` from network share

Windows XP do not have whoami command, we can share it from a network drive

```cmd
copy \\192.168.29.218\Public\whoami.exe whoami.exe
```


---

## 5. List All Users

Using ```net users```

```cmd
net users
```

Usin powershell

```powershell
Get-LocalUser | ft Name,Enabled,Lastlogon
```

Directly from the ```Users``` directory

```powershell
Get-ChildItem C:\Users -Force | select Name
```

---

## 6. Logon Requirements and Policies

display logon policies like password expiration, lockout duration, and complexity requirements- useful for brute-forcing stratgies

```cmd 
net accounts
```

---

##  7. Get Details about a User

```cmd
net user Administrator
net user DevNull
```

```cmd
net user '%USERNAME%'
```

---


## 8. Get Details about a group

```cmd
net localgroup
```

Using powershell

```powershell
Get-LocalGroup | ft name
```

##  9. Get Details about a group

```cmd
net localgroup Administrator
```

Domain Admin

```cmd
net group "Domain Admins"
```

using powershell

```powershell
Get-LocalGroupName Administrators | ft Name, PrincipalSource
```

---

