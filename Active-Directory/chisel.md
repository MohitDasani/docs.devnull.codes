[Chisel github repo]()

### Download Links

* [Linux AMD64]()
* [Linux i386]()
* [Windows AMD 64]()
* [Windows i386]()

---

### Installation

Install chisel using ``apt``

```bash
apt install chisel
```

Verify the installation:

```bash
chisel -v
```

---

### Setup

create a directory for Chisel:

```bash
mkdir /opt/chisel
```

copy the downloaded files to a directory:

```bash
cp -v chisel_1.9.1_* /opt/chisel/
```

Navigate to the Directory

```bash
cd /opt/chisel
```

Unzip the Files

```bash
gunzip chisel_1.9.1_*
```

---

### Server Setup

Display help information

```bash
chisel -h
```

View the manual

```bash
man chisel
```

check server options:

```bash
chisel server -h
```

start Chisel server on port 8000 in reverse mode:

```bash
chisel server -p 8000 --reverse
```

Alternatively

```bash
chisel server --port 8000 --reverse
```

---

### Client Setup

Download the client binary

```bash
wget http://192.168.29.218/chisel
```

Extract the binary

```bash
gunzip chisel_1.7.6_linux_amd64.gz
```

remane the binary

```bash
mv chisel_1.7.6_linux_amd64 chisel
```

make it executable 

```bash
chmod +x chisel
```

Use python http sevrer to transfer 

```bash
python2 -m SimpleHTTPServer 80
```

---

### Chisel Port Forwarding

Chisel supports three types of port forwarding

#### local Port Forwarding

Local port forwarding allows you to forward a port on the client machine to a port on the server machine or a remote destination

Example :

Forward local port ``1337`` to ``127.0.0.1:1337`` through a remote chisel server:

```bash
chisel client 10.10.14.6:8000 R:1337:127.0.0.1:1337
```

In this case:

*   ``10.10.14.6`` - Chisel server IP
*   ``8000`` - Chisel server port
*   ``R:1337:127.0.0.1:1337`` - Forwarking local port ``1337`` to remote `` 127.0.0.1:1337
  

#### Remote Port Forwarding

Remote port forwarding allows you to expose a service running on the client machineto the Chisel server, whichmakes it accessible to other clients connected to the server:

Example:

Forward Remote port ``8080`` ro local ``127.0.0.1:8080``:

```bash
chisel client 192.168.29.218:80 R:8080127.0.0.1:8080
```

In this case:

*   ``192.168.29.218`` : Client server IP
*   ``80`` : chisel server port
*   ``R:8080:127.0.0.1:8080`` : Forward remote port ``8080`` to local ``127.0.0.1``


Forward remote port ``22`` to local ``22``:

```bash
chisel client 192.168.29.218:8000 R:22:127.0.0.1:22
```

*   ``192.168.29.218`` : Client server IP
*   ``8000`` : chisel server port
*   ``R:22:127.0.0.1:22`` : Forward remote port ``22`` to local ``22``


Forward remote port ``80`` to remote host:

```bash
chisel client 192.168.29.218:8000 R:80:10.0.0.100:80
```

*   ``192.168.29.218`` : Client server IP
*   ``8000`` : chisel server port
*   ``R:80:10.0.0.100:80`` : Forward remote port ``80`` to local ``10.0.0.100:80``
    


#### Windows Client Example

Forward remote port ``80`` to local ``80`` using a windows client

```cmd
chisel.exe client 192.168.29.218:21 R:80:127.0.0.1:80
```

*   ``192.168.29.218`` : Client server IP
*   ``80`` : chisel server port
*   ``R:80:127.0.0.1:80`` : Forward remote port ``80`` to local ``80``
*   

---

### Dynamic Port Forwarding

Dynamic port forwarding creates a SOCKS proxy, which allows the client to tunnel traffic dynamically o multiple destinations through the chisel server

Example: Create a SOCKS proxy on local port ``1080``:

```bash
chisel client 192.168.29.218:8000 R:socks
```

*   ``192.168.29.218`` : Client server IP
*   ``8000`` : chisel server port
*   ``R:socks`` : set up a dynamic SOCKS proxy on local port ``1080``