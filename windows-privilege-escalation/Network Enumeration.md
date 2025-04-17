### IP Configuration

the ```ipconfig``` command is used to display and manage network configurations of a Windows machine

```powershell
ipconifg
```

*   List all network interfaces

```powershell
ipconfig /all
```

*  release IPv4 address for the specific adapter.

```powershell
ipconfig /release
```

*  release IPv6 for the specific adapter

```powershell
ipconfig /release6
```

*  Renew IPv4 address for the specific adapter

```powershell
ipconfig /renew
```

*  Renew IPv6 address for the specific adapter

```powershell
ipconfig /renew6
```

* Display the contents of DNS resolver Cache

```powershell
ipconfig /displaydns
```

*  Purges the DNS resolver cache

```powershell
ipconfig /flushdns
```

* Refreshes the DHCP leases and registers DNS names

```powershell
ipconfig /registerdns
```

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

### Traceroute

the ``` tracert``` command is used to trace the path or route to a specific remote host.

* Trace the route to a specific IP

```powershell
tracert 192.168.29.18
```

### Pathping

The ```pathping``` command is used to combine the features of ping and tracert, providing information on network latency and packet loss

```powershell
pathping 8.8.8.8
```

### ARP (Address Resolution Protocol)

the ```arp``` command is used to view and manage the ARP cache

* Display the arp cache

```powershell
arp -a
```

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

### Route Table

The ```route``` command is used to manipulate the IP routing table

* List the current routing table

```powershell
route print
```

### Netstat

The ```netstat``` command is used to display network statistics, connections and routing tables

*   Display active connections numerically
  
```powershell
netstat -n
```

*  Display all active connections and listening ports numerically
  
```powershell
netstat -an
```

*   Display active connections with process ID
  
```powershell
netstat -ano
```

*  Show TCP connections with process information
  
```powershell
netstat -anop tcp
```

*  Show UPD connections with process information
  
```powershell
netstat -anop udp
```

*   Display network connections with the associated executables

```powershell
netstat -nob
```

*   show all active connections, listening ports and associated executables.
  

```powershell
netstat -anob
```
