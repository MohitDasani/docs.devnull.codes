### Viewing User Privileges

to list user privileges:

```powershell
whoami \all
```

###  Viewing Users and Groups

To list all users:

```powershell
net user
```

to get details about specific users:

```powershell
net user DevNull
```


```powershell
net user u1
```


```powershell
net user %username%
```

To get details about  local group

```powershell
net localgroup
```

To get details about specific group

```powershell
net localgroup administrator
```

To view details of other groups:

```powershell
net localgroup "Remote Desktop Users"
```

```powershell
net localgroup TelnetClients
```

### Managing Users

to add new user:

```powershell
net user DevNull /add
```

```powershell
net user u1 test@123 /add
```


*   ```u1``` - username
*   ```test@123``` - password
  
To make a user an administrator

```powershell
net localgroup administrators u1 /add
```

*   ```administrators``` - group name
*   ```u1```    -   username

To Delete a user:

```powershell
net user u1 /del
```

To update user's password:

```powershell
net user u1 123456789
```

### Enabling and Disabling Accounts

To enable a user account

```powershell
net user administrator /active:yes
```

To disable account

```powershell
net user administrator /active:no
```

### Removing Users from Groups

To remove a user drom the administrators group:

```powershell
net localgroup administrators DevNull /del
```

### File Ownership and Access Control

To take ownership of an file:

```powershell
takeown /f runme.bat
```

To modify access control lists (ACLs) for files and folders:

```powershell
cacls runme.bat /G DevNull:F
```

To display permissions of a file

```powershell
icacls runme.bat
```

To grant full control of a file to user:

```powershell
icacls runme.bat /grant u1:F
```


### Domain Management

To add a new user in Domain Enviornment:

```powershell
net user u1 test@123 /add
```

To list domain groups:

```powershell
net group
```

To add user to "Domain Admin" group

```powershell
net groups "Domain Admins" u1 /add
```

To add a user to "Enterprise Admins" group in a domain:

```powershell
net groups "Enterprise Admins" u1 /ADD /DOMAIN
```

To add a user to "Domain Admins" group in a domain:

```powershell
net groups "Domain Admins" u1 /ADD /DOMAIN

```

To add a user to "Administrators" group in a domain:

```powershell
net groups "Administrators" u1 /ADD /DOMAIN
```

To list all domain controllers:

```powershell
nltest /dclist:domain.com
```  

To check if user is a part of group in a domain:

```powershell'
gpresult /R
```

### Remote Desktop Connection

To connect to a remote desktop

```powershell
rdesktop -d devnull.com -u u1 -p test@123 192.168.29.18
```

To check current remote desktop settings:

```powershell
query session
```

To logg off remote desktop session:

```powershell
logoff session_id
```

### Additional Commands

To view all user-related details:

```powershell
whoami /all
```

To check user group policies:

```powershell
gpresult /H report.html
```