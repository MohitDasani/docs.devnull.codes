

##  Booting Files

Boot loader is a piece of code that runs before any operating system is running. They are used to boot other operating systems, usually each operating system has a set of bootloaders specific for it.

### Windows Booting Files

*   __bootmgr__ :   Operating System loader code in Windows Vista and later versions of windows., This file is stored in the __System Reserved Partition__ or the windows installation drive.
*   __Boot Configuration Database(BCD)__ : Builds the operating system selection menu in Windows Vista and later versions of Windows. This file is stored in ``` System Reserved Partition\boot\BCD``` or the windows installation drive.
*   __winload.exe__ : Loads Windows Vista and later version of operating system. The file is located in ``` %SystemRoot\system32\winload.exe```.
*   __winresume.exe__ : Resume the Windows Operating System if system is started from hiberated state. This file is located in ```%SystemRoot%\system32\winresume.exe```.
*   __ntldr__ : Operating System loader code in windows XP and earlier versions. This file is stored in Windows Installation drive ``` C:\ntlar```.
*   __boot.ini__ : Build the operating system selection menu in WindowsXP and earlier version. This file is stored in Windows installation drive ```c:\boot.ini```.
  
  </br>


### System Reserved Partition

Windows 7 and 8 create a special __System Reserved__ partition when installed on a clean disk. Windows does not assign a drive letters to this partitions. This partitions consumes __100 MB__ of space in Windows 7 and __350 MB__ on windows 8.

</br>

### Advance Boot Options Menu

*   __Safe Mode__ : Starts a system with minimal set of files and drivers, including only the mouse, monitor, keyboard, mass storage, base video and default system services. No network support.
*   __Safe Mode with Networking__ : Adds network support to safe mode. usefull for debugging tools on a network drive.
*   __Safe Mode with Command Prompt__ : Loads safe mode but starts windows in a command prompt instead of GUI.
*   __Enable Boot Logging__ : Records boot drivers and objects during windows boot.
*   __Enable VGA mode__ : Starts Windows XP using the default VGA driver in 640X480 resolution with 256 colors.
*   __Last known goot configuration__ : Boots windows using the last successfull configuration (```HKEY_LOCAL_MACHINE\System\Select\LastKnownGood```).
*   __Starts Windows Normally__ : Performs a standard windows boot.
*   __Reboot__ :  Restarts the system from scratch.
  
  </br>

### Bootloader Modification tools

*   __EasyBCD 2.2__ :   A tool for modyfying the bootloader in Windows Vista and Later Versions.
  
  </br>

### Accessing the EFI System Partition in Windows

#### Method 1: Using Disk Management

1.  Press ```Win+R``` type ```diskmgmt.msc```, and press __Enter__.
2.  Locate the __EFI System Partition__ (Usually labeled as __EFI, System Reserved__ or a __100MB/200MB FAT32 partitions__)
3.  Right-Click the __EFI partition__ and choose __Change Drive Letter and Paths...__
4.  Click __Add__. select a drive letter(e.g. __Z:__) and click __OK__.
5.  The EFI partition is now accessible via __File-Explorer__ as ```Z:\```.


####  Method 2: Using Command prompt

```bash 
diskpart
list disk
select disk  X        # Replace X with the disk containing the EFI partition.
list partition
select partition Y    # Replace Y with EFI partition (usually a small FAT32 partition)
assign letter=Z       # Assign a drive letter
exit
```

Now, access the EFI partition via ```Z:\```.

#### Method 3: Using Poweshell

```bash
mountvol Z: /s
```

this mounts the EFI partition as ```Z:\```

#### Method 4:  Accessing EFI Files via Command Line

Once the partition is mounted (```Z:\```), navigate using.

```bash
cd /d Z:\EFI
dir
```

you should see folders like;

* ```Boot```
* ```Microsoft```
* ```Ubuntu``` (if dual-booting with Linux)
* ```OEM``` (depending on the manufacturer)

To unmount the EFI partition, remove the assigned letter using ```diskpart``` :
```bash 
diskpart
select volume Z
remove letter=Z
exit
```

Or Use:

```bash
mountvol Z: /D
```
</br>

### Useful Commands for Boot Management

#### Windows Commands

```powershell
bcdedit /enum                               #   view Current Boot Configuration
bcdedit /set {Current} safeboot minimal     #   Enable Safe Mode
bcdedit /set {Current} safeboot network     #   Enable Safe Mode with Networking
bcdedit /delete {ID}                        #   Delete a Boot entry
bootrec /fixmbr                             #   Repair Master Boot Record (MBR)
bootrec /fixboot                            #   Fix the Boot sector
bootrec /scanos                             #   Scan for installed operating systems
bootrec /rebuildbcd                         #   Rebuilds the boot configuration data
```

#### Linux Commands (GRUB Bootloader)

```bash
sudo update-grub                #   Update GRUB boot configuration
sudo grub-install /dev/sda      #   install GRUB to primary disk
cat /boot/grub/grub.cfg         #   View GRUB configuration
 ```

 This Guide provides an overview of bootloaders, boot options and key commands for Windows and Linux Systems.

