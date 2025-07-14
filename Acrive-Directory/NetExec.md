## NetExec (modern CrackMapExec)

NetExec is a powerful and moduler post-exploitation tool used for lateral movement, enumeration and authentication checks in ACtive Directory enviornments. It is a modern rewrite and continuation of the popular CrackMapExec (CME)

[Repository](https://github.com/Pennyw0rth/NetExec)
[Release](https://github.com/Pennyw0rth/NetExec/releases)

---

### Installation with Pipx

NetExec is recommended to be installed using ``pipx`` to manage isolated Python enviornments

#### Step 1. Install Pipx

```bash
apt install pipx
```

#### Step 2. Install NetExec From Github using pipx

```bash
pipx install git+https://github.com/Pennyw0rth/NetExec
```

#### Step 3. Move Binaries To Global Path

This step ensures ``netexec``, ``nxc`` and ``nxcdb`` commands are avialable globally on the system

```bash
cp -v /root/.local/bin/netexec /usr/local/bin/
```

```bash
cp -v /root/.local/bin/nxc /usr/local/bin/
```

```bash
cp -v /root/.local/bin/nxcdb /usr/local/bin/
```

```bash
chmod +x /usr/local/bin/nxc
```

---

### Basic smb usage

The ``nxc smb`` module is used to authenticate against SMB services across multiple targets

#### Scan a list of IPs

```bash
nxc smb target-ips.txt
```

#### Authenticate against a single SMB host

```bash
nxc smb <IP> -u <username> -p <password>
```

#### Authenticate Using username and password files

```bash
nxc smb <IP> -u /root/ad/username.txt -p /opt/passwords.txt
```

#### Continue Scanning after first valid login

```bash
nxc smb <IP> -u /root/ad/username.txt -p /opt/passwords.txt --continue-on-success
```

#### Quick SMB Authentication test

```bash
nxc smb <IP> -u <username> -p <password> --continue-on-success
```

---

### Winrm usage

WinRM (Windows Remote Management) allows remote command execution on windows machines.

```bash
nxc smb <IP> -u <username> -p <password> --continue-on-success
```

---

### Other Protocols Supported

#### Scan SMB Targets

```bash
nxc smb target-ip.txt
```

#### Scan SSH Targets

```bash
nxc ssh target-ip.txt
```
#### Scan LDAP Targets

```bash
nxc ldap target-ip.txt
```
#### Scan FTP Targets

```bash
nxc ftp target-ip.txt
```
#### Scan RDP Targets

```bash
nxc rdp target-ip.txt
```

### Transferring files using nxc

```bash
nxc smb <IP> -u <username> -p <password> --put-file /opt/share/winpeas.exe '\\Users\\Public\\winpeas.exe'
```