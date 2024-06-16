#Windows 
The Active Directory allows easy managment of devices and users within corporate environment.
# Windows Domains
A windows domain is a group of users and machines within a repository called an *Active Directory (AD)*, which allows the centralised administration of components of a Windows computer network. The *Active Directory* runs on a server called the *Domain Controller (DC)*.
The advantages of a Windows domain are:
- **Centralised identity management:** All users across the network can be configured from a *Active Directory*
- **Managing security policies:** configure security policies directly from a *Active Directory* and apply them to users and machines across the network.

**Real World Example**
In school/university networks pupils will often be provided with a username and password, with which they can login into the computers available on the campus. 
When a student logs into a computer his credentials will be send forward to the Active Directory, where the credentials will be checked.  

**Exercise**
# Active Directory
The *Active Directory Domain Service (AD DS)* acts as a catalogue, in which the informations of the objects (user, groups, machines, ...) existing in the network.

**Users**
Users, one of the most common objects, can be assigned to have privileges over *resources*, like files or printer, and are thus called *security principals* (an object that can act upon resources in the network). 
Users can represent two entities:
- **People:** persons, like employees, needing access to the network 
- **Services:** f.e IIS or MSSQL, every service needs an user to run and they have, opposed to a regular user, only the privileges they need to function

**Machines**
Every computer on the network will get a own machine object. They are also considered *security principals*, but have limited rights inside the domain itself. The accounts themselves are local administrators on the assigned computer and are only suppussed to be accessed by the computer itself, but if you have the password you can login. The passwords however consits of 120 random characters and are automatically rotated out.
You can ifentify machine accounts by their naming scheme, which is the computers name followed by a `$`. For example the machine account `DC01$` belongs to the machine named `DC01`. 

**Security Groups**
You can assign accounts to groups, enhancing the manageability by automatically giving the account the groups privileges. As groups can have privilege over resources on the network they are also considered *security principals*.
Groups can have users, machines and other groups as members.

A number of groups are created by default in a domain granting specific privileges to users:

| Group             |                                         Description                                          |
| ----------------- | :------------------------------------------------------------------------------------------: |
| Domain Admins     |      Users have administrative privileges and can administer any computer on the domain      |
| Server Operators  | Users can administer Domain Controllers;cannot change membership of of administrative groups |
| Backup Operator   |        Users can access any file and are used to perform backups of data on computers        |
| Account Operators |                   Users can create and modify other accounts in the domain                   |
| Domain User       |                      Includes all existing users accounts in the domain                      |
| Domain Computers  |         Includes all existing computers in the domain, excluding domain controllers          |
| Domain Contollers |                    Includes all existing domain controllers in the domain                    |
A full list of the default groups can be found in the [Microsoft documentation](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-groups). 

**Configuring Objects using "Active Directory Users and Computers"**
