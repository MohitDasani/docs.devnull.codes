# Managing Files and Folder Permissions

### Taking Ownership of a file using TAKEOWN

The TAKEOWN command in windows allos a administrator to take ownership of files and folder, especially when access is denied due to permission issues

* Displays help information fo the TAKEOWN command

```bash
takeown /?
```

* Takes Ownership of a specified file

```bash
takeown /F <filename>
```

* Takes ownership of all files recursively in a folder

```bash
takedown /F <foldername> /R /D Y
```

* Take ownership of all files in drivee

```bash
takedown /F E:\* /R /D y
```

\


### Modifying File Permissions with CACLS command

The CACLS command is used to view and modify file access control lists (ACLs) in windows. However, CACLS is depricated and replaced by ICACLS in modern windows versions

* Displays the information of CACLS commans

```bash
cacls /?
```

* Shows the current permissions of the specified file.

```bash
cacls <filename>
cacls iisstart.htm
```

* Grants specific user the specific permissions.

```bash
cacls <filename> /G <username>:<permission>
cacls iisstart.htm /G DevNull:F
takeown /F iisstart.htm
cacls iisstart.htm /G Administrator:F
```

* F - Full Control
* C - Change (Modify)
* R - Read
* W - Write

\


### Managing File and Folder Permissions in Windows with ICACLS Commands

ICACLS is the modern command-line tool in windows for managing file and folder permissions. It replaces CACLS and provides more granular control over Access COntroll Lists(ACLs)

*   Basic Syntax, viewing permissions

    ```bash
    ICACLS <File_or_Folder> [Options]
    ```
* Displays the Current ACLs

```bash
ICACLS iisstart.htm
```

* Modifying Permissions, Grant Permissions, Grant Full Control(F)

```bash
icacls iisstart.htm /grant DevNull:F
```

* Grants Full Control (F) and Modify (M) permissions

```bash
icacls iisstart.html /grant DevNull:(F,M)
```

* Grant Read (R) access to everyone

```bash
icacls iisstart.htm /grant Everyone:R
```

* Remove Permission, Removes Eceryone from the ACL lists.

```bash
icacls iisstart.htm /remove Everyone
```

* Modifying Inheritance, Disables inheritance for the file

```bash
icacls iisstart.htm /inheritance:d
```

* Enables inheritance for the files

```bash
icacls iisstart.htm /inheritance:e
```

* Set Ownership

```bash
icacls iisstart.htm /setowner Administrator
```

* Backup and Restore ACL

```bash
icacls iisstart.htm /save backup_acl.txt
```

restore backup\_acl.txt

```bash
icacls iisstart /restore backup_acl.txt
```
