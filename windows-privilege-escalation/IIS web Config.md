##  Web COnfiguration Files and Sensitive Data Discovery

### common web configuration files

web configuration files often stores sensitive information such as database connection strings, credentials, and API keys. These files are valuable targets for privilege escalation or lateral movement.


### Typical Locations of Web Configuration Files.

common locations for `web.config` files on windows system:

```cmd
C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\Web.config
C:\inetpub\wwwroot\web.config
```

---

</br>

### Finding web conifguration files

```powershell
Get-ChildItem -Path C:\inetpub\ -Include web.config -File -Recurse -ErrorAction SilentlyContinue
```

This Command:

*   Searches within `C:\inetpub\` 
*   Filters for files named `web.config`
*   Recursively checks subdirectories
*   Supresses error messages



---

Other Potentially Sensitive Configuration Files

|File Name | Description |
|----------|-------------|
|db_cont.php|  Database connection details( often includes username and password) |
|db.php| PHP file storing database credentials|
| wp-config.php| Wordpress configuration file (Contains database credentials, salts and keys)|

Example search for files using powershell

```powershell
Get-ChildItem -Path C:\ -Include db_cont.php, db.php, wp-config -File -Recurse -ErrorAction SilentlyContinue
```

Example search using `findstr` in the Command Prompt:

```cmd
findstr /si "password" *.php *.config *.xml *.ini
```

</br>

### Direct File Discovery with `dir`

```cmd
dir /S /B *pass*.txt *pass*.xml *pass*.ini *cred* *vnc* *.config
```

