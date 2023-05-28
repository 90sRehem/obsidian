#linux #terminal
1.  Open a terminal application i.e. shell prompt.
2.  Run any one of the following command on Linux to see open ports:  
    `$ sudo lsof -i -P -n | grep LISTEN   $ sudo netstat -tulpn | grep LISTEN   $ sudo ss -tulpn | grep LISTEN   $ sudo lsof -i:22 ## see a specific port such as 22 ##   $ sudo nmap -sTU -O IP-address-Here`
3.  For the latest version of Linux use the ss command. For example, ss -tulw

Let us see commands and its output in details.

## Option #1: lsof command

The syntax is:  
`$ sudo lsof -i -P -n   $ sudo lsof -i -P -n | grep LISTEN   $ doas lsof -i -P -n | grep LISTEN # OpenBSD #`  
Sample outputs:  

[![Fig.01: Check the listening ports and applications with lsof command](https://www.cyberciti.biz/media/new/faq/2016/11/lsof-outputs.png)](https://www.cyberciti.biz/faq/unix-linux-check-if-port-is-in-use-command/lsof-outputs/)

Fig.01: Check the listening ports and applications with lsof command

Consider the last line from above outputs:

sshd    85379     root    3u  IPv4 0xffff80000039e000      0t0  TCP 10.86.128.138:22 (LISTEN)

-   **sshd** is the name of the application.
-   **10.86.128.138** is the IP address to which sshd application bind to (LISTEN)
-   **22** is the TCP port that is being used (LISTEN)
-   **85379** is the process ID of the sshd process

### Viewing the Internet network services list

The /etc/services is a text file mapping between human-friendly textual names for internet services and their underlying assigned port numbers and protocol types. Use the [cat command](https://www.cyberciti.biz/faq/linux-unix-appleosx-bsd-cat-command-examples/ "cat Command in Linux / Unix with examples") or [more command](https://bash.cyberciti.biz/guide/More_command "More command - Linux Bash Shell Scripting Tutorial Wiki")/[less command](https://bash.cyberciti.biz/guide/Less_command "Less command - Linux Bash Shell Scripting Tutorial Wiki") to view it:  
`$ less /etc/services`  
A sample file:

tcpmux          1/tcp                           # TCP port service multiplexer
echo            7/tcp
echo            7/udp
discard         9/tcp           sink null
discard         9/udp           sink null
systat          11/tcp          users
daytime         13/tcp
daytime         13/udp
netstat         15/tcp
qotd            17/tcp          quote
chargen         19/tcp          ttytst source
chargen         19/udp          ttytst source
ftp-data        20/tcp
ftp             21/tcp
fsp             21/udp          fspd
ssh             22/tcp                          # SSH Remote Login Protocol
telnet          23/tcp
smtp            25/tcp          mail
time            37/tcp          timserver
time            37/udp          timserver
whois           43/tcp          nicname
tacacs          49/tcp                          # Login Host Protocol (TACACS)
tacacs          49/udp
domain          53/tcp                          # Domain Name Server
domain          53/udp

Each line describes one service, and is of the form:

#service-name   port/protocol   [aliases ...]
ssh             22/tcp                          # SSH Remote Login Protocol
time            37/tcp          timserver

## Option #2: netstat or ss command

You can check the listening ports and applications with netstat as follows.

### Linux netstat syntax

Run netstat command along with [grep command](https://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/ "How to use grep command in Linux/ Unix with examples") to filter out port in LISTEN state:  
`$ netstat -tulpn | grep LISTEN`  
The netstat command deprecated for some time on Linux. Therefore, you need to use the ss command as follows:  
`$ sudo ss -tulw   $ sudo ss -tulwn   $ sudo ss -tulwn | grep LISTEN   `  
![Linux check if port is in use using ss command](https://www.cyberciti.biz/media/new/faq/2016/11/Linux-check-if-port-is-in-use-using-ss-command.png)  
Where, ss command options are as follows:

-   **-t** : Show only TCP sockets on Linux
-   **-u** : Display only UDP sockets on Linux
-   **-l** : Show listening sockets. For example, TCP port 22 is opened by SSHD server.
-   **-p** : List process name that opened sockets
-   **-n** : Don’t resolve service names i.e. don’t use DNS

Related: [Linux Find Out Which Process Is Listening Upon a Port](https://www.cyberciti.biz/faq/what-process-has-open-linux-port/)

### FreeBSD/macOS (OS X) netstat syntax

The syntax is as follows:  
`$ netstat -anp tcp | grep LISTEN   $ netstat -anp udp | grep LISTEN`

### OpenBSD netstat syntax

`$ netstat -na -f inet | grep LISTEN   $ netstat -nat | grep LISTEN`

## Option #3: nmap command

The syntax is:  
`$ sudo nmap -sT -O localhost   $ sudo nmap -sU -O 192.168.2.13 ##[ list open UDP ports ]##   $ sudo nmap -sT -O 192.168.2.13 ##[ list open TCP ports ]##`  
Sample outputs:  

[![Fig.02: Determines which ports are listening for TCP connections using nmap](https://www.cyberciti.biz/media/new/faq/2016/11/nmap-outputs.png)](https://www.cyberciti.biz/media/new/faq/2016/11/nmap-outputs.png)

Fig.02: Determines which ports are listening for TCP connections using nmap

You can combine TCP/UDP scan in a single command:  
`$ sudo nmap -sTU -O 192.168.2.13`

## A note about Windows users

You can check port usage from Windows operating system using following command:  
`$ netstat -bano | more   $ netstat -bano | grep LISTENING   $ netstat -bano | findstr /R /C:"[LISTEING]"   `

## Conclusion

This page explained command to determining if a port is in use on Linux or Unix-like server. For more information see the [nmap command](https://www.cyberciti.biz/networking/nmap-command-examples-tutorials/ "Nmap Command Examples For Linux Users / Admins") and lsof command page [online here](https://github.com/lsof-org/lsof) or by typing the [man command](https://bash.cyberciti.biz/guide/Man_command "Man command - Linux Bash Shell Scripting Tutorial Wiki") as follows:  
`$ man lsof   $ man ss   $ man netstat   $ man nmap   $ man 5 services`