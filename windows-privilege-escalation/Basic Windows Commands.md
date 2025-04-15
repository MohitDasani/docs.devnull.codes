
##  Change Directory

```bash
 cd \
```


```bash
cd devnull
 ```



```bash
 cd ..\
 ```


 * Change the drive:
```bash
 D:
 ```

  * To see current location:
```bash
 cd
 ```

* Change Directory to SystemRoot

```bash
cd %SystemRoot%
```

</br>

 ##  System Information (systeminfo)


  &emsp; Displays detailed information about the system including OS version, hardware specs, network configuration andf uptime

```bash
systeminfo
```

</br>

##  Windows Version 

```bash 
ver
```

</br>

##  Show Current User

```bash
whoami
```


*  detailed info 

```bash
whoami /all
```

</br>

## Print a message

 ```bash
 echo Welcome to Devnull notes
 ```   

</br>
 
 ## Display Enviornment Variables
 
  List all system enviornment variables and their values

  ```bash
  set 
  ```

  </br>


  ##  Display System PATH 
  
  shows the directories where executables files are searched when running cmmmands.

  ```bash
  echo %PATH%
  echo %SystemRoot%
  echo %Username%
  echo %cd%
  ```

   </br>


  ##  Create a file ( > Redirection Operator)

  ```bash
  echo "DevNull" > welcome.txt
  ``` 

</br>

  ##  View File Contents

  ```bash
  type welcome.txt
  ```


  </br>

  ## Define a enviornment variables

  ```bash
set DEMO=DevNull
  ```

* Retrieve the value of an enviornment variable

```bash
echo %DEMO%
```

</br>

## dir command

* Display help for dir command.

```bash
dir /?
```

* Listing file and folder

```bash
dir
```

* Display Short File Names (8.3 format)
```bash
dir /x
```

* Display file and folder in Bare format
```bash
dir  /s /b
```

*  /a  =   Displays all files, including hidden and system files
*  /a:hs = Lists only hidden and system files
*  /b  =   shows filesnames only, without additional details
*  /p  =   Stops output after each screenfull
*  /s  =   List all files in all subdirectives recursively 
*  /l  =   Converts filenames to lowercase
*  /o:gn = sorts files with directories first, then alphabatically
*  /v  =   shows extra information like file attributes and last access time
*  /x  =   Displays the short (8.3) filename format

</br>


* List Only Hidden and System Files (recursive)

```bash
dir /A:SH /b /S
```

* List only hidden files (Recursive)

```bash
dir /A:H /S /b
```

</br>

##  Create a New folder

```bash
md NewFolder
```

```bash
mkdir NewFolder
```

* Create folder in specific path

```bash
md C:\Users\DevNull\Documents\Project
```

* Create multiple folder at once

```bash
mkdir Folder1 Folder2 Folder3
```

* Create Nested Directories (Subdirectories in One Command)

```bash
md Parent\Child\GrandChild
```

</br>

##  Delete Files and Folders

* Delete Folder (rd or rmdir)

```bash
rd data
rmdir data
```

* Delete a folder without confirmation(/q)

```bash
rmdir folder /q
```

* Delete a folder and its content (/s /q)

```bash
rmdir folder1 /s /q
```

  + /s = deletes the specified folder and all files and subfolders inside it.
  + /q = Quiet mode, deletes without confirmation


</br>

* Delete a file (del)

```bash
del hello.txt
```

* Delete a specific file with confirmation (/p)

```bash
del d:\temp\filename.txt /p
```


* Delete files recursively (/s /f /q)

```bash
 del /s /f /q data
```

  * /s - Deletes all matching files in subdirectories
  * /f - force delete read-only files.
  * /q - Quiet mode (no confirmation)

</br>


##  Rename a File or folder(ren or rename)

```bash
ren oldname newname
rename oldname newname
```

</br>

##  Display or set date and time

```bash
date
time
```

</br>

##  Get MAC Address of the system

```bash
getmac
```


</br>

##  Exit Command prompt

```bash
exit
```

</br>

##  Change command prmpt title

```bash
title DevNull
```

</br>

##   Displays " Press any key to continue ..." and waits for user input

```bash
pasue
```

</br>

##  change command prompt Display

```bash
prompt MyPrompt$
```

</br>

##  Call Another Batch File (call) 

runs runme.bat without stopping the current 

```bash
call c:\runme.bat
```

</br>


##  Copying files and folders in Windows

 1.  Basic Copying with Copy
   
   ```bash
    copy /a c:\data\file.txt e:\data /v /y
   ```

  * /a - specifies the source file (ASCII mode).
  * /v - verifies the copied file for accuracy.
  * /y - suppresses confirmation prompts when overwriting existing files.

  2.  Advanced copy with XCOPY
   
  it is more powerfull alternative to COPY for copying directories, files and subdirectories.
  It can copy multiple files at once

  ```bash
xcopy.exe d:\data*.* c:\test\ /a /d /p /s /v /w
  ```

  * /a - Copies only files with archieve attribute.
  * /d - Copies files modified after a specific dates
  * /p - Prompts before copying each file.
* /s - copies directories and subdirectories, except empty ones.
* /v - verifies copyied files
* /w - waits for confirmation before starting


```bash
xcopy c:\inetpub\ /s /y c:\data\
```

copies everything from c:\inetpub (including subdirectoreis) to c:\data\

```bash
xcopy C:\inetpub\*.htm C:\htmfiles\ /s /v /y
```


3.  Robust copying with ROBOCOPY (Recommeded)
  &nbsp;
    ROBOCOPY (robusty file copy) is a more powerfull tool introduced in later windows versions

*  Copies all files form c:\data to C:\data1 including subdirectories.
  &nbsp;
    ```bash
    robocopy c:\data c:\data1 /s
    ```

*  Copies a specific files 
    &nbsp;

    ```bash
    robocopy C:\data c:\data1 img.jpg /S
    robocopy <source> <destination> <file to copy>
    ```

* Copy specific file types, Copies all .js, .css and .html files from C:\data to their respective folders

  &nbsp;

  ```bash
  robocopy c:\data c:\js *.js /s    
  ```

*  Copy Multiples File types
    &nbsp;

    ```bash
    robocopy c:\data\ c:\allfiles *.jpg *.png *.html *.txt /s
    ```

* Additional options
    &nbsp;

    * /r:1 - Retries once if a file fails to copy
    * /w:1 - Waits one second between Retries
    * /ndl - Prevents directories from being listed in the output.
    * /xjd - Excludes junction points (to avoid infinite loops).

</br>

##  Changing File and Folder Attributes Using ATTRIB

The ATTRIB command in windows allows users to modify file and folder such as hidden, system, Read-only and archieve

* Basic Syntax, Displays the attributes of a file

```bash
attrib [filename/foldername]
```

* Displays help information for the attrib command.

```bash
attrib /?
```

* +H/-H - Adds/Removes the Hidden Attributes
* +S/-S - Adds/Removes the System Attributes
* +R/-R - Adds/Removes the Read-Only attribute
* +A/-A - Adds/Removes the Archieve attribute
* /S  - Applies changes to all subdirectories
* /D  - Applies changes to directories

</br>

* Modifying File Attributes, make a file Hidden, System and Read-Only

```bash
attrib +s +r +h test.txt
attrib +s +r +h shell.exe
```

* Remove System, Hidden and Read-Only attributes

```bash
attrib -s -r -h test.txt
attrib -s -r -h shell.exe
```

* Applying attributes to all files in a Directory

```bash
attrib +s +h +r c:\data\* /s
attrib +s +h +r e:\* /S /D
```

* Remove Hidden, System and Read-Only attributes from all files

```bash
attrib -s -h -r c:\data\* /s
```


* Unhiding all files on a drive

```bash
attrib -h -s -r *.* /s /d
```

* protecting a file from accidential deletion

```bash
attrib +r important.docx
```


</br>

##  Restarting a Windows System immediately without any delay

```bash
shutdown /r /t 0 /f
```
* __/s__ - shutdown the computer
* __/r__ - Restart the computer
* __/t 0__ - set the time delay to 0 seconds(immediate restart) 
* __/f__ - force close all running applications without warning

</br>

* Allow user to cancel shutdown

```bash
shutdown /a
```




