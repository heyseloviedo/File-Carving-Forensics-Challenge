# File Carving Forensics Challenge

## Objective

The objective of this project was to examine an unknown file, identify its actual file type, locate embedded data, extract a compressed archive, and recover a hidden flag. The investigation required following multiple layers of data from a PNG file to embedded gzip data, then to a TAR archive containing the final text file.

### Skills Learned

- Identifying unknown files using file signatures.
- Using Binwalk to locate embedded data inside a file.
- Interpreting byte offsets during file carving.
- Extracting data from a specific offset with `dd`.
- Decompressing gzip data and identifying archive formats.
- Inspecting and extracting TAR archives.
- Following multiple layers of hidden data during a forensic investigation.

### Tools Used

- Kali Linux for the forensic investigation.
- `file` to identify the actual file type.
- `binwalk` to locate embedded file signatures.
- `dd` to carve data from a specific byte offset.
- `gunzip` to decompress the embedded gzip data.
- `tar` to inspect and extract the TAR archive.
- `cat` to read the recovered text file.

## Steps

### Step 1: Identify the File Type

The investigation started by identifying the actual file type of the provided file.

```bash
file green_file
```

The output identified `green_file` as a PNG image.



---

### Step 2: Inspect the File for Embedded Data

Binwalk was used to scan the file for embedded file signatures.

```bash
binwalk green_file
```

The scan revealed several file signatures, including gzip-compressed data beginning at byte offset `3243`.



---

### Step 3: Carve the Embedded Gzip Data

The gzip section was extracted from the original file using the offset identified by Binwalk.

```bash
dd if=green_file of=hidden.gz bs=1 skip=3243
```

The extracted file was then checked to confirm its format.

```bash
file hidden.gz
```

**Screenshot to add:** Terminal showing the `dd` extraction and file identification.

*Ref 3: Carving the embedded gzip data from byte offset 3243.*

---

### Step 4: Decompress the Extracted Data

The gzip data was decompressed into a new file.

```bash
gunzip -c hidden.gz > hidden
```

The decompressed file was identified with:

```bash
file hidden
```

The result showed that `hidden` was a POSIX TAR archive.

**Screenshot to add:** Terminal showing `hidden` identified as a POSIX TAR archive.

*Ref 4: Identifying the decompressed data as a TAR archive.*

---

### Step 5: Inspect and Extract the TAR Archive

The contents of the TAR archive were listed before extraction.

```bash
tar -tf hidden
```

The archive contained:

```text
flags/
flags/flags.txt
```

The archive was then extracted.

```bash
tar -xf hidden
```

**Screenshot to add:** Terminal showing the contents of the TAR archive.

*Ref 5: Discovering the `flags/flags.txt` file inside the archive.*

---

### Step 6: Recover the Hidden Flag

The recovered text file was opened with:

```bash
cat flags/flags.txt
```

This revealed the hidden challenge flag and completed the forensic investigation.

**Screenshot to add later:** Terminal showing the recovered flag. The flag can be cropped or blurred if you want to keep the answer private.

*Ref 6: Reading the recovered flag from the extracted text file.*
