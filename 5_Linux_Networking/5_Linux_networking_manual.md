# Linux Networking

(Using AWS services)
---
DNS Resolution 
- First check is done in browser, OS
- DNS Resolver is requested, Ideally it is internet Service provider
- If DNS Resolver dont have it get from Root Servers
    - root server use anycast, and will contact neariest root server ips
    - then search for DNS Resolver for there TLD (.com) 
    - TLD Server will tell about name serervers / autoratative servers
    - Autoritative Name Server will give the IP
    - Autoritative Name server get the details from your domain name settings
    - DNS Resolver is also called as DNS Recursor
    - we can setup another DNS resolvers
        - 8.8.8.8 - for google
        - 1.1.1.1 - for cloudflare

---

when laptop have ip it will connect to your VM using TCP

Request will go through AWS firewall if allowed then request will go through

after firewall there is a network interface (it can be multiple) (network card is the physical NW interface)
in VM it create virtual network interface

Now you can have OS firewall, as we have more control on it

---
### Network Interfaces


All the network interface in Ec2 is virtual
all the interface have a ip assigned to it , it is a private ip in that network

127.0.0.1 - localhost - loopback network interface - `lo`

if using docker, your local lapy will have docker related network interfaces, if using tunnel you will have tunnel related network interface 

---
commands

`ifconfig` - it is a old command may not run in few vms
`ip a` -  this command give network interface details
    - enX0 also have state up which means it is up and running, it will also show the ip address of the system 

`ethtool -i enX0` - command to check if it is physical or virtual interface

network interface can also go down , if down then we will also not able to do ssh
- easiest solution is to reboot the system 
- if not we can start a new ec2 instance attach volume of this system to another, make changes using new vm and then start the network interface

`ip link set enX0 down` - dont run, but this command will turn down the network interface

`ip link set eth0 up` to turn on a network interface

---

when server is started it contact DHCP(Dynamic host configration protocol) server and request the ip , sometime there is a error and ip is not assigned so just reboot the machine to refresh it 


---
sometimes we are not able to access open internet services , in this case we will do the following 
- check if internet conectivity is there or not 
    - do `ping 8.8.8.8`
    - `ping openai.com`
- if internet is working from ping command but still not able to access our api we will `dig openai.com`      
to check dns details for openai.com this will give ip address of openai.com from DNS
    - `dig openai.com @<DNS IP>` - this command will explicitly tell which dns server to use to resolve a particular ip 
- majority times there is a DNS issue and DNS is not getting resolved
    - in this case we will check `ip route` to check ip route details
    - it will tell if you have dhcp connection using which ip, if direct link is present for the ip range as well
`sudo cat /etc/resolv.conf` - to view config of nameservers, internal dns resolvers

- `resolvectl status`  to know current DNS server details 

---

#### when not able to access service from outside

We need to check the following 
- check if service is running 
    `sudo lsof -i :80` - to list all open file in network sockets

- check secuity groups of AWS (inbound rules)    
- check internal firewall
`uwf` - uncomplecated firewall
`sudo uwf status` - to check firewall status and allowed details

`sudo uwf enable` - to enable firewall
- by default all outgoing servcies are allwoed but all incomming services are blocked (existing session will not be blocked )

`sudo ufw allow ssh` - to allow ssh
`sudo uwf enable` - to enable firewall

`sudo ufw allow http` - to allow http (port 80 for inbound)
`sudo uwf status verbose` - to check firewall status and allowed details

`sudo ufw default deny outgoing` - to stop all outgoing traffic 

`sudo ufw default allow outgoind` - to allow all outgoing
- You can allow outgoing or incoming traffic based on IP, Ports etc

---

Hostname
Hostname is the name of the system 

`hostname` - to get hostname
`uname -n` - to get hostname

`hostname dev-lernst-01` - to change hostname

`sudo vim /etc/host` - update file to change hostname details of other servers
add `<ip address> <hostname>`
now you can login using hostname

---



































































































































































































