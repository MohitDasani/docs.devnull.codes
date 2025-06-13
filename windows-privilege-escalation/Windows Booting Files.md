# Booting Files

Boot loader is a piece of code that runs before any operating system is running. They are used to boot other operating systems, usually each operating system has a set of bootloaders specific for it.

## Windows Booting Files

* **bootmgr** : Operating System loader code in Windows Vista and later versions of windows., This file is stored in the **System Reserved Partition** or the windows installation drive.
* **Boot Configuration Database(BCD)** : Builds the operating system selection menu in Windows Vista and later versions of Windows. This file is stored in `System Reserved Partition\boot\BCD` or the windows installation drive.
* **winload.exe** : Loads Windows Vista and later version of operating system. The file is located in `%SystemRoot\system32\winload.exe`.
* **winresume.exe** : Resume the Windows Operating System if system is started from hiberated state. This file is located in `%SystemRoot%\system32\winresume.exe`.
* **ntldr** : Operating System loader code in windows XP and earlier versions. This file is stored in Windows Installation drive `C:\ntlar`.
* **boot.ini** : Build the operating system selection menu in WindowsXP and earlier version. This file is stored in Windows installation drive `c:\boot.ini`.

\


## System Reserved Partition

Windows 7 and 8 create a special **System Reserved** partition when installed on a clean disk. Windows does not assign a drive letters to this partitions. This partitions consumes **100 MB** of space in Windows 7 and **350 MB** on windows 8.

\


## Advance Boot Options Menu

* **Safe Mode** : Starts a system with minimal set of files and drivers, including only the mouse, monitor, keyboard, mass storage, base video and default system services. No network support.
* **Safe Mode with Networking** : Adds network support to safe mode. usefull for debugging tools on a network drive.
* **Safe Mode with Command Prompt** : Loads safe mode but starts windows in a command prompt instead of GUI.
* **Enable Boot Logging** : Records boot drivers and objects during windows boot.
* **Enable VGA mode** : Starts Windows XP using the default VGA driver in 640X480 resolution with 256 colors.
* **Last known goot configuration** : Boots windows using the last successfull configuration (`HKEY_LOCAL_MACHINE\System\Select\LastKnownGood`).
* **Starts Windows Normally** : Performs a standard windows boot.
* **Reboot** : Restarts the system from scratch.

\


## Bootloader Modification tools

* **EasyBCD 2.2** : A tool for modyfying the bootloader in Windows Vista and Later Versions.

\


## Accessing the EFI System Partition in Windows

### Method 1: Using Disk Management

1. Press `Win+R` type `diskmgmt.msc`, and press **Enter**.
2. Locate the **EFI System Partition** (Usually labeled as **EFI, System Reserved** or a **100MB/200MB FAT32 partitions**)
3. Right-Click the **EFI partition** and choose **Change Drive Letter and Paths...**
4. Click **Add**. select a drive letter(e.g. **Z:**) and click **OK**.
5. The EFI partition is now accessible via **File-Explorer** as `Z:\`.

### Method 2: Using Command prompt

```bash
diskpart
list disk
select disk  X        # Replace X with the disk containing the EFI partition.
list partition
select partition Y    # Replace Y with EFI partition (usually a small FAT32 partition)
assign letter=Z       # Assign a drive letter
exit
```

Now, access the EFI partition via `Z:\`.

### Method 3: Using Poweshell

```bash
mountvol Z: /s
```

this mounts the EFI partition as `Z:\`

### Method 4: Accessing EFI Files via Command Line

Once the partition is mounted (`Z:\`), navigate using.

```bash
cd /d Z:\EFI
dir
```

you should see folders like;

* `Boot`
* `Microsoft`
* `Ubuntu` (if dual-booting with Linux)
* `OEM` (depending on the manufacturer)

To unmount the EFI partition, remove the assigned letter using `diskpart` :

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

\


## Useful Commands for Boot Management

### Windows Commands

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

### Linux Commands (GRUB Bootloader)

```bash
sudo update-grub                #   Update GRUB boot configuration
sudo grub-install /dev/sda      #   install GRUB to primary disk
cat /boot/grub/grub.cfg         #   View GRUB configuration
```

This Guide provides an overview of bootloaders, boot options and key commands for Windows and Linux Systems.
