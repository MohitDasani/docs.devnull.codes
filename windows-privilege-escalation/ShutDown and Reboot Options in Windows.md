In Windows, users can trigger system shutdown or reboots through the ``shutdown`` command. additionally, **Group Polioy (GPO)** settigns can be used to control which users are allowed to shutdown or reboot the system, including remotely

---

### ShutDown and Reboot Commands

1.  **Reboot the system**

```cmd
shutdown /r /t 0 /f
```

*   ``/r`` : Reboot the System
*   ``/t 0`` : Sets the timeout to 0 seconds (immediate shutdown)
*   ``/f`` : Forces running application to close without warning

2.  **Shutdown the system**

```cmd
shutdown /s /t 0 /f
```

---

### Aloow/Prevent Shutdown and Reboot via GPO(Group Policy):

Group Policy allows administrators to control shutdoen and reboot permissins for local users or remote users. This can be configured using gpedir.msc on non-active directory(non-AD) system

#### Steps to configure shutdown permissions Using GPO:

1. **Open Group policy editor**

    *   Press ``Win + R`` and type ``gpedit.msc`` to open **Group Policy Editor**

2.  **Navigate to Shutdown settings.**
    
    *   Goto:

    ```cmd
    Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Local Policies -> User Rights Assignment
    ```

3.  **Modify Shutdown Privileges**

    *    **Shut down th system** : This policy controls which users can shut down the system locally

            *   Default Users/Groups with permissions:

                 *   **Administrator**
                 *  **Backup operators**

            *   Add any other user to allow them to shutdown the system

    *   **Force shutdown for a remote system**: This policy controls which users can initiate's a shutdown or reboot from a remote machine without administrative permissions

        *   Default Users/Groups with permissions:

             *   **Administrator**     


---

### Allow Remote Shutdown/Restart without admin permissions

1. **Open Group policy editor**

    *   Press ``Win + R`` and type ``gpedit.msc`` to open **Group Policy Editor**

2.  **Navigate to Shutdown settings.**
    
    *   Goto:

    ```cmd
    Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Local Policies -> User Rights Assignment
    ```

3.  **Modify Shutdown Privileges**

*   **Force shutdown for a remote system**: Allow users to initiate a shutdown or reboot from another machine without requiring admin privileges

*   Default Users/Groups with permissions:

             *   **Administrator**