### working with ``smbpasswd`` and SMB Client Utilities

#### Scans SMB ports with Nmap

```bash
nmap -v -sT -sV -sC -A -p 139,135,455 <IP>
```

---

#### Enumerate SMB Shares with ``smbclient``

```bash
smbclient -L //<IP> -U u1
```

```bash
smbclient -L //<IP> -U 'devnull.infosecwarrior'
```

---

#### Change SMB Password using Remote Server

```bash
smbpasswd -r <IP> -U '<username>'
```

---

#### Enable Debugging Output

```bash
smbpasswd -D 3 -r <IP> -U '<Username>'
```

---

#### Execute Commands Remotely with ``impacket-psexec``

```bash
impacket-psexec <domain>/<username>:<password>@<IP>
```