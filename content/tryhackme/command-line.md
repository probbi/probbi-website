+++
date = '2025-06-15T22:14:02+01:00'
draft = false
title = 'Command Line'
language = "en"
+++

### Windows Command Line

1. Basic System Information
2. Network Troubleshooting
3. File and Disk Management
4. Task and Process Management
5. Conclusion
6. 
### Windows PowerShell

1. What Is PowerShell
2. PowerShell Basics
3. Navigating the File System and Working with Files
4. Piping, Filtering, and Sorting Data
5. System and Network Information
6. Real-Time System Analysis
7. Scripting
8. 
### Linux Shells

1. Introduction to Linux Shells
2. How To Interact With a Shell?
3. Types of Linux Shells
4. Shell Scripting and Components
5. The Locker Script

---

## 📝 **Notes**

## Windows Command Line

### Basic System Information

 - command `set`, `PATH=`
 - `C:\>set ALLUSERSPROFILE=C:\ProgramData`
 - `C:\>systeminfo`
   - `more`, `driverquery`, `driverquery | more`
   - 
### Network Troubleshooting

 - `C:\>ipconfig`
 - `C:\ipconfig /all`
 - `C:\>ping target_name`
 - `C:\>tracert example.com`
 - time-to-live (TTL)
 - `C:\>nslookup example.com`
 - `C:\>netstat`
   - `-a` displays all established connections and listening ports
   - `-b` shows the program associated with each listening port and established connection
   - `-o` reveals the process ID (PID) associated with the connection
   - `-n` uses a numerical form for addresses and port numbers
 - `sshd.exe`
 - `C:\>netstat -abon`
 - 
### File and Disk Management

 - `cd` - current drive and directory
 - `dir` - child directories
 - `dir /a` - displays hidden and system files as well
 - `dir /s` - displays files in the current directory and all subdirectories.
 - `C:\Users\ibuv4r.k>tree` - visually represent the child directories and subdirectories
 - `del` - deletes a file (del *.png)
 - `copy` - copies a file to a new location (*.txt)
 - `move` - oves a file to a new location
 - `type` - displays the contents of a text file
 - `more` - shows a file's content one page at a time
 - `rmdir` - delete a folder
 
### Task and Process Management

 - `tasklist`
 - `taskkill` /PID (pid)
 - `tasklist` /FI (filter)
 
### Conclusion

 - `chkdsk` - checks the file system and disk for errors, helping identify bad sectors or corrupt files
 - `driverquery`
 - `sfc /scannow` - scans and repairs corrupted system files
## Windows PowerShell

### What Is PowerShell

> From the official Microsoft page: “PowerShell is a cross-platform task automation solution made up of
>- a command-line shell,
>- a scripting language, and
>- a configuration management framework.

CMD vs PowerShell:
    CMD is text-based (like copy, echo).
    PowerShell returns objects (e.g., Get-ChildItem, Write-Output)
    
### PowerShell Basics

 - `Get-Content` (read file content) – similar to type in CMD and cat in Linux.
 - `Set-Location` (alias: cd) changes the current working directory, similar to cd in other shells. It’s useful when scripting or navigating file systems in PowerShell.
 - `Get-Help` Run the Get-Help core cmdlet to invoke a built-in help system.
 - `Get-Command` lists all available commands (including cmdlets, functions, aliases, etc.). It helps discover what commands are available and how to use them.
  - `cmdlet` (pronounced "command-let")

### Navigating the File System and Working with Files

 - `Get-ChildItem` (`ls`, `dir`): Lists the contents of a directory. You can use it to see files and subfolders.
 - `Set-Location` (`cd`): Changes the current working directory.
 - `New-Item`: Creates a new item, such as a file or folder.
 - `Remove-Item` (`rm`, `del`): Deletes files or folders.
 - `Get-Content` (`cat`, `type`): Displays the content of a file.

### Piping, Filtering, and Sorting Data
 
 - PowerShell uses **piping** (`|`) - `Where-Object`
 - `Get-Process | Where-Object { $_.CPU -gt 100 }`
 - `Get-ChildItem | Select-Object Name, Length`
 - 
#### comparison operators

| Operator | Meaning                  | Example     |
| -------- | ------------------------ | ----------- |
| `-eq`    | Equal to                 | `$_ -eq 10` |
| `-ne`    | Not equal to             | `$_ -ne 10` |
| `-gt`    | Greater than             | `$_ -gt 5`  |
| `-ge`    | Greater than or equal to | `$_ -ge 5`  |
| `-lt`    | Less than                | `$_ -lt 20` |
| `-le`    | Less than or equal to    | `$_ -le 20` |

### Common Commands

 - `Sort-Object` `Get-Process | Sort-Object CPU`
 - `Where-Object` `Get-Process | Where-Object { $_.CPU -gt 100 }`
 - `Select-Object` `Get-Process | Select-Object Name, CPU`

### System and Network Information

 - `Get-ComputerInfo` - retrieves a comprehensive set of system details, including OS version, architecture, BIOS, and more Useful for checking OS, hardware specs, and system configuration

 - `Get-NetIPConfiguration` - displays details about network interfaces, such as IP address, gateway, and DNS settings. Great for troubleshooting network settings or viewing adapter info.
 - 
 - `Get-LocalUser` - lists all local user accounts on the machine. Useful for auditing user accounts or managing local users.

### Real-Time System Analysis
 `Get-Process` - displays all currently running processes on the system

Useful for identifying resource usage and active programs.
`Get-Service` - lists all services, including their status (Running, Stopped, etc.)

Helpful for checking or managing Windows services.
`Get-NetTCPConnection` - shows active TCP connections, including local/remote IP addresses and ports

Essential for network diagnostics, identifying open ports, or spotting suspicious connections.

### Scripting
`Invoke-Command -ComputerName RemotePC -ScriptBlock {Get-Service}`
 
This command executes Get-Service on a remote computer named RemotePC. It returns a list of services running (or stopped) on that machine.

## Linux Shells

### Introduction to Linux Shells

 - A Linux shell is a command-line interface that allows users to interact with the operating system by entering text-based commands. Common shells like **Bash**, **Zsh**, and **Fish** enable automation, scripting, and deeper control over system operations.

### How To Interact With a Shell?

 - Most Linux distributions use Bash (Bourne Again Shell) as their default shell.
 - `pwd` - Current working directory
 - `cd` - Change Directory
 - `ls` - List directory contents
 - `cat` - Displaying file contents
 - `grep` - Searching a word in file
 
### Types of Linux Shells

    echo $SHELL
    # command in Linux displays the default login shell for the current user
    cat /etc/shells
    # available shells in Linux
    chsh -s /usr/bin/zsh
    # permanently change your default shell (zsh)
 - Bourne Again Shell (Bash) -  default Shell in most Linux distributions
 - Friendly Interactive Shell (Fish) - is known for its out-of-the-box usability, autosuggestions, syntax highlighting, and easy scripting without needing configuration.
 - Z Shell (Zsh) - is powerful and highly customizable, often used with frameworks like Oh My Zsh, and is popular for its advanced features like better tab completion, globbing, and prompt customization.
 - 
### Shell Scripting and Components

#### Key Components of a Shell Script:

 - Shell scripting is a way to automate tasks in Unix-like systems by writing a series of commands in a script file, typically using shells like Bash, Zsh, or Fish.
 - **Shebang** (`#!/bin/bash`) – Tells the system which interpreter to use.
 - - Comments (`#`) – Explain what the script or specific lines do.
 - **Variables** – Store and reuse values.
 - 
#### Defining the Interpreter 
 
```
# This script solution was part of a TryHackMe room (Linux Shells)
# I did not write it myself — it's included here for reference and learning purposes.

# Defining the Interpreter 
#!/bin/bash
echo "Please enter your name first:"
read name
if [ "$name" = "Stewart" ]; then
        echo "Welcome Stewart! Here is the secret: THM_Script"
else
        echo "Sorry! You are not authorized to access the secret."
fi
```

### The Locker Script

A user has a locker in a bank. To secure the locker, we have to have a script in place that verifies the user before opening it. When executed, the script should ask the user for their name, company name, and PIN. If the user enters the following details, they should be allowed to enter, or else they should be denied access.

 - Username: John
 - Company name: Tryhackme
 - PIN: 7385

```
# This script solution was part of a TryHackMe room (Linux Shells)
# I did not write it myself — it's included here for reference and learning purposes.

#!/bin/bash

# Prompting for user input
echo "Enter your Username:"
read username

echo "Enter your Company name:"
read companyname

echo "Enter your PIN:"
read -s pin  # -s = silent input (not shown in terminal)

# Checking credentials
if [[ "$username" == "John" && "$companyname" == "Tryhackme" && "$pin" == "7385" ]]; then
    echo "Authentication Successful. You can now access your locker, John."
else
    echo "Authentication Denied!!"
fi

```

🐧 **Conclusions**  
This summary explains how to use command-line tools in Windows (CMD and PowerShell) and Linux. In Windows CMD, you can check system info, manage files, and fix network issues. PowerShell offers more advanced features like object-based commands, filtering, and scripting. In Linux, shells like Bash let you control the system, manage files, and write simple scripts for automation.

	