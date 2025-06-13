PowerShell maintains a history of the commands you execute, but how to store and retrieve this history depends on the version and execution context.

### Session History (Current session)

#### View Command History

```powershell
Get-History
```

####    Rerun a Command History

```powershell
Invoke-History <ID>
```

####    Clear the Session History

```powershell
Clear-History
```

---

### Persistent History(Across Sessions)

starting from **PowerShell 5.0**, command history is saved persistently in a text file


#### Default Location of Persistent history

**Windows:**

```powershell
$env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
```

**Linux/MacOS:**

```bash
~/.local/share/powershell/PSReadLine/ConsoleHost_history.txt
```

#### view last 50 commands from Persistent History

```powershell
Get-Content (Get-PSReadLineOption).HistorySavePath -Tail 50
```

---

### Filtering and Searching History

#### Search History for a Keyword

```powershell
Get-History | Where-Object CommandLine -Match "keyword"
```

####    Retrieve the last 10 command

```powershell
Get-History -Count 10
```

####    Interactive Reverse Search


press ``Ctrl + R`` to interactively search through command history, similar to bash

---

### Deleting History

####    Remove specific comnmands from session History

```powershell
Remove-History <ID>
```

#### Clear Persistent History

```powersell
Remove-Item (Get-PSReadLineOption).HistorySavePath
```

---

