Alternate Data Stream (ADS) are a feature of the **NTFS (New Technology File System)** that allows additional data to be stored inside a file without affecting its main content. This technique can be used to hide information, including passwords, within files.

### Listing Alternate Data Streams

To check for **Alternate Data Streams(ADS)** in a directory, use the following command in the **Command Prompt(cmd)**:

```cmd
dir /R
```

This will list all files and any associated **ADS** in the directory. if a file has ADS, it will be displayed after the file name in the format

```cmd
filename.txt:streamname
```

### Additional tool for finding ADS

[nirosoft tool](https://nirosoft.net/utils/alternate_data_streams.html)

---

### Retrieving ADS in powershell

```powershell
Get-Item -Path * -Stream *
```

---

### Viewing a specific Alternate Data Stream 

```powershell
Get-Item -Path .\5.txt -Stream test1.txt
```

This retrieves the **"test1.txt"** stream from **"5.txt"**. However it only shows information about the stream, not its content.

---

### Reading Hidden Data from an ADS

```powershell
Get-Content -Path flag.txt -Stream Flag
```

---

### Recreate the file with ADS

```powershell
echo "This is a secret message" > C:\Users\Administrator\secret.txt
Set-Content -Path C:\Users\Administrator\secret.txt -Stream hidden -Value "HiddenPassword123"
```

###  Writing to an ADS

```powershell
echo "HiddenPassword123" > secret.txt:hidden
```



---

### Extracting data from ADS

```powershell
Get-Content -Path secret.txt -Stream hidden
```

---

### Saving ADS content to a file

```powershell
Get-Content -Path secret.txt -Stream hidden | Out-File extracted.txt
```

---

### Deleting an ADS

```powershell
Remove-Item -Path secret.txt -Stream hidden
```

---