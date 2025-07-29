## Nmap Scans

### Nmap aggressive full port scan 

```bash
nmap -Pn -p- --min-rate 10000 <Target IP> -oN nmap-port
```

### Nmap aggresive version scan 

```bash
nmap -v -sT -sU -sC -A -O -T4 -p<ports> <Target IP> -oN nmap-version
```

* If you want to scan **UDP** scanning for ports you can use ``-sU`` switch


### Alternate way (Using NetCat)

```bash
nc -zv <Target IP> 1-65535
```

* use ``u`` switch for **UDP**

