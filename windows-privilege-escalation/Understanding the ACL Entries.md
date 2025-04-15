## Permission Levels

  * F - Full Control
  * D - Delete access
  * N - No access
  * M - Modify
  * RX  - Read & execute
  * R - Read
  * W - Write
</br>

* Grant full access to a user

```bash
icacls C:\example.txt /grant DevNull:F
```

* Grant Read Access to a Folder & Subfolders

Grants Read(R) Permissions to everyone recursively (/t)

```bash
icacls C:\MyFolder /grant Everyone:R /t
```

* Remove all permission for a User

```bash
icacls C:\example.txt /remove DevNull
```

* Reset all permissions to default

```bash
icacls C:\example.txt /reset 
```


## Understanding the ACL entries

</br>

  * __DESKTOP-SQPBP0N\DevNull:(F)__ - The DevNull user has full control (F) over the file.
  * __BUILTIN\IIS_IUSRS:(RX)__  - the IIS_IUSRS group has read & execute(RX) permissions
  * __NT Service\TrustedInstaller:(F)__ - The TrustedInstaller Service has full control (F) over the file
  * __NT AUTHORITY\SYSTEM:(F)__ - The SYSTEM account has full control (F)
  * __BUILTIN\Administrator:(F)__ - The Administrator group has full control(F)
  * __BUILTIN\Users:(RX)__  - All users have read & execute (RX) permissions
  *  __DESKTOP-SQPBP0N\Administrator:(F)__ - The administrator account has full control
  *  __BUILTIN\IIS_IUSRS:(I)(RX)__  - The (I) means permission is inherited from parent folder
  *  __NT Service\TrustedInstaller:(I)(F)__ - Inherited full control (F) for TrustedInstaller
  *  __NT AUTHORITY\SYSTEM:(I)(F)__ - Inherited full control (F) for SYSTEM
  *  __BUILTIN\Administrator:(I)(F)__ - Inherited full control (F) to administrator
  * __BUILTIN\Users:(I)(RX)__  - Inherited read and execute(RX) permission for users
  *  __DESKTOP-SQPBP0N\Administrator:(I)(F)__ - Inherited full control (F) for administrator
  

  ### Key Observation

__"Inherited"__ (I) Entries

* The file inherits permissions from its parent folder C:\inetpub\wwwroot
* Any changes to C:\inetpub\wwwroot will effect this file unless inheritance is disabled
  
Permissions Breakdown 

  * Administraor, SYSTEM, TrustedInstaller and Administrators have full control
  * Users and IIS_IUSRS have read and execute access
  * The DevNull user has explicit  Full Control


</br>

### Inheritance Settings in ICACLS

These flags controls how permissions propogate to child objects (files/folders)

* __(OI) Object Inherit__ - Applies to files within the folder
* __(CI) Container Inherit__  - Applies to subfolders witin a folder
* __(IO) Inherit Only__ - Does not apply to this folder, only child objects
* __(NP) No Propogate Inherit__ - Inherited permissions will not be passed down further
* __(I) Inherited__ - This permission is inherited from the parent folder

</br>

### Detailed Permissions

  * __DE__ - Delete
  * __RC__ - Read control (view security settings)
  * __WDAC__ - Write DAC (change security settings)
  * __WO__ - write owner (change file owner)
  * __S__ - Synchronize (Used for processes)
  * __AS__ - Access system security
  * __MA__ - maximum allowed permissions
  * __GR__ - Generic Read
  * __GW__ - generic write
  * __GE__ - generic execute
  * __GA__ - generic all
  * __RD__ - read data/ List directory
  * __WD__ - write data / add files
  * __AD__ - append data / add subdirectory
  * __REA__ - read extended attributes
  * __WEA__ - Write extended attributes
  * __X__ - Execute/Traverse
  *  __DC__ - delete child
  *  __RA__ - Read Attributes
  *  __WA__ - write attributes