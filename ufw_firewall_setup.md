### How to Setup and Config UFW on VPS
#### What is Firewall ?
##### A firewall is computer hardware or software that controls inbound and outbound traffic of a machine.

#### What is UFW ?
##### UFW (Uncomplicated Firewall) is presented as a front-end of Iptables. By default, UFW denies all incoming connections and allows all outgoing connections.
#

- To Get Access via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 root@216.32.44.12
```
- Install UFW
```sh
apt install ufw
```
- Enable UFW
```sh
ufw enable
```
- Check UFW Status
```sh
To Check Normal Mode: ufw status 
To Check in more Comprehensive: ufw status verbose
To Check with Number: ufw status numbered
```
- Configure to support IPv6:
```sh
Open Config File: nano /etc/default/ufw
then Change: IPV6=yes
```
- To Restart Firewall Disable it then Enable it:
```sh
ufw disable
ufw enable
```
- To Check Open Port, It will show only those which are currently running:
```sh
netstat -tulpn
```
- To Open Port:
```sh
Syntax:- ufw allow port/protocol
Example:- ufw allow 21/tcp
```
- To Close Port:
```sh
Syntax:- ufw deny port/protocol
Example:- ufw deny 21/tcp
```
- To Open a Range of Ports:
```sh
Syntax:- ufw allow [Starting_port:Ending_port]/protocol
Example:- ufw allow 300:310/tcp
```
- To Close a Range of Ports:
```sh
Syntax:- ufw deny [Starting_port:Ending_port]/protocol
Example:- ufw deny 300:310/tcp
```
- To Allow Service:
```sh
Syntax:- ufw allow service_name
Example:- ufw allow http
```
- To Deny Service:
```sh
Syntax:- ufw deny service_name
Example:- ufw deny http
```
- To Allow Access to IP Address:
```sh
Syntax:- ufw allow from IPAddress
Example:- ufw allow from 192.168.1.4
```
- To Deny Access for IP Address:
```sh
Syntax:- ufw deny from IPAddress
Example:- ufw deny from 192.168.1.5
```
- To Allow IP to connect only specific Port:
```sh
Syntax:- ufw allow from IPAdress to any port Port
Example:- ufw allow from 192.168.1.4 to any port 45
```
- To Delete a Specific Rule:
```sh
1. Check Status with Number: ufw status numbered
2. Delete with Number 
   Syntax:- ufw delete number
   Example:- ufw delete 3
```
- To Reset to Default Setting:
```sh
ufw reset
```
- Some usefull connection which You may want to allow
```sh
To Allow SSH Connection: ufw allow ssh or ufw allow 22/tcp
To Secure Web Server: ufw allow 80/tcp or ufw allow http
To allow https: ufw allow 443/tcp or ufw allow https 
To Allow FTP Connection: ufw allow ftp or ufw allow 21/tcp and 20/ftp
To Allow Web Server Profile: ufw allow www
```
IMPORTANT NOTE 
If you are practing firewall 
Always disable firewall before exiting from remote server 
```sh
ufw disable
```
why?
Its because firewall is always open even though we exit from remote server  
If you enabled UFW (firewall) and then changed the SSH port (e.g., from 22 to another port like 2222) but didn’t allow the new port in UFW before exiting, we may now be locked out of SSH access  
that's why we must allow the new SSH port in UFW before we restart SSH or log out. then how ?  
1st solution is disabling the firewall which i mentioned above  
OR if the firewall is active and need to change the port then  
After editing config file  
```sh
nano /etc/ssh/sshd_config
```
and changing port to 2222
Allow the new port through the firewall:
```sh
 ufw allow 2222/tcp
```
then after only Restart the SSH service:
```sh
sudo systemctl restart ssh
```
Test new port in a separate terminal:
```sh
ssh -p 2222 username@your_server_ip
```
Once confirmed working, you can close the old session.

❌ What not to do:  
Don’t restart SSH before allowing the new port in UFW (firewall).  
Don’t log out before verifying the new port works.  
