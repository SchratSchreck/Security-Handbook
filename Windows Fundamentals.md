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

**To be continued**

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
