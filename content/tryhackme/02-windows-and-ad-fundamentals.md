+++
date = '2025-06-15T22:07:03+01:00'
draft = false
title = '02 Windows and AD Fundamentals'
+++
# Windows and AD Fundamentals

1. Introduction
2. System Configuration
3. Change UAC Settings
4. Computer Management
5. System Information
6. Resource Monitor
7. Command Prompt
8. Registry Editor
9. Conclusion
 ---
## 📝 **Notes**
 
 ### 2. System Configuration

**System Configuration** - `MSConfig`
 - General
 - Boot
 - Services
 - Startup
 - Tools
 
**Task Manager** - `taskmgr`

### 3. Change UAC Settings

**User Account Control** - `UAC`

User Account Control helps prevent potentially harmful programs from making changes our computers.

### 4. Computer Management

**Computer Management** - `compmgmt`

This utility has three primary sections: **System Tools**, **Storage**, and **Services and Applications**.

### 4. Computer Management

**The following table describes the five event types used in event logging.**

| **Event type** | **Description**                                                                                                                                                                                                                                                                                          |
|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Error          | An event that indicates a significant problem such as loss of data or loss of functionality. For example, if a service fails to load during startup, an Error event is logged.                                                                                                                           |
| Warning        | An event that is not necessarily significant, but may indicate a possible future problem. For example, when disk space is low, a Warning event is logged. If an application can recover from an event without loss of functionality or data, it can generally classify the event as a Warning event.     |
| Information    | An event that describes the successful operation of an application, driver, or service. For example, when a network driver loads successfully, it may be appropriate to log an Information event. Note that it is generally inappropriate for a desktop application to log an event each time it starts. |
| Success Audit  | An event that records an audited security access attempt that is successful. For example, a user's successful attempt to log on to the system is logged as a Success Audit event.                                                                                                                        |
| Failure Audit  | An event that records an audited security access attempt that fails. For example, if a user tries to access a network drive and fails, the attempt is logged as a Failure Audit event.                                                                                                                   |

https://docs.microsoft.com/en-us/windows/win32/eventlog/event-types


**The event log contains the following standard logs as well as custom logs:**

| **Log**     | **Description**                                                                                                                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Application | Contains events logged by applications. For example, a database application might record a file error. The application developer decides which events to record.                                                                             |
| Security    | Contains events such as valid and invalid logon attempts, as well as events related to resource use such as creating, opening, or deleting files or other objects. An administrator can start auditing to record events in the security log. |
| System      | Contains events logged by system components, such as the failure of a driver or other system component to load during startup.                                                                                                               |
| _CustomLog_ | Contains events logged by applications that create a custom log. Using a custom log enables an application to control the size of the log or attach ACLs for security purposes without affecting other applications.                         |

https://docs.microsoft.com/en-us/windows/win32/eventlog/eventlog-key

**Local Users and Groups** - `lusrmgr.msc`

**Performance Monitor** - `perfmon`

**Device Manager** - `devmgmt.msc`

**Windows Management Instrumentation** - `WMI`

### 5. System Information

**System Information** `msinfo32`

The System Information tool in Windows, accessed by typing msinfo32 in the Run dialog (Win + R), gives a detailed overview of your system’s hardware and software. Here’s what it shows:

##### System Summary

- OS name and version
- System manufacturer/model
- Processor (CPU)
- BIOS version/date
- Installed RAM
- System type (e.g., x64-based PC)

##### Hardware Resources

- Conflicts/Sharing
- DMA, I/O, IRQs
- Memory details

##### Components
- Display (graphics card info)
- Storage (drives, partitions, S.M.A.R.T. status)
- Network (adapters, protocols)
- Sound, USB, etc.

##### Software Environment

- Drivers
- Running tasks
- Environment variables
- Services
- Startup programs

#### Environment Variables

> "Environment variables (Per Microsoft) store information about the operating system environment. This information includes details such as the operating system path, the number of processors used by the operating system, and the location of temporary folders.

> The environment variables (Per Microsoft) store data that is used by the operating system and other programs. For example, the WINDIR environment variable contains the location of the Windows installation directory. Programs can query the value of this variable to determine where Windows operating system files are located".

https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.1

`Control Panel > System and Security > System > Advanced system settings > Environment Variables OR Settings > System > About > system info > Advanced system settings > Environment Variables`

### 6. Resource Monitor

**Resource Monitor** - `resmon`

> "Resource Monitor (Per Microsoft) displays per-process and aggregate CPU, memory, disk, and network usage information, in addition to providing details about which processes are using individual file handles and modules. Advanced filtering allows users to isolate the data related to one or more processes (either applications or services), start, stop, pause, and resume services, and close unresponsive applications from the user interface. It also includes a process analysis feature that can help identify deadlocked processes and file locking conflicts so that the user can attempt to resolve the conflict instead of closing an application and potentially losing data."

- CPU
- Disk
- Network
- Memory

### 7. Command Prompt

**Command Prompt** `cmd`
 - `hostname`
 - `whoami`
 - `ipconfig`
 - `ipconfig /?`
 - `cls` - clear command promt
 - `netstat`
 - `net`
 - `net user`
 - `net help user`
 
### 8. Registry Editor

**Windows Registry** - `regedit`

 - `regedt32.exe`

> Windows Registry (per Microsoft) is a central hierarchical database used to store information necessary to configure the system for one or more users, applications, and hardware devices.

 - Profiles for each user
 - Applications installed on the computer and the types of documents that each can create
 - Property sheet settings for folders and application icons
 - What hardware exists on the system
 - The ports that are being used.

----
🐧 **Conclusions**  
Anyone can start learning how to use important Windows tools to manage and control system settings. I can now work with Task Manager, Command Prompt and Registry Editor. These skills will help me understand and control how a Windows computer works.