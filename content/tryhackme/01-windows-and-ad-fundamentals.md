+++
date = '2025-06-15T21:43:56+01:00'
draft = false
title = '01 Windows and AD Fundamentals'
+++

## Windows Fundamentals 1

1. Introduction to Windows
2. Windows Editions
3. The Desktop (GUI)
4. The File System
5. The Windows\System32 Folders
6. User Accounts, Profiles, and Permissions
7. User Account Control
8. Settings and the Control Panel
9. Task Manager
10. Conclusion

---
## 📝 **Notes**

#### 1. Introduction to Windows

Connect to virtual machine via Remote Desktop

#### 2. Windows Editions
 
 > Windows 10 will reach end of support on October 14, 2025 (This applies to the following editions: Home, Pro, Pro Education, Pro for Workstations)
 >
 > https://learn.microsoft.com/en-us/lifecycle/end-of-support/end-of-support-2025#product-retirements>

#### 3. The Desktop (GUI)

A brief description of the Windows graphical user interface also known as GUI.

#### 4. The File System

Windows uses the NTFS (New Technology File System)
 - Supports files larger than 4GB
 - Set specific permissions on folders and files
 - Folder and file compression  
 - Encryption ( Encryption File System or EFS )
 

| **Permissions**      | **Meaning for Folders**                                                                                                | **Meaning for Files**                                                                |
|----------------------|------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| Read                 | Permit viewing and listing of files and subfolderss                                                                    | Permits viewing or accessing of the file's contents                                  |
| Write                | Permits adding of files and subfolders                                                                                 | Permits writing to a file                                                            |
| Read & Execute       | Permits viewing and listing of files and subfolders as well as executing of files; inherited by files and folders only | Permits viewing or accessing of the file's contents as well as executing of the file |
| List folder contents | Permits viewing and listing of files and subfolders as well as executing of files; inherited by folders only           | N/A                                                                                  |
| Modify               | Permits reading and writing of files and subfolders; allows deletion of the folder                                     | Permits reading and writing of the file; allows deletion of the file                 |
| Full control         | Permits reading, writing, changing, and deleting of files and subfolders                                               | Permits reading, writing, changing, and deleting of the file                         |

**Alternate Data Streams ( ADS )**
  - Alternate Data Streams (ADS) are a feature of the NTFS file system in Windows that allow you to store additional metadata or hidden data within a file—without changing the file's apparent size or contents
- $DATA is the default stream type in NTFS (New Technology File System), and it's closely related to Alternate Data Streams (ADS)

>[!IMPORTANT]
>- Natively Window Explorer doesn't display ADS to the user
>- Used by attackers to hide malware or data in plain sight.

#### 5. The Windows\System32 Folders

The `C:\Windows` folder is the central directory of the Windows operating system - effectively the heartbeat of the system.

#### Content and significance:

 - It contains kernel files, system services, and all the essential files needed to run Windows.
 - Some of its sub-folders are key:
   - `System32`: contains all default system programs and .dll files.
   - `WinSxS`: stores the various versions of system components. 
   - `Logs,`Temp`,`Fonts`,`SoftwareDistribution`:for various logging, updating and configuration functions.
- Examples:
   - `notepad.exe`, `cmd.exe`, `taskmgr.exe` all run from the `C:\Windows\System32` folder.
   - Important variables such as `%windir%` are used to indicate the `C:\Windows` path.

#### 6. User Accounts, Profiles, and Permissions

User accounts can be one of two types on a typical local Windows system: Administrator & Standard User.
   - An **Administrator** has full control over the system. They can add or remove users, modify groups, install or uninstall software, and change system-wide settings.
   - A **Standard User**, on the other hand, has limited permissions. They can modify their own files and folders but cannot make system-level changes, such as installing applications or altering system settings. 

#### 7. User Account Control

**User Account Control** (**UAC**) is a Windows security feature that helps prevent unauthorized changes to the operating system. It prompts for permission or an administrator password before allowing actions that could affect system-wide settings or other users. UAC helps protect your system from malware and unintended system modifications by ensuring that only trusted changes are made.

#### 8. Settings and the Control Panel

For those who have never used a computer before. Skip to the next exercise.

#### 9. Task Manager

For those who have never used a computer before. Skip to the next exercise.

---
🐧 **Conclusion**  
Actually, these tasks were really easy.