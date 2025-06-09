
##  Windows Firewall Service Management

### Check Windows Firewall Service Status

*   Retrieves the configuration details of the Windows Firewall service

```powershell
sc qc mpssvc
```

*   Checks the current status of the Windows Firewall service

```powershell
sc query mpssvc
```

### Start and Stop Windows Firewall Service

```powershell
sc stop mpssvc
```

```powershell
sc start mpssvc
```

##  Configuring Windows Firewall using ```netsh```

### Access Windows Firewall Settings

* Opens the Windows Firewall settings in the command-line interface

```powershell
netsh advfirewall firewall
```

###  Display Help for firewall commands
  
```powershell
netsh advfirewall /?
```

```powershell
netsh advfirewall show /?
```

### Display Firewall profiles

*   Display the current firewall porfile

```powershell
netsh advfirewall show currentprofile
```

*   Shows the firewall settings for the public profile

```powershell
netsh advfirewall show publicprofile
```

*   Shows the firewall settings for the private profile

```powershell
netsh advfirewall show privateprofile
```

*   Shows the firewall settings for all profiles

```powershell
netsh advfirewall show allprofile
```


### Enable or disable Windows Firewall

*   Disables the windows Fireall

```powershell
netstat firewall set opmode disable
```

*    Enables the windows firewall

```powershell
netsh firewall set opmode enable
```

*   Turn off firewall for all profile

```powershell
netsh advfireall set allprofiles state off
```

*   Turn on firewall for all profiles

```powershell
netsh advfirewall set allprofiles state on
```

### View and Modify Firewall Rules

*   Displays help for firewall rule commands

```powershell
netsh advfirewall firewall /?
```

*   Dump the current firewall configuration

```powershell
netsh advfirewall firewall dump
```

*   Opens TCP port 23 for Telnet service

```powershell
netsh firewall add portopening tcp 23 "Telnet Service"
```

* Display the current state of the firewall

```powershell
netsh firewall show state
```

*   Add a rule to allow inbound FTO client connections

```powershell
netsh advfirewall firewall add rule name="Permit FTP Cient" dir=in action=allowenable=yes profile=any program=%SystemRoot%\System32\ftp.exe
```

*   Displays all configured firewall rules

```powershell
netsh advfirewall firewall show rule
```

```powershell
netsh advfirewall firewall show rule name=nc64
```

* Display all Dynamic inbound rules
  
```powershell
netsh advfirewall firewall show rule name=all dir=in type=dynamic
```

*   Displays details of the "Permit FTP Client" rule

```powershell
netsh advfirewall firewall show rule name="Permit FTP Client"
```


### Adding and Removing Speicific Rules

*   Allow inbound RDP traffic on port 3389

```powershell
netsh advfirewall firewall add rule name="RDP" dir=in action=allow enable=yes profile=any protocol=TCP localport=3389
```

* Deletes the "RDP" rule
  
```powershell
netsh advdirewall firewall delete rule name="RDP" dir=in
```

### Modifying Firewall Rules

*   Display help for firewall rule modifications

```powershell
netsh advfirewall firewall set /?
```

* Display help for modifying exiting firewall rules.

```powershell
netsh advfirewall firewall set rule /?
```

* Modifies the "HTTP 80" rule to allow traffic on additional ports(80,80,82,83)

```powershell
netsh advfirewall firewall set rule name="HTTP 80" new localport=80,81,82,83 action=allow
```

##  Managing Windows Firewall Using PowerShell

* List all firewall rules
  
```powershell
get-netfirewall -all
```

* lists all firewall rules from the configurable service store

```powershell
get-netfirewall -policystore configurableservice -all
```

##  Windows Defender Antivirus Management

### Check Windows Defender status

* Checks the status o fwindows defender
  
```powershell
sc query windefend
```

* Retrieves the configuration details of windows defender
  
```powershell
sc qc windefend
```