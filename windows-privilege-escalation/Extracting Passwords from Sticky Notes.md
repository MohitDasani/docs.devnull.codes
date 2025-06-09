### Overview

The **Sticky Notes** app in **Windows** stores its contents, including potential passwords, in an **SQLite database** file located at:

```cmd
C:\Users\<User>\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite
```

Since users often store **sensitive information** like **passwords, notes, and credentials** in **sticky Notes**, This database can be valuable target for **Forensic analysis** and **security testing**

---

### Extracting data from Sticky Notes (SQLite)

#### 1. Using SQLite Command-Line Tool

```bash
sqlite3 plum.sqlite
```

Now, list all tables

```bash
.tables
```

to view stored **Sticky Notes** content, run:

```bash
Select * from Note;
```

#### 2. Extracting Notes with python

```python
import sqlite3

db_path = r"C:\Users\%USERNAME%\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite"

conn = sqlite3.connect(db_path)
cursor = conn.cursor()

cursor.execute("SELECT * FROM Note")
notes = cursor.fetchall()

for note in notes:
    print(note)

conn.close()
```