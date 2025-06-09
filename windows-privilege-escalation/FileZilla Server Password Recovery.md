



Filezilla Server stores user passwords in a configuration file (FiliZilla Server.xml) located on the System. The passwords are stored in hashed format, which can be extracted and cracked using tools like `hashcat`


### 1.  FileZilla Server Download Link

*   [FileZilla_Server-0_9_42.exe](http://files.xdigital.com.br/Programas/FileZilla-Server/)

*   Version 1.1.0

</br>

### 2. Location of FileZilla Server COnfiguration File

```cmd
C:\Program Files(x86)\FileZilla Server\FileZilla Server.xml
```


</br>

* if you open xml file and it shows empty then press ctrl+U to see its content, or using `type` or `more` command (**in windows**) and `cat` or `less` command (**in Linux**)

</br>

### 3. Extracting and cracking passwod Hash

copy the hash and salt and paste it in a file 

```bash
vim hash.txt
```

```bash
<hash>:<salt>
```

*   For hashcat use `mode -m 1710` for sha512(\$pass.$salt)

```bash
hashcat -m 1710 -a 0 hash.txt /usr/share/wordlists/rockyou.txt 
```
---

