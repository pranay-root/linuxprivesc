# linuxprivesc
### **Linux Permissions Notes**

---

### **1. File Ownership**
Every file or directory in Linux has three types of ownership:  
- **User (`u`)**: The owner of the file.  
- **Group (`g`)**: A group of users who share the same permissions on the file.  
- **Others (`o`)**: All other users.  

---

### **2. Types of Permissions**
Permissions determine what actions can be performed on a file or directory. They include:

| **Permission** | **Symbol** | **Meaning**                 |
|----------------|------------|-----------------------------|
| **Read**       | `r`        | View the file's contents.   |
| **Write**      | `w`        | Modify the file or contents.|
| **Execute**    | `x`        | Run the file as a program.  |

#### **Directory Permissions**
- **Read (`r`)**: List files in the directory.
- **Write (`w`)**: Create, delete, or modify files in the directory.
- **Execute (`x`)**: Access files in the directory and enter it with `cd`.

---

### **3. Viewing Permissions**
Use the `ls -l` command to view permissions:

```
$ ls -l
-rw-r--r--  1 user group 1024 Dec 1 10:00 file.txt
```

#### **Explanation**:
- `-rw-r--r--`: File type and permissions.
  - `-`: Regular file.
  - `rw-`: User has read and write.
  - `r--`: Group has read only.
  - `r--`: Others have read only.
- `user`: File owner.
- `group`: Associated group.

---

### **4. Modifying Permissions**

#### **Using `chmod` (Change Mode)**

**Symbolic Mode Syntax**:
```
chmod [who][operator][permissions] file
```
- **Who**:
  - `u`: User.
  - `g`: Group.
  - `o`: Others.
  - `a`: All (user, group, others).  
- **Operator**:
  - `+`: Add permission.
  - `-`: Remove permission.
  - `=`: Set exact permission.

**Examples**:
- Add execute permission for the user:  
  ```bash
  chmod u+x file.txt
  ```
- Remove write permission for the group:  
  ```bash
  chmod g-w file.txt
  ```
- Give read-only permission to everyone:  
  ```bash
  chmod a=r file.txt
  ```

**Numeric Mode**:
Permissions use octal values:  
- **Read (`r`)**: 4  
- **Write (`w`)**: 2  
- **Execute (`x`)**: 1  

Add these values for combinations:  
- `chmod 755 file.txt`:  
  - User: `rwx` (4+2+1 = 7).  
  - Group: `r-x` (4+0+1 = 5).  
  - Others: `r-x` (4+0+1 = 5).

---

### **5. Special Permissions**

#### **Set User ID (SUID)**  
- **Purpose**: Run the file with the permissions of its owner.  
- **Symbol**: `s` (user execute position).  
- Example: `/usr/bin/passwd`.

#### **Set Group ID (SGID)**  
- **Purpose**: Files created in a directory inherit the group ownership of the directory.  
- **Symbol**: `s` (group execute position).  

#### **Sticky Bit**  
- **Purpose**: Only file owners can delete files in a shared directory.  
- **Symbol**: `t` (others execute position).  
- Example: `/tmp`.

**Set Special Permissions**:
- SUID:  
  ```bash
  chmod u+s file
  ```
- SGID:  
  ```bash
  chmod g+s directory
  ```
- Sticky Bit:  
  ```bash
  chmod +t directory
  ```

Numeric mode:  
- SUID = `4`, SGID = `2`, Sticky Bit = `1`.  
  Example:  
  ```bash
  chmod 4755 file  # SUID + rwxr-xr-x.
  ```

---

### **6. Changing Ownership**

**Change Owner**:  
```bash
chown new_owner file
```

**Change Group**:  
```bash
chgrp new_group file
```

**Change Both**:  
```bash
chown new_owner:new_group file
```

**Recursive Change**:  
```bash
chown -R user:group directory/
```

---

### **7. Default Permissions (`umask`)**

`umask` defines the default permissions for new files and directories.  

- **Default Permissions**:
  - Files: `666` (read/write for all).
  - Directories: `777` (read/write/execute for all).

**To Check `umask`**:  
```bash
umask
```

**To Set `umask`**:  
```bash
umask 022  # Default file permissions: 644, directory permissions: 755.
```

---

### **8. Practical Examples**

1. **Grant Full Access to User Only**:  
   ```bash
   chmod 700 secret.txt
   ```

2. **Create Shared Directory for a Group**:  
   ```bash
   mkdir shared_dir
   chmod 2775 shared_dir  # SGID + rwxrwxr-x.
   chgrp developers shared_dir
   ```

3. **Secure Temporary Directory**:  
   ```bash
   mkdir /secure_tmp
   chmod 1777 /secure_tmp  # Sticky Bit + rwxrwxrwt.
   ```

---

### **Quick Reference Table**

| **Permission** | **Octal** | **Meaning**                  |
|----------------|-----------|------------------------------|
| `r`            | `4`       | Read                        |
| `w`            | `2`       | Write                       |
| `x`            | `1`       | Execute                     |
| `s`            | N/A       | SUID/SGID                   |
| `t`            | N/A       | Sticky Bit                  |

---

This is a copy-paste-ready guide to Linux permissions! Let me know if you'd like examples or exercises added.
