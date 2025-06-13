# Understanding the ACL Entries

### Permission Levels

* F - Full Control
* D - Delete access
* N - No access
* M - Modify
* RX - Read & execute
* R - Read
*   W - Write

    \

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

### Understanding the ACL entries

\


* **DESKTOP-SQPBP0N\DevNull:(F)** - The DevNull user has full control (F) over the file.
* **BUILTIN\IIS\_IUSRS:(RX)** - the IIS\_IUSRS group has read & execute(RX) permissions
* **NT Service\TrustedInstaller:(F)** - The TrustedInstaller Service has full control (F) over the file
* **NT AUTHORITY\SYSTEM:(F)** - The SYSTEM account has full control (F)
* **BUILTIN\Administrator:(F)** - The Administrator group has full control(F)
* **BUILTIN\Users:(RX)** - All users have read & execute (RX) permissions
* **DESKTOP-SQPBP0N\Administrator:(F)** - The administrator account has full control
* **BUILTIN\IIS\_IUSRS:(I)(RX)** - The (I) means permission is inherited from parent folder
* **NT Service\TrustedInstaller:(I)(F)** - Inherited full control (F) for TrustedInstaller
* **NT AUTHORITY\SYSTEM:(I)(F)** - Inherited full control (F) for SYSTEM
* **BUILTIN\Administrator:(I)(F)** - Inherited full control (F) to administrator
* **BUILTIN\Users:(I)(RX)** - Inherited read and execute(RX) permission for users
* **DESKTOP-SQPBP0N\Administrator:(I)(F)** - Inherited full control (F) for administrator

#### Key Observation

**"Inherited"** (I) Entries

* The file inherits permissions from its parent folder C:\inetpub\wwwroot
* Any changes to C:\inetpub\wwwroot will effect this file unless inheritance is disabled

Permissions Breakdown

* Administraor, SYSTEM, TrustedInstaller and Administrators have full control
* Users and IIS\_IUSRS have read and execute access
* The DevNull user has explicit Full Control

\


#### Inheritance Settings in ICACLS

These flags controls how permissions propogate to child objects (files/folders)

* **(OI) Object Inherit** - Applies to files within the folder
* **(CI) Container Inherit** - Applies to subfolders witin a folder
* **(IO) Inherit Only** - Does not apply to this folder, only child objects
* **(NP) No Propogate Inherit** - Inherited permissions will not be passed down further
* **(I) Inherited** - This permission is inherited from the parent folder

\


#### Detailed Permissions

* **DE** - Delete
* **RC** - Read control (view security settings)
* **WDAC** - Write DAC (change security settings)
* **WO** - write owner (change file owner)
* **S** - Synchronize (Used for processes)
* **AS** - Access system security
* **MA** - maximum allowed permissions
* **GR** - Generic Read
* **GW** - generic write
* **GE** - generic execute
* **GA** - generic all
* **RD** - read data/ List directory
* **WD** - write data / add files
* **AD** - append data / add subdirectory
* **REA** - read extended attributes
* **WEA** - Write extended attributes
* **X** - Execute/Traverse
* **DC** - delete child
* **RA** - Read Attributes
* **WA** - write attributes
