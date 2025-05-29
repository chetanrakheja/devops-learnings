
### User Ids (UID)

Root user -> 0 Always
system users -> 1 - 999
local users -> 1000 - 60000*
    - 60000 can be changed 

#### Why don't we use root user for everyday tasks?
- if we use root user and provide details to evryone than if anyone runs a command it will be difficult to track which user executed the command
- we give sudo access to user to act as root permissions

----



- `sudo cat /etc/passwd` - command to list user present in the system
all the properties are seprated using `:` 
Example - 
    `root:*:0:0:System Administrator:/var/root:/bin/sh`
    `username:password_placeholder:user_id:group_id:user_homedir:defualif_shell`

group is helpful to provide permission 

---
- `sudo cat /etc/shadow` - this file contain user password details

example - 
    `root:*:20152:0:99999:7::`
    `ubuntu:!:20195:0:99999:7::`
`username:hashed_password_placeholder:....`

passowrd placeholder - 
`!` means - user dont have password or it may be disabled(it may have key but not password) 

---

`id ubuntu` - `id <userid>` - command to get user details
give user id , group id , list of groups user is part in

uid=0(root) gid=0(wheel) groups=0(wheel),1(daemon),2(kmem),3(sys),4(tty),5(operator),8(procview),9(procmod),12(everyone),20(staff),29(certusers),61(localaccounts),80(admin),701(com.apple.sharepoint.group.1),101(access_bpf),703(com.apple.sharepoint.group.3),33(_appstore),98(_lpadmin),100(_lpoperator),204(_developer),250(_analyticsusers),395(com.apple.access_ftp),398(com.apple.access_screensharing),399(com.apple.access_ssh),400(com.apple.access_remote_ae),702(com.apple.sharepoint.group.2),704(com.apple.sharepoint.group.4)


---
`sudo add chaicode` - create user named chaicode
- low level command 
- above command will not create user password, home directory, skeleton files
- not recommended command
---
`sudo user del -r chaicode` - delete user chaicode
-r is for recursive to delete all user details
---
`sudo adduser chetan` - add user named chetan
- this command will automatically create following
    - user uid/ gid(group id)
    - create user group 
    - create home direcory
    - create skeleton files for the user
    - will ask for password while creating user
    - will ask for user details 

---
`sudo cat /etc/group` - see group details, include list of users added in the group
---

`sudo addgroup developers` - create group named developers

---
`sudo delgroup dev` - delete group named dev

---
`sudo usermod -aG developers chetan`
add chetan to developers , 
- `-a` is to append into existing imp so that old data is not over rided
- G is for group
---
`sudo usermod -l chetanrakheja chetan` - rename user chetan to chetanrakheja, dont change user home directory

---
`sudo usermod -d newdir chetan` - change user home directory from chetan to newdir

---
by default user is not allowed to login via password in AWS to update change below config
```
cd /etc/ssh
cd sshd_config.d/
sudo vim 60-cloud-settings.conf
```

update password to yes 
- not recomended way tologin as it is not that secure

---
`su - username` - change login user to username

---
How to allow user to login via ssh pvt key for pokemon user

`sudo -u pokemon mkdir -p /home/pokemon/.ssh`
- sudo -u pokemon - run command as a pokemon user
- mkdor -p - command is used to make sub directories as well if not present

`sudo chmod 700 /home/pokemon` - give permission to only owner

`ssh-keygen -t rsa -b 4096 -f Destop/id_rsa.chetanrakheja` - command to generate key, to file will be generated one id_rsa.chetanrakheja and id_rsa.chetanrakheja.pub 
- .pub is public key and will be stored in server 
- update `/home/pokemon/.ssh/autorized_keys`  and add the content of .pub ssh into the file


`echo '.pub file content....' | sudo -u pokemon tee /home/pokemon/.ssh/autorized_keys`
`sudo chmod 600 /home/pokemon/.ssh/autorized_keys` - to secure the file 

---
`sudo passwd root` - create root user password

root has not password but it may not be able to login and user will get permission denied
to allow update
`sudo vim /etc/ssh/sshd_config.d/60-cloud-settings.conf `
add below config into file
```PermitRootLogin yes```

`sudo systemctl restart ssh`

----
sudo - super user do 

by  default every user dont have supo powers it needs to be explicitly added
sudo is nothing but a group 

`sudo usermod -aG sudo pokemon` - add pokemon to sudo group

---

`sudo cat /etc/sudoers`
see permissions

note - we dont diretlcy change this file we use `sudo visudo` command to change the permissions

---

in rhel , centos  , other linux systems may not have sudo but similar group like wheel



