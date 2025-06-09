##  Unattended Install Files (Cleartext Passwords)

1.  C:\unattend.xml
2.  C:\Windows\Panther\Unattend.xml
3.  C:\Windows\Panther\Unattend\Unattend.xml
4.  C:\windows\system32\sysprep.inf
5.  C:\Windows\System32\sysprep\sysprep.xml

*   This files often store **local admin password** or **domain join credentials** in cleartext or base64-encoded:

### Log Files and System FIles

6.  %SYSTEMDRIVE%\pagefile.sys - Potentially contains sensitive data in memory dumps
7.  %WINDIE%\debug\NetSetup.log - may contain domain join credentials
8.  %WINDIR%\iis6.log - Can Expose web app credentials or session info.

### Registry Hives (Direct Hash Extraction)

9.  %WINDIR%\repair\sam - Local user hashes
10. %WINDIR%\repair\system - may store **LSA Secret Keys**
11. %WINDIR%\repair\software - can store **service credentials**
12. %WINDIR%\repair\security - might hold **DPAPI keys** or security info

### Event Logs and COnfig Files

13. %WINDIR%\system32\config\AppEvent.Evt
14. %WINDIR%\system32\config\SecEvent.Evt
15. %WINDIR%\system32\config\default.sav
16. %WINDIR%\system32\config\security.sav
17. %WINDIR%\system32\config\software.sav
18. %WINDIR%\system32\config\system.sav

* Event logs might capture **failed logins** or **successful authentication sttempts**


Example with Powershell

```powershell
Get-WinEvent -LogName Security | Where-Object {$_.ID -eq 4624}
```


### Remote Management Credentisld

19. %WINDIR%\system32\CCM\logs*.log - May contain **SSCM deploment credentials**
20. %USERPROFILE%\ntuser.dat -Holds **registry settings** for the user. **(most Common)**
21. %USERPROFILE%\LocalS1\ContentIES\index.dat - Stores browser history and saved credentials.

Extract NTUSER.DAT settings:

```cmd
reg load HKU\TempUser C:\Users\<username>\ntuser.dat
reg query HKU\TempUser
```

### VNC Config Files (Saved Passwords)

22. dir c:*vnc.ini /s /b
23. dir c:*ultravnc.ini /s /b

