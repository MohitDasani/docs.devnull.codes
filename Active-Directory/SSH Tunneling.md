SSH Tunneling allows seure communication between systems by forwarding ports through an encrypted SSH connection. These are three types of SSH Port forwarding

### 1. SSH Local Port Forwarding

* Redirects a local port to remote server through an SSH connection
* Used to access services on remote networks securly

**Syntax :**

```bash
ssh -L [LOCAL_PORT]:[DESTINATION_HOST]:[DESTINATION_PORT] user@ssh_server
```

**Example**

```bash
ssh -L 8080:10.10.10.23:80 user@remote_host
```

This allows access to 10.10.10.23 from localhost:8080

---

### 2. Remote Port Forwarding

* Allows a remote system to access a local service securly
* Useful when exposing local services to external clients.

**Syntax:**

```bash
ssh -R [BIND_ADDRESS][BIND_PORT][LOCAL_HOST][LOCAL_PORT] user@ssh_server
```

**Example**

```bash
ssh -R 192.168.29.35:80:192.168.29.25:80 user@remote_host
```

This lets remote_host connect to loclhost:22 via remote_host:9090

---

### 3. SSH Dynamic Port Forwarding

* Creates a SOCKS proxy for dynamic routing of traffic

* Used for anonymized browsing and bypassing firewalls.

**Syntax**

```bash
ssh -D [LOCAL_PORT] user@ssh_server
```

**Example**

```bash
ssh -D 1080 user@remote_host
```

This sets up a SOCKS proxy at localhost:1080, routing traffic via remote_host

### Using ProxyChains with SSH Dynamic Port forwarding

you cna use ProxyChains with SSH Dynamic Port Forwarding to route your traffic securely through a SSH tunnel. This step is useful for anonymizing your traffic, bypassing restrictions or pivoting into a network


#### Step 1: Establish an SSH dynamic Port Forwarding Tunnel

```bash
ssh -D 9050 -q -C -N devnull@192.168.29.25
```

* -D 9050 : Binds the SOCKS proxy to port 9050 on your local machine

* -q : Suppresses unnecessary SSH output
* -C : Enables Compression (useful for slow connections)
* -N : No command execution, just forwarding traffic.


#### Step 2 : Install ProxyChains


```bash
apt install proxychains
```

#### Step 3 : Configure ProxyChains

* Edit the ProxyChains configuration file: **/etc/proxychains.conf**

```bash
vim /etc/proxychains.conf
```

#### Step 4 : Use ProxyChains with SSH Tunnel

```bash
proxychains curl http://192.168.29.25
```

```bash
proxychains nmap -v -Pn -p- 192.168.29.25
```

