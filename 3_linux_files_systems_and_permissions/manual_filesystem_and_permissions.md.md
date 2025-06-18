## Folder structures
`usr/bin` is the directory where most of the binaries are stored

---
`boot` - files which are used when system is started (booted)

---
`dev` - devices directories
- all the config of hardware devices conected
- we dont manually change it but used by system to access hardware

---

`etc` - system wide settings are stored in this directory
- it contains all the system wide configs 
- nginx is also saved in this directory

---
`lib` - all the C standard libraries, tools which are low level libs 

---
`lost+found` - special folder , if there is a processing going on data is stored here if by any chance system crashes , this can be used to check the last state and may be used for recovery

---
`media` - pendrive like devices get mounted here - autoatic mounting

---
`mnt` - mounting directory 
when you mound the devices we mount it here 
manual mounting is done in this directory 

---
`opt` - option folder , when any software which dont follow standards are installed it is installed in this directory

---
`proc` - processes
files for the processes are made here
in memory virtual file system
`ls -la proc`

---
`root` - root used home directory

---
`run` - temp data of process, get cleared when system gets restarted

---
`sbin` - system level binaries are stored here

---
`snap` - ubuntu specific 

---
`srv` - when we serve a website .html is needed to be stored in this diretory
- data which needed to be accessed via network are stored here  
- it is not rule but just a convention

---
`sys` - kernal level config, like network interface , low level info are stored here

`/sys/class/net` - files contains all the interfaces details for the system, low level details, network level details
---
`tmp` - temprary folder
- its get autoatically cleared

---
`usr` - user programs

---
`var` - data which get continuesly changed is stored here 
    - example - cache, website

---

## File permissions

#### Symbolic notions
`d rwx rwx rwx` - symbolic permissions
`Type ownwer group others`

Type
d - directory
- - file 
l - link

Permissions
r - read
w - write
x - execute

---
`chown user:group filename`

`chmod g+w filename` add write permisson to group

---
#### octal notations

| Permission | Digit | 
|--|--|
| Read | 4 | 
| Write | 2 | 
| Execute | 1 | 

`chmod 755 filename`

#### Defualt permissions

`sudo` user - create folder
`sudo mkdir foldername`
- **default permission** - 755 
- **owner** - root 
- **group** - root 

---
`sudo` user - create file 
`sudo touch filename`
- **default permission** - 644 
- **owner** - root 
- **group** - root 

---
sudo user create folder using another user
`sudo -u codersgyan mkdir foldername`
- **default permission** - 775 
- **owner** - codersgyan 
- **group** - codersgyan 

---

sudo user create file using another user
`sudo -u codersgyan mkdir foldername`
- **default permission** - 664 
- **owner** - codersgyan 
- **group** - codersgyan 

---
local user create a folder
- **default permission** - 775 
- **owner** - codersgyan 
- **group** - codersgyan 

---
local user create a file
- **default permission** - 664 
- **owner** - codersgyan 
- **group** - codersgyan 

---

umask define the default permissions for the users
.profile can be updated for particular user

---

|Permission Type|Suymbol |File| Folder | 
|---|---|---|---|
|Read|r|cat, head, tail|ls, du|
|write|w|nano, vi |edit, rename, delete, create file|
|execute|x|execute file|cd|
|None|-|Do Nothing|Do nothing|

---

`d rwx rws rwt`

s - set GID for groups
t - sticky bit for others

- when user create a file , by default user, group in that user itself so others will not be able to read the file

`sudo chmod 2755 diretoryname` or `sudo chmod g+s directoryname` - using this command when a user create a file in the folder that file get group of the parent folder
will not have to change user owner

t - is for sticky bit, by this only owner can delete the file
`sudo chmod 1755 diretoryname` or `sudo chmod o+w diretoryname` - not only owner can now rename or delete the file
