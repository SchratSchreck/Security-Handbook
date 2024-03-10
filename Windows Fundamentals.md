#Windows

!! Windows 10 is discussed here !!
# The Graphical User Interface (GUI)

The desktop, the *graphical user interface (GUI)*, has 7 essential components:
- The Desktop
- Start Menu
- Search Box
- Task View
- Taskbar
- Toolbars
- Notification area

## The Desktop

The desktop is where you have your shortcuts to programs, folders, files , etc. By right clicking on the desktop you can open a context menu and change the size of the desktop, create new folders/ shortcuts and more. Under **Display Settings** you can change screens resolution, configure a multi screen set up and personalize the desktop by setting different wallpapers, fonts, color schemes and more.

**Seems obvious**

# The File System

The [*New Technology File System (NFTS)*](https://docs.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview) is used in modern Windows. Before NFTS the *File Allocation Table (FAT)* was used on Windows, and is still in USB devices, MicroSD cards, etc.
NFTS is a journaling file system, in case of a failure it can repair files / folders on disk by using information stored in a log file. 
It also allows:
- Support of files larger than 4GB
- Set specific permissions on folders and files
- Folder and file compression
- [*Encryption File System (EFS)*](https://docs.microsoft.com/en-us/windows/win32/fileio/file-encryption) 
You can check the properties (rigth clicking) on the drive the OS is installed on (usually C:\). 
For an overview of FAT, HPFS and NTFS click [here](https://docs.microsoft.com/en-us/troubleshoot/windows-client/backup-and-storage/fat-hpfs-and-ntfs-file-systems) 

On NTFS volumes, you can set permission or restriction to access files and folders:

|      Permission      |                                Meaning for Folders                                |                   Meaning for Files                    |
| :------------------: | :-------------------------------------------------------------------------------: | :----------------------------------------------------: |
|         Read         |                        View and list files and subfolders                         |           View and access the file's content           |
|        Write         |                        Permit adding files and subfolders                         |                    Write to a file                     |
|    Read & Execute    | View and list files and subfolders; execute files; inherited by files and folders | View and access the content of a file and executing it |
| List Folder Contents | View, listing of files and subfolders and execute files inherited by folders only |                          N/A                           |
|        Modify        |              Read, Write files and subfolders; deletion of a folder               |              Read, write and delete files              |
|     Full Control     |                Read, write, change and delete files and subfolders                |          Read, write, change and delete files          |
To view the permissions for a file or folder:
Right click the file or folder > select "Properties" > click the "Security" tab > select a use etc. in the "Group or user names" list

## Alternate Data Streams (ADS)

*Alternate Data Streams*  are a file attribute that is exclusive to Windows NTFS, it allows files to contain more than one stream of data (`$DATA`). ADS are often used to hide data of malware in them. To learn more read the [MalwareBytes article](https://www.malwarebytes.com/blog/news/2015/07/introduction-to-alternate-data-streams) about ADS.

# The Windows System32 Folders

Traditionally is the Windows OS located In the `C:\Windows` folder, although it can be on another drive and folder. (System) enviromental variables, like OS path, number of processors used by the OS, location of temporary folders, etc. , is stored. The system environmental variable for the Windows directory is `%windir%`
One of the many folders in the "Windows" folder is "*System32*", which contains critical files for the OS. Read more about System32 [here](https://www.howtogeek.com/346997/what-is-the-system32-directory-and-why-you-shouldnt-delete-it/) 

# User Accounts, Profiles and Permissions

There are two types of user accounts on local Windows OS with different permissions :
- Administrator: can add / delete users, modify groups and change settings
- Standart user: can change folders / files attributed to him and perform system-level changes (install programs)

One way to determine what user accounts exist on the system is by:
1. click on the Start Menu
2. type Other user
3. click on the shortcut to System Settings > Other users

If you are on a administrator account you can see the option to "Add someone else(...)"
When a new user is added a new profile with its own path, for example "c:\Users\Max", will be created. 

Another way for this is by:
1. right click on the Start Menu and select Run
2. type `lusrmgr.msc`

Now you can see the local groups and users and their permissions along with a description of them.
# User Account Control
*User Account Control* only lets accounts with high permissions execute certain actions. When executing an operation requiring high privileges , like installing a program, a prompt will appear asking the user to confirm that they want to permit the operation. When a standard user tries the operation a prompt will appear asking you to contact an administrator or entering a password to permit the operation.
Learn more [here](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/how-it-works) 
# Settings and the Control Panel
The Settings menu was indroduced in Windows 8 and replaced the Control Panel as the primary location for system changes.

# Task Manager
The Task Manager `taskmgr` provides information about what applications and processes are currently running and also how much CPU and RAM are bein used. 
Learn more [here](https://www.howtogeek.com/405806/windows-task-manager-the-complete-guide/). 

# System Configuration
The System Configuration utility `MSConfig` is a tool for advanced troublesooting. One way to open it is to enter `MSConfig` in the search bar.
The configuration has five tabs:

| Tab      | Description                                                                                              |
| -------- | -------------------------------------------------------------------------------------------------------- |
| General  | select what devices and services load upon boot                                                          |
| Boot     | define boot options for the OS                                                                           |
| Services | lists all services configured for the system                                                             |
| Startup  | (use the Task Manager to manage startup items)                                                           |
| Tools    | lists variouse tools used to configure the system along with a description; to run a tool click "Launch" |
## Change UAC Settings
The `UserAccountControlSettings.exe` tool allows us to change or turn off the UAC, meaning that we either get more or less notifications when changes to the system are made.

## Computer Management

The Computer Management (`compmgmt`) tool has three sections;
- System Tools
- Storage
- Services and Applications
### System Tools
#### Task Scheduler
The Task Scheduler can create and manage tasks that will be done automatically. The tasks can be an application, script, etc. You can also configure a specific time when the task should run, even at log in or log out.

#### Event Viewer
The Event Viewer lists all events that occured on the computer, often used to diagnose or investigate problems.
There are five types of events:

|  Event type   |                                                                                                      Description                                                                                                      |
| :-----------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|     Error     |                                 indicates a significant problem such as loss of data or functionality; if a service fails to load during startup it will be logged as an Error event                                  |
|    Warning    |      indicates events that are not necessarily significant but migth be a future problem; if an application can recover from an event without data loss or the disk space is low it will be logged as a warning       |
|  Information  | describes the successful operation of an application or service; when a network loads successfully it may log an information event; it is inappropriate for a desktop application to log an event each time it starts |
| Success Audit |                                                     records an successfull audited security access attempt; a user logging in the system is a Success Audit event                                                     |
| Failure Audit |                                              revcords unsuccessful audited security attempts; a user tries to access a network drive and fails is a Failure Audit event                                               |
The standard logs can be seen under "Windows Logs":

|     Log     |                                                                                                  Description                                                                                                  |
| :---------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| Application |                                           Contains events logged by applications; a database migth record a file error; the developer decides what events to record                                           |
|  Security   | Contains valid and invalid logon attempts and other such events; creation, opening or deleting of files and other objects are also found here; administrator can start auditing to record security log events |
|   System    |                                                Contains events logged by system components, such as the failure of a driver or other system component to load                                                 |
| *CustomLog* |                     Contains special logs created by applications; custom logs allows an application to control the size of the log and attach ACL's without affecting other applications                     |
#### Shared Folders
Here you can see a list of shares and shared folders. You can right-click on a folder and view its properties. Under Session you can see the users who are currently connected to the shares.  
#### Performance
The Performance Monitor (`perfmon`) is used to view to performance either in real-time or from a log file. Usefull for troubleshooting performance issues.
#### Device Manager
The Device Manager allows you to view and configure (f.e. enable/disable) any hardware.
#### Storage
The Disk Manager allows you to manage storage tasks:
- Set up a new drive
- Extend a partition
- Shrink a partition
- Assign or change a drive letter
#### Services and Applications
Here you can manage services and view their properties. 
## System Information
The System Information (`msinfo32`) tool lists information about your computer. It is devided into three sections:
- **Hardware Resources**: bus paths (read more [here](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/hardware-resources))
- **Components**: information about hardware devices installed on the computer
- **Software Environment**: information about software baked into the OS and installed by the user

You can search specific information with the search bar.
## Resource Monitor
The Resource Monitor (`resmon`) displays the resources (CPU, memory, disk, network) used by individual processes and lets you manage them.

## Command Prompt
The Command Prompt (`cmd`) lets you communicate with the OS through the use of commands.
### First Commands
The `hostname` command will output the computer name:
```
C:\Users\Administrator>hostname
> THM-WINFUN2
```

`whoami` will out put the naem of the logged in user:
```
C:\Users\Administrator>
> thm-winfun2\administrator
```

Some useful commands:
- `ipconfig`:  display the network address and settings for the computer
- `command /?`: lets you see the manual page for a command (`ipconfig /?`)
- `netstat`:  displays the current TCP/IP connections
- `net`: manage network resources; to see the manual use `net (user) help`

[Here](https://ss64.com/nt/) is a list of commands you can use in the Command Prompt.
## Registry Editor
The Windows Registry is a database used to store information needed to configure the system for users, applications and hardware. The Registry Editor (`regedit`) is a way to view or edit the registry
The information contained is: 
- Profiles for each user
- Applications installed and what types of documents they can create
- Settings for folders and application icons
- What hardware is installed
- What ports are used
# Windows Updates
Windows Update (`control /name Mircosoft.WindowsUpdate`) is a service that realeses security and feature updates, usually released on the 2nd Tuesday of each month, the Patch Tuesday.  

# Windows Security
In Windows Security you can manage the tools used to protect your computer and data.

## Virus & threat protection

### Current threats
Scan Options:
- **Quick scan**: scan the folders where threats are commonly found
- **Full scan**: all files and running programs; can take more than one hour
- **Custom scan**: choose which files and folders to scan 

Threat history
- **Last scan**: The Windows Defender Antivirus automatically scans the computer for malware.
- **Quarantined threats**: Threats that are isolated and will be periodically removed.
- **Allowed Threats**: Software that are identified as threats, which you allow to run.
### Virus & threat protction settings
Manage settings
- **Real-time protection**: Locates and stops malware from installing or running.
- **Cloud-delivered protection**: Increased and faster protection and access to the latest protection data in the cloud.
- **Automatic sample submission**: Send samples files.
- **Controlled folder access**: Prohibit unauthorized changes to files, folders adn memory areas.
- **Exclusion**: Select items that the Windows Defender Antivirus will not scan.
- **Notifications**: Notifications will be send with critical information about the health and security of your device.

Virus & threat protection updates
- **Check for update**: manually check for updates

Ransomware protection
- **Controlled folder access**: nedds to be enabled for Ransomware protection; (Real-time protection musst be enabled first)

## Firewall & network protection
The Firewall controlls what enters and leaves the ports of a computer.
There are three firewall profiles:
- **Domain**: applies to a network where the host system can authenticate a domain controller.
- **Private**: User-assigned profile, used to designate private or home networks
- **Public**: Default profile; used to designate public networks (WI-FI hotspots at coffee shops, airports,...)

After clicking on a firewall profile you can choose to turn it on/off or block all incoming connections. You can also view the settings for any profile and configure the firewall with `WF.msc`. Read the best practices for fireall configuration [here](https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/configure)

## App & browser control
The App & browser control protects your device against phishing, malware websites or applications and downloading potentially malicious files. More about this [here](https://learn.microsoft.com/en-us/windows/security/operating-system-security/virus-and-threat-protection/microsoft-defender-smartscreen/) 

## Device security
See descriptions in Settings

# BitLocker
Offers additional protection against data theft or exposure from lost, stolen or inappropriately decommissioned computers.
Read more about BitLocker [here](https://learn.microsoft.com/en-us/windows/security/operating-system-security/data-protection/bitlocker/)

# Volume Shadow Copy Service
The [Volume Shadow Copy Service (VSS)](https://learn.microsoft.com/en-us/windows-server/storage/file-server/volume-shadow-copy-service) manages the actions to create a shadow copy (aka snapshot or point-in-time copy) of data. These are stored on the Systen Volume Information folder on drives that have System Protection turned on.
With VSS enabled you can:
- Create a restore point
- Perform system restore
- Configure restore settings
- Delete restore points

Malware can look for these files and delete them, making it impossible to recover from an attack without offline/off-site backups.