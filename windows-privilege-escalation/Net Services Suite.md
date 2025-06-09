
### General help

```powershell
net /?
```

### Account and Computer Management

Gives password policy's

```powershell
net ACCOUNTS
```

Used in AD, add user / delete user

```powershell
net COMPUTER
```

### Server Configuration

Works in AD

```powershell
net CONFIG Server
```

### Viewing Local and Remote Shares

*   Display a list of local shares
  
```powershell
net share
```

*   List the shares on remote computer
  
```powershell
net view \\192.168.29.18 
```

*   List all shares on remote computer, including hidden shares

```powershell
net view \\192.168.29.18 /all
```

### Connecting to a remote share

*   connect using specific user credentials
  
```powershell
net use \\192.168.29.18 /user:Administrator DevNull@123
```

*   View shares on remote computer

```powershell
net view \\192.168.29.18 
```

*   Connect to a remote share with a drive letter

```powershell
net use t: \\192.168.29.18\data \user:DevNull 123
```

*   Disconnect the mapped drive

```powershell
net use t: /delete
```

### Managing Network Sessions and Files

*   View active network sessions

```powershell
net session
```

*   view open files over the network

```powershell
net file
```

### Workstation and Server Configuration

*   View server configuraion

```powershell
net config server
```

*   View workstation configuration

```powershell
net config workstation
```

### Network Time Synchronization

```powershell
net TIME
```

