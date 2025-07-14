**PowerHuntShares** is a PowerShell tool used for discovering and analyzing network shares within an **Active Directory** enviornment. It helps identify **insecure or misconfigured shared folders** that could lead to **privilege escalation** or **lateral movement** by attackers

* **Discovers all network shares** in an Active Directory enviornment.
* **Identify weak permissions** (e.g. ``Everyone`` or ``Authenticated Users`` with ``Write`` access)
* **Generate CSV and JSON repo**rts to easy analysis.
* **useful for security assesments** to find potential attack paths

---

### Setup and Execution

#### Step 1: Download PowerHuntShares

```bash
git clone https://github.com/NetSPI/PowerHuntShares.git
```

**Transfer PowerHuntShares to target machine**

```powershell
powershell (New-Object System.Net.WebClient).DownloadFile('<url>','PowerHuntShares.ps1')
```

#### Step 2: Execution Policy Bypass

```powershell
powershell -nop -ep bypass
```

#### Step 3: Unblock the script

```powershell
Unblock-File -Path .\PowerHuntShares.ps1
```

#### Step 4: Run PowerHuntShares

```powershell
.\PowerHuntShares.ps1 verbose
```

alternatively **Import it as a module** to access its functions

```powershell
Import-Module .\PowerHuntShares.ps1
```

---

### PowerHuntShares Features

| **Feature** | **Description**|
|-------------|----------------|
| Enumerate SMB Shares | Identifies and lists accessible SMB network shares|
|Permissions Analysis | Checks for overly permissiive access rights (e.g. ``Everyone`` or ``Authenticated Users``) |
| Recursive Scanning | Identifies subdirectories with weak permissions |
| Identify Sensitive Files | Detects potential sensitive files (e.g. ``.config``, ``.txt``, ``.xslx``) |
| CSV Output | Saves results for further analysis |

---

### PowerHuntShares Commands

#### Find Open SMB Shares

```powershell
Find-OpenShares -verbose
```

#### Scan a specific Domain for Exposed Shares

```powershell
Find-OpenShares -Domain 'infosecwarrior.local'
```

#### Scan a specific Target

```powershell
Find-OpenShares -ComputerName 'Target-PC'
```

#### Save Results to a CSV file

```powershell
Find-OpenShares | Export-csv -Path "Shares_report.csv" -NoTypeInformation
```

---

### Attack Scenarios & Defence Recommendations

#### Offensive User (Red Team)

* Find Shares with sensitive data

```powershell
Find-OpenShares -Verbose | Where-Object { $_.Path -match "finance|passwords|backup"}
```

* Look for weak permissions (Full Control)

```powershell
Find-OpenShares | Where-Object { $_.Permissions -match "Everyone|Authenticated Users"}
```

* Automate SMB Enumeration with CrackMapExec

```bash
crackmapexec smb <IP>/24 --shares -u user -p password
```


#### PowerHuntShares Features

##### Discover Shared Folders

```powershell
.\PowerHuntShares.ps1 -Scan
```

##### Generated Reports

```powershell
.\PowerHuntShares.ps1 -Scan -OutputDirectory c:\SharesReport
```

##### Identify Insecure Shares

```powershell
.\PowerHuntShares.ps1 -Scan -InsecureOnly
```

---

### Using PowerHuntShares for SMB Enumeration

**PowerHuntShares** to scan for **SMB Shares** on a lists of hosts **without ping them first** and saves the output to a specific directory

```powershell
Invoke-HuntSMBShares -NoPing -OutputDirectory C:\AdTools\ -HostList C:\Adtools\servers.txt
```

#### Advanced Usage 

* sacn and save only Shares with "Everyone" or "Authenticated Users" Access

```powershell
Invoke-HuntSMBShares -NoPing -OutputDirectory C:\ADTools\ -HostList C:\ADTools\servers.txt | Where-Object {$_.Permissions -match "Everyone|Authenticated Users"} | Export-Csv -Path C:\ADtools\WeakShares.csv -NoTypeInformation
```

* Scan a specific Domain

```powershell
Invoke-HuntSMBShares -Domain "infosecwarrior.local" -OutputDirectory C:\ADTools
```

---

### Defence Recommendations (For Blue Teams)

* Restrict SMB shares permissions (Remove ``EveryOne`` or ``Authenticated Users`` from sensitive shares)
* Enable SMB shares auditing (Audit ``Object Access`` via group policy)
* Disable SMBv1 (Legacy and insecure, use SMBv2 or SMBv3 instead)
* Periodically scan with PowerHuntShares to detect misconfigured shares

