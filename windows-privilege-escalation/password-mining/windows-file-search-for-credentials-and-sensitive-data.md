# Windows File Search For Credentials and Sensitive Data

These Commands help search for sensitive information such as passwords and credentials within files on a windows system. the goal is to locate hardcoded passwords, API keys, and other sensitive data that might be stored in configuration files or plaintext files.

#### 1. Search for Passwords in Files.

Use `findstr` to search for the keyword `password` in common configuration files.

* Search within .xml, .ini, .txt files:
* Always use in specific location

```cmd
findstr /SI /M "password" *.xml *.ini *.txt *.config
```

**/SI** - Searches for the string in subdirectories.**/M** - Displays only the file names that matches the search pattern.

**Search for `password` in all files (case-insiensitive)**

```cmd
findstr /spin "password" *.*
```

**/S** - Searches in subdirectorie&#x73;**/P** - skips files with non-printable characters.**I** - Case insensitive search**N** - Displays line numbers in the output

\


#### 2. Search for specific File Names

Use `dir` to find files with specific names or patterns related to credentials.

**Search for files containing `pass`, `cred`, `vnc` or `.config`**

```cmd
dir /S /B *pass*.txt *pass*.xml *pass*.ini *cred* *vnc* *.config*
```

**/S** - Searches in subdirectorie&#x73;**/B** - output file path without additional details.

\


#### 3. Locate Speicific diles using `where`

```cmd
where /R C:\user.txt
```

```cmd
where /R C:\*.ini
```

\


#### 4. Powershell command for advance search

```powershell
Get-ChildIten -Recurse -Include *.xml, *.ini, *.txt | Select-String -Pattern "password'
```
