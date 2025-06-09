### IP Configuration

the ```ipconfig``` command is used to display and manage network configurations of a Windows machine

```powershell
ipconifg
```

Using Powershell

```powershell
Get-NetIPConfiguration | ft InterfaceAlias,InterfaceDescription,IPv4Address
```

List DNS Servers:

```powershell
Get-DnsClientServerAddress -AddressFamily IPv4 | ft
```


|Command|Description|
|-------|-----------|
|ipconfig /all|List all network interfaces|
|ipconfig /release| release IPv4 address for the specific adapter.|
|ipconfig /release6|release IPv6 for the specific adapter|
|ipconfig /renew|Renew IPv4 address for the specific adapter|
|ipconfig /renew6|Renew IPv6 address for the specific adapter|
|ipconfig /displaydns|Display the contents of DNS resolver Cache|
|ipconfig /flushdns|Purges the DNS resolver cache|
|ipconfig /registerdns|Refreshes the DHCP leases and registers DNS names

---

### Ping

The ```ping``` comamnd is used to check communication or connectivity between computers.

*   Check communication or connectivity of computers

```powershell
ping 192.168.29.18
```

*  ping indefinitely

```powershell
ping 192.168.29.18 -t
```

* Specify the number of echo request to send

```powershell
ping -n 1 192.168.29.18
```

* Adjust the size of ping packets

```powershell
ping -n 1 -l 65500 192.168.29.18
```

---

### Traceroute

the ``` tracert``` command is used to trace the path or route to a specific remote host.

* Trace the route to a specific IP

```powershell
tracert 192.168.29.18
```

---

### Pathping

The ```pathping``` command is used to combine the features of ping and tracert, providing information on network latency and packet loss

```powershell
pathping 8.8.8.8
```

---

### ARP (Address Resolution Protocol)

the ```arp``` command is used to view and manage the ARP cache

* Display the arp cache

```powershell
arp -a
```

---

### NSLookup

the ```nslookup``` command allows users to look up DNS-related information

*   find the hostname associated with an IP address.
  
```powershell
nslookup 192.168.29.18
```

*   Find the IP address associated with domain name
  
```powershell
nslookup devnull.codes
```

---

### Route Table

The ```route``` command is used to manipulate the IP routing table

* List the current routing table

```powershell
route print
```

---

### Netstat

The ```netstat``` command is used to display network statistics, connections and routing tables


|Command|Description|
|-------|-----------|
|netstat -n|Display active connections numerically|
|netstat -an|Display all active connections and listening ports numerically
|netstat -ano|Display active connections with process ID|
|netstat -anop tcp|Show TCP connections with process information|
|netstat -anop udp|Show UPD connections with process information|
|netstat -nob|Display network connections with the associated executables|
|netstat -anob|show all active connections, listening ports and associated executables.|

```cmd
dism /online /add-capabilty /capabilityname:WMIC
```