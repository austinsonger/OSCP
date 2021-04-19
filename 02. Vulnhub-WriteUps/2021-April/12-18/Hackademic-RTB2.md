
> **FYI** - The IP Address for the vulnerable machine will change, this is just due to times when I'm taking a break and I'm working on multiple machines at one time, but I'm only gathering notes for one machine at a time.



## Information Gathering and Recon

### Nikto

```shell
┌─[✗]─[osboxes@parrot]─[/]
└──╼ $nikto -h http://10.0.3.136
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.0.3.136
+ Target Hostname:    10.0.3.136
+ Target Port:        80
+ Start Time:         2021-04-18 20:43:15 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache/2.2.14 (Ubuntu)
+ Retrieved x-powered-by header: PHP/5.3.2-1ubuntu4.7
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Uncommon header 'tcn' found, with contents: list
+ Apache mod_negotiation is enabled with MultiViews, which allows attackers to easily brute force file names. See http://www.wisec.it/sectou.php?id=4698ebdc59d15. The following alternatives for 'index' were found: index.php
+ Apache/2.2.14 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ OSVDB-12184: /?=PHPB8B5F2A0-3C92-11d3-A3A9-4C7B08C10000: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings.
+ OSVDB-12184: /?=PHPE9568F36-D428-11d2-A769-00AA001ACF42: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings.
+ OSVDB-12184: /?=PHPE9568F34-D428-11d2-A769-00AA001ACF42: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings.
+ OSVDB-12184: /?=PHPE9568F35-D428-11d2-A769-00AA001ACF42: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings.
+ OSVDB-3092: /phpmyadmin/changelog.php: phpMyAdmin is for managing MySQL databases, and should be protected or limited to authorized hosts.
+ OSVDB-3268: /icons/: Directory indexing found.
+ Server may leak inodes via ETags, header found with file /icons/README, inode: 413560, size: 5108, mtime: Tue Aug 28 06:48:10 2007
+ OSVDB-3233: /icons/README: Apache default file found.
+ /phpmyadmin/: phpMyAdmin directory found
+ OSVDB-3092: /phpmyadmin/Documentation.html: phpMyAdmin is for managing MySQL databases, and should be protected or limited to authorized hosts.
+ 8877 requests: 0 error(s) and 18 item(s) reported on remote host
+ End Time:           2021-04-18 20:43:42 (GMT-4) (27 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested

```


### NMAP


```shell
nmap -sS -sU -T4 -A -v -PE -PP -PS80,443 -PA3389 -PU40125 -PY -g 53 --script "default or (discovery and safe)" 10.0.3.136
```


```shell
Starting Nmap 7.80 ( https://nmap.org ) at 2021-04-18 17:37 EDT
NSE: Loaded 292 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 17:37
NSE: [mtrace] A source IP must be provided through fromip argument.
NSE: [shodan-api] Error: Please specify your ShodanAPI key with the shodan-api.apikey argument
too short
Completed NSE at 17:37, 10.50s elapsed
Initiating NSE at 17:37
Completed NSE at 17:37, 0.00s elapsed
Initiating NSE at 17:37
Completed NSE at 17:37, 0.00s elapsed
Pre-scan script results:
| broadcast-igmp-discovery: 
|   10.0.3.1
|     Interface: eth0
|     Version: 2
|     Group: 224.0.0.251
|     Description: mDNS (rfc6762)
|   10.0.3.1
|     Interface: eth0
|     Version: 2
|     Group: 224.0.0.252
|     Description: Link-local Multicast Name Resolution (rfc4795)
|   10.0.3.1
|     Interface: eth0
|     Version: 2
|     Group: 239.255.102.18
|     Description: Organization-Local Scope (rfc2365)
|   10.0.3.1
|     Interface: eth0
|     Version: 2
|     Group: 239.255.255.250
|     Description: Organization-Local Scope (rfc2365)
|_  Use the newtargets script-arg to add the results as targets
| targets-asn: 
|_  targets-asn.asn is a mandatory parameter
Initiating ARP Ping Scan at 17:37
Scanning 10.0.3.136 [1 port]
Completed ARP Ping Scan at 17:37, 0.00s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 17:37
Completed Parallel DNS resolution of 1 host. at 17:38, 13.01s elapsed
Initiating SYN Stealth Scan at 17:38
Scanning 10.0.3.136 [1000 ports]
Discovered open port 80/tcp on 10.0.3.136
Completed SYN Stealth Scan at 17:38, 0.12s elapsed (1000 total ports)
Initiating UDP Scan at 17:38
Scanning 10.0.3.136 [1000 ports]
Increasing send delay for 10.0.3.136 from 0 to 50 due to max_successful_tryno increase to 5
Increasing send delay for 10.0.3.136 from 50 to 100 due to max_successful_tryno increase to 6
Warning: 10.0.3.136 giving up on port because retransmission cap hit (6).
Increasing send delay for 10.0.3.136 from 100 to 200 due to 11 out of 11 dropped probes since last increase.
UDP Scan Timing: About 8.36% done; ETC: 17:44 (0:05:40 remaining)
Increasing send delay for 10.0.3.136 from 200 to 400 due to 11 out of 11 dropped probes since last increase.
Increasing send delay for 10.0.3.136 from 400 to 800 due to 11 out of 11 dropped probes since last increase.
UDP Scan Timing: About 12.10% done; ETC: 17:46 (0:07:23 remaining)
UDP Scan Timing: About 15.07% done; ETC: 17:48 (0:08:33 remaining)
UDP Scan Timing: About 18.44% done; ETC: 17:49 (0:09:08 remaining)
UDP Scan Timing: About 38.17% done; ETC: 17:51 (0:08:32 remaining)
UDP Scan Timing: About 44.27% done; ETC: 17:52 (0:07:50 remaining)
UDP Scan Timing: About 50.40% done; ETC: 17:52 (0:07:06 remaining)
UDP Scan Timing: About 56.24% done; ETC: 17:52 (0:06:21 remaining)
UDP Scan Timing: About 61.76% done; ETC: 17:52 (0:05:37 remaining)
UDP Scan Timing: About 67.07% done; ETC: 17:52 (0:04:52 remaining)
UDP Scan Timing: About 72.74% done; ETC: 17:53 (0:04:04 remaining)
UDP Scan Timing: About 77.96% done; ETC: 17:53 (0:03:19 remaining)
UDP Scan Timing: About 83.17% done; ETC: 17:53 (0:02:33 remaining)
UDP Scan Timing: About 88.39% done; ETC: 17:53 (0:01:46 remaining)
UDP Scan Timing: About 93.60% done; ETC: 17:53 (0:00:59 remaining)
Completed UDP Scan at 17:54, 952.34s elapsed (1000 total ports)
Initiating Service scan at 17:54
Scanning 38 services on 10.0.3.136
Service scan Timing: About 5.26% done; ETC: 18:06 (0:11:42 remaining)
Service scan Timing: About 10.53% done; ETC: 18:05 (0:10:29 remaining)
Completed Service scan at 17:56, 137.61s elapsed (38 services on 1 host)
Initiating OS detection (try #1) against 10.0.3.136
NSE: Script scanning 10.0.3.136.
Initiating NSE at 17:56
Completed NSE at 18:05, 536.50s elapsed
Initiating NSE at 18:05
Completed NSE at 18:05, 2.31s elapsed
Initiating NSE at 18:05
Completed NSE at 18:05, 0.01s elapsed
Nmap scan report for 10.0.3.136
Host is up (0.00055s latency).
Not shown: 1961 closed ports
PORT      STATE         SERVICE   VERSION
80/tcp    open          http      Apache httpd 2.2.14 ((Ubuntu))
|_http-apache-negotiation: mod_negotiation enabled.
|_http-comments-displayer: Couldn't find any comments.
| http-headers: 
|   Date: Sun, 18 Apr 2021 21:56:36 GMT
|   Server: Apache/2.2.14 (Ubuntu)
|   X-Powered-By: PHP/5.3.2-1ubuntu4.7
|   Vary: Accept-Encoding
|   Connection: close
|   Content-Type: text/html
|   
|_  (Request type: HEAD)
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-mobileversion-checker: No mobile version detected.
| http-php-version: Versions from logo query (less accurate): 5.3.0 - 5.3.29, 5.4.0 - 5.4.45
| Versions from credits query (more accurate): 5.3.1 - 5.3.2
|_Version from header x-powered-by: PHP/5.3.2-1ubuntu4.7
|_http-referer-checker: Couldn't find any cross-domain scripts.
|_http-security-headers: 
|_http-traceroute: ERROR: Script execution failed (use -d to debug)
| http-useragent-tester: 
|   Allowed User Agents: 
|     Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)
|     libwww
|     lwp-trivial
|     libcurl-agent/1.0
|     PHP/
|     Python-urllib/2.5
|     GT::WWW
|     Snoopy
|     MFC_Tear_Sample
|     HTTP::Lite
|     PHPCrawl
|     URI::Fetch
|     Zend_Http_Client
|     http client
|     PECL::HTTP
|     Wget/1.13.4 (linux-gnu)
|_    WWW-Mechanize/1.34
|_http-xssed: No previously reported XSS vuln.
666/tcp   filtered      doom
53/udp    open|filtered domain
68/udp    open|filtered dhcpc
120/udp   open|filtered cfdptkt
177/udp   open|filtered xdmcp
514/udp   open|filtered syslog
1014/udp  open|filtered unknown
1019/udp  open|filtered unknown
1020/udp  open|filtered unknown
1029/udp  open|filtered solid-mux
1039/udp  open|filtered sbl
1050/udp  open|filtered cma
16545/udp open|filtered unknown
16680/udp open|filtered unknown
16974/udp open|filtered unknown
17077/udp open|filtered unknown
17683/udp open|filtered unknown
17836/udp open|filtered unknown
17939/udp open|filtered unknown
18250/udp open|filtered unknown
19130/udp open|filtered unknown
19283/udp open|filtered keysrvr
20560/udp open|filtered unknown
20742/udp open|filtered unknown
20851/udp open|filtered unknown
21710/udp open|filtered unknown
24007/udp open|filtered unknown
25337/udp open|filtered unknown
29823/udp open|filtered unknown
32818/udp open|filtered unknown
40732/udp open|filtered unknown
41308/udp open|filtered unknown
43094/udp open|filtered unknown
47772/udp open|filtered unknown
49166/udp open|filtered unknown
49168/udp open|filtered unknown
49182/udp open|filtered unknown
57172/udp open|filtered unknown
MAC Address: 00:0C:29:61:B0:A1 (VMware)
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.17 - 2.6.36
Uptime guess: 0.736 days (since Sun Apr 18 00:25:30 2021)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=194 (Good luck!)
IP ID Sequence Generation: All zeros

Host script results:
|_fcrdns: FAIL (No PTR record)
| firewalk: 
| HOP  HOST        PROTOCOL  BLOCKED PORTS
|_0    10.0.3.129  udp       53,68,120,177,514,1014,1019-1020,1029,1039
|_ipidseq: ERROR: Script execution failed (use -d to debug)
|_path-mtu: PMTU == 1500
|_qscan: ERROR: Script execution failed (use -d to debug)
| traceroute-geolocation: 
|   HOP  RTT   ADDRESS     GEOLOCATION
|_  1    0.55  10.0.3.136  - ,- 

TRACEROUTE
HOP RTT     ADDRESS
1   0.55 ms 10.0.3.136

NSE: Script Post-scanning.
Initiating NSE at 18:05
Completed NSE at 18:05, 0.00s elapsed
Initiating NSE at 18:05
Completed NSE at 18:05, 0.00s elapsed
Initiating NSE at 18:05
Completed NSE at 18:05, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1654.55 seconds
           Raw packets sent: 2610 (94.407KB) | Rcvd: 2054 (99.204KB)

```





### Uniscan

```shell
####################################
# Uniscan project                  #
# http://uniscan.sourceforge.net/  #
####################################
V. 6.3


Scan date: 14-4-2021 17:59:9
===================================================================================================
| Domain: http://192.168.56.107/
| Server: Apache/2.2.14 (Ubuntu)
| IP: 192.168.56.107
===================================================================================================
===================================================================================================
| Looking for Drupal plugins/modules
| 
| GET,HEAD,POST,OPTIONS
===================================================================================================
===================================================================================================
| WEB SERVICES
| 
===================================================================================================
| FAVICON.ICO
| 
===================================================================================================
| ERROR INFORMATION
| 
|  404 Not Found Not Found The requested URL /&quot;eC7XzdMB4&gt;aSbzV@Ah was not found on this server. Apache/2.2.14 (Ubuntu) Server at 192.168.56.107 Port 80 
|  404 Not Found Not Found The requested URL /xfYphn}zb))/-z_[O}k was not found on this server. Apache/2.2.14 (Ubuntu) Server at 192.168.56.107 Port 80 
===================================================================================================
| TYPE ERROR
| 
===================================================================================================
| SERVER MOBILE
| 
===================================================================================================
| LANGUAGE
| 
===================================================================================================
| INTERESTING STRINGS IN HTML
| 
| font color="green">Password
| input name="password" type="text" id="password">
| strong>Member Login 
===================================================================================================
| WHOIS
| 
===================================================================================================
| BANNER GRABBING: 
===================================================================================================
===================================================================================================
| PING
| 
| PING 192.168.56.107 (192.168.56.107) 56(84) bytes of data.
| 64 bytes from 192.168.56.107: icmp_seq=1 ttl=64 time=0.435 ms
| 64 bytes from 192.168.56.107: icmp_seq=2 ttl=64 time=0.930 ms
| 64 bytes from 192.168.56.107: icmp_seq=3 ttl=64 time=0.577 ms
| 64 bytes from 192.168.56.107: icmp_seq=4 ttl=64 time=0.690 ms
| 
| --- 192.168.56.107 ping statistics ---
| 4 packets transmitted, 4 received, 0% packet loss, time 3054ms
| rtt min/avg/max/mdev = 0.435/0.658/0.930/0.181 ms
===================================================================================================
| TRACEROUTE
| 
| traceroute to 192.168.56.107 (192.168.56.107), 30 hops max, 60 byte packets
|  1  192.168.56.107 (192.168.56.107)  0.662 ms  0.487 ms  0.372 ms
===================================================================================================
| NSLOOKUP
| 
| ;; connection timed out; no servers could be reached
| 
===================================================================================================
| NMAP
| 
| ;; connection timed out; no servers could be reached
| 
===================================================================================================
| NMAP
| 
| Starting Nmap 7.80 ( https://nmap.org ) at 2021-04-14 18:03 EDT
| NSE: Loaded 151 scripts for scanning.
| NSE: Script Pre-scanning.
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating ARP Ping Scan at 18:03
| Scanning 192.168.56.105 [1 port]
| Completed ARP Ping Scan at 18:03, 0.01s elapsed (1 total hosts)
| Initiating SYN Stealth Scan at 18:03
| Scanning 192.168.56.105 [1000 ports]
| Completed SYN Stealth Scan at 18:03, 0.14s elapsed (1000 total ports)
| Initiating Service scan at 18:03
| Initiating OS detection (try #1) against 192.168.56.105
| Retrying OS detection (try #2) against 192.168.56.105
| NSE: Script scanning 192.168.56.105.
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Nmap scan report for 192.168.56.105
| Host is up (0.00028s latency).
| All 1000 scanned ports on 192.168.56.105 are closed
| MAC Address: 08:00:27:61:74:64 (Oracle VirtualBox virtual NIC)
| Too many fingerprints match this host to give specific OS details
| Network Distance: 1 hop
| 
| TRACEROUTE
| HOP RTT     ADDRESS
| 1   0.28 ms 192.168.56.105
| 
| NSE: Script Post-scanning.
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Read data files from: /usr/bin/../share/nmap
| OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
| Nmap done: 1 IP address (1 host up) scanned in 2.63 seconds
|            Raw packets sent: 1013 (45.696KB) | Rcvd: 1013 (41.632KB)
===================================================================================================
|
| Directory check:
| Skipped because http://192.168.56.105/uniscan81/ did not return the code 404
===================================================================================================
|                                                                                                   
| File check:
| Skipped because http://192.168.56.105/uniscan834/ did not return the code 404
===================================================================================================
|
| Check robots.txt:
|
| Check sitemap.xml:
===================================================================================================
===================================================================================================
Scan end date: 14-4-2021 18:3:8



HTML report saved in: report/192.168.56.105.html
| ;; connection timed out; no servers could be reached
| 
===================================================================================================
| NMAP
| 
| Starting Nmap 7.80 ( https://nmap.org ) at 2021-04-14 18:03 EDT
| NSE: Loaded 151 scripts for scanning.
| NSE: Script Pre-scanning.
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating ARP Ping Scan at 18:03
| Scanning 192.168.56.107 [1 port]
| Completed ARP Ping Scan at 18:03, 0.00s elapsed (1 total hosts)
| Initiating SYN Stealth Scan at 18:03
| Scanning 192.168.56.107 [1000 ports]
| Discovered open port 80/tcp on 192.168.56.107
| Completed SYN Stealth Scan at 18:03, 0.10s elapsed (1000 total ports)
| Initiating Service scan at 18:03
| Scanning 1 service on 192.168.56.107
| Completed Service scan at 18:03, 6.01s elapsed (1 service on 1 host)
| Initiating OS detection (try #1) against 192.168.56.107
| NSE: Script scanning 192.168.56.107.
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.10s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Nmap scan report for 192.168.56.107
| Host is up (0.00055s latency).
| Not shown: 998 closed ports
| PORT    STATE    SERVICE VERSION
| 80/tcp  open     http    Apache httpd 2.2.14 ((Ubuntu))
| | http-methods: 
| |_  Supported Methods: GET HEAD POST OPTIONS
| |_http-server-header: Apache/2.2.14 (Ubuntu)
| |_http-title: Hackademic.RTB2
| 666/tcp filtered doom
| MAC Address: 00:0C:29:61:B0:A1 (VMware)
| Device type: general purpose
| Running: Linux 2.6.X
| OS CPE: cpe:/o:linux:linux_kernel:2.6
| OS details: Linux 2.6.17 - 2.6.36
| Uptime guess: 0.038 days (since Wed Apr 14 17:09:00 2021)
| Network Distance: 1 hop
| TCP Sequence Prediction: Difficulty=201 (Good luck!)
| IP ID Sequence Generation: All zeros
| 
| TRACEROUTE
| HOP RTT     ADDRESS
| 1   0.55 ms 192.168.56.107
| 
| NSE: Script Post-scanning.
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:03, 0.00s elapsed
| Read data files from: /usr/bin/../share/nmap
| OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
| Nmap done: 1 IP address (1 host up) scanned in 8.68 seconds
|            Raw packets sent: 1020 (45.626KB) | Rcvd: 1016 (41.374KB)
===================================================================================================
|
| Directory check:
| [+] CODE: 200 URL: http://192.168.56.107/check/
| [+] CODE: 200 URL: http://192.168.56.107/icons/
| [+] CODE: 200 URL: http://192.168.56.107/index/
| [+] CODE: 200 URL: http://192.168.56.107/phpmyadmin/
===================================================================================================
|                                                                                                   
| File check:
| [+] CODE: 200 URL: http://192.168.56.107/index.php
===================================================================================================
|
| Check robots.txt:
|
| Check sitemap.xml:
===================================================================================================
===================================================================================================
Scan end date: 14-4-2021 18:4:16



HTML report saved in: report/192.168.56.107.html
| Starting Nmap 7.80 ( https://nmap.org ) at 2021-04-14 18:02 EDT
| NSE: Loaded 151 scripts for scanning.
| NSE: Script Pre-scanning.
| Initiating NSE at 18:02
| Completed NSE at 18:02, 0.00s elapsed
| Initiating NSE at 18:02
| Completed NSE at 18:02, 0.00s elapsed
| Initiating NSE at 18:02
| Completed NSE at 18:02, 0.00s elapsed
| Initiating ARP Ping Scan at 18:02
| Scanning 192.168.56.104 [1 port]
| Completed ARP Ping Scan at 18:02, 0.00s elapsed (1 total hosts)
| Initiating SYN Stealth Scan at 18:02
| Scanning 192.168.56.104 [1000 ports]
| Discovered open port 22/tcp on 192.168.56.104
| Discovered open port 80/tcp on 192.168.56.104
| Discovered open port 445/tcp on 192.168.56.104
| Discovered open port 993/tcp on 192.168.56.104
| Discovered open port 111/tcp on 192.168.56.104
| Discovered open port 995/tcp on 192.168.56.104
| Discovered open port 25/tcp on 192.168.56.104
| Discovered open port 21/tcp on 192.168.56.104
| Discovered open port 139/tcp on 192.168.56.104
| Discovered open port 143/tcp on 192.168.56.104
| Discovered open port 443/tcp on 192.168.56.104
| Discovered open port 3306/tcp on 192.168.56.104
| Discovered open port 110/tcp on 192.168.56.104
| Discovered open port 5904/tcp on 192.168.56.104
| Discovered open port 5802/tcp on 192.168.56.104
| Discovered open port 6003/tcp on 192.168.56.104
| Discovered open port 6002/tcp on 192.168.56.104
| Discovered open port 6004/tcp on 192.168.56.104
| Discovered open port 5903/tcp on 192.168.56.104
| Discovered open port 5902/tcp on 192.168.56.104
| Discovered open port 6001/tcp on 192.168.56.104
| Discovered open port 5901/tcp on 192.168.56.104
| Discovered open port 5801/tcp on 192.168.56.104
| Completed SYN Stealth Scan at 18:02, 0.08s elapsed (1000 total ports)
| Initiating Service scan at 18:02
| Scanning 23 services on 192.168.56.104
| Completed Service scan at 18:02, 14.04s elapsed (23 services on 1 host)
| Initiating OS detection (try #1) against 192.168.56.104
| NSE: Script scanning 192.168.56.104.
| Initiating NSE at 18:02
| NSE: [ftp-bounce] Couldn't resolve scanme.nmap.org, scanning 10.0.0.1 instead.
| Completed NSE at 18:03, 32.21s elapsed
| Initiating NSE at 18:03
| Completed NSE at 18:06, 194.58s elapsed
| Initiating NSE at 18:06
| Completed NSE at 18:06, 0.00s elapsed
| Nmap scan report for 192.168.56.104
| Host is up (0.00043s latency).
| Not shown: 977 closed ports
| PORT     STATE SERVICE     VERSION
| 21/tcp   open  ftp         vsftpd 2.0.5
| | ftp-anon: Anonymous FTP login allowed (FTP code 230)
| |_drwxr-xr-x    2 0        0            4096 Jun 05  2013 pub
| | ftp-syst: 
| |   STAT: 230
| |_Login successful.
| 22/tcp   open  ssh         OpenSSH 4.3 (protocol 2.0)
| | ssh-hostkey: 
| |   1024 5e:ca:64:f0:7f:d2:1a:a2:86:c6:1f:c2:2a:b3:6b:27 (DSA)
| |_  2048 a3:39:2d:9f:66:96:0d:82:ad:52:1f:a1:dc:b1:f1:54 (RSA)
| 25/tcp   open  smtp        Sendmail
| | smtp-commands: localhost.localdomain Hello [192.168.56.106], pleased to meet you, ENHANCEDSTATUSCODES, PIPELINING, EXPN, VERB, 8BITMIME, SIZE, DSN, ETRN, DELIVERBY, HELP, 
| |_ 2.0.0 This is sendmail 2.0.0 Topics: 2.0.0 HELO EHLO MAIL RCPT DATA 2.0.0 RSET NOOP QUIT HELP VRFY 2.0.0 EXPN VERB ETRN DSN AUTH 2.0.0 STARTTLS 2.0.0 For more info use "HELP <topic>". 2.0.0 To report bugs in the implementation see 2.0.0 http://www.sendmail.org/email-addresses.html 2.0.0 For local information send email to Postmaster at your site. 2.0.0 End of HELP info 
| 80/tcp   open  http        Apache httpd 2.2.3 ((CentOS))
| |_http-favicon: Drupal CMS
| | http-git: 
| |   192.168.56.104:80/.git/
| |     Git repository found!
| |     Repository description: Unnamed repository; edit this file 'description' to name the...
| |_    Last commit message: initial commit 
| | http-methods: 
| |_  Supported Methods: GET HEAD POST OPTIONS
| | http-robots.txt: 36 disallowed entries (15 shown)
| | /includes/ /misc/ /modules/ /profiles/ /scripts/ 
| | /sites/ /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt 
| | /INSTALL.pgsql.txt /install.php /INSTALL.txt /LICENSE.txt 
| |_/MAINTAINERS.txt
| |_http-server-header: Apache/2.2.3 (CentOS)
| 110/tcp  open  pop3        Dovecot pop3d
| |_pop3-capabilities: PIPELINING STLS RESP-CODES USER TOP SASL(PLAIN) CAPA UIDL
| |_ssl-date: 2021-04-14T16:17:38+00:00; -5h45m58s from scanner time.
| 111/tcp  open  rpcbind     2 (RPC #100000)
| 139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
| 143/tcp  open  imap        Dovecot imapd
| |_ssl-date: 2021-04-14T16:17:38+00:00; -5h45m58s from scanner time.
| 443/tcp  open  ssl/https?
| |_ssl-date: 2021-04-14T16:17:35+00:00; -5h45m58s from scanner time.
| 445/tcp  open  netbios-ssn Samba smbd 3.0.33-3.7.el5 (workgroup: WORKGROUP)
| 993/tcp  open  ssl/imaps?
| |_ssl-date: 2021-04-14T16:17:35+00:00; -5h45m58s from scanner time.
| | sslv2: 
| |   SSLv2 supported
| |   ciphers: 
| |     SSL2_RC4_128_WITH_MD5
| |     SSL2_DES_192_EDE3_CBC_WITH_MD5
| |     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
| |     SSL2_RC2_128_CBC_WITH_MD5
| |_    SSL2_RC4_128_EXPORT40_WITH_MD5
| 995/tcp  open  ssl/pop3s?
| |_ssl-date: 2021-04-14T16:17:35+00:00; -5h45m58s from scanner time.
| 3306/tcp open  mysql       MySQL (unauthorized)
| 5801/tcp open  vnc-http    RealVNC 4.0 (resolution: 400x250; VNC TCP port: 5901)
| | http-methods: 
| |_  Supported Methods: GET HEAD
| |_http-server-header: RealVNC/4.0
| |_http-title: VNC viewer for Java
| 5802/tcp open  vnc-http    RealVNC 4.0 (resolution: 400x250; VNC TCP port: 5902)
| | http-methods: 
| |_  Supported Methods: GET HEAD
| |_http-server-header: RealVNC/4.0
| |_http-title: VNC viewer for Java
| 5901/tcp open  vnc         VNC (protocol 3.8)
| |_ssl-cert: ERROR: Script execution failed (use -d to debug)
| |_ssl-date: ERROR: Script execution failed (use -d to debug)
| |_sslv2: ERROR: Script execution failed (use -d to debug)
| |_tls-nextprotoneg: ERROR: Script execution failed (use -d to debug)
| |_vnc-info: ERROR: Script execution failed (use -d to debug)
| 5902/tcp open  vnc         VNC (protocol 3.8)
| |_ssl-cert: ERROR: Script execution failed (use -d to debug)
| |_sslv2: ERROR: Script execution failed (use -d to debug)
| |_tls-alpn: ERROR: Script execution failed (use -d to debug)
| |_tls-nextprotoneg: ERROR: Script execution failed (use -d to debug)
| |_vnc-info: ERROR: Script execution failed (use -d to debug)
| 5903/tcp open  vnc         VNC (protocol 3.8)
| |_ssl-cert: ERROR: Script execution failed (use -d to debug)
| |_sslv2: ERROR: Script execution failed (use -d to debug)
| |_tls-alpn: ERROR: Script execution failed (use -d to debug)
| |_tls-nextprotoneg: ERROR: Script execution failed (use -d to debug)
| |_vnc-info: ERROR: Script execution failed (use -d to debug)
| 5904/tcp open  vnc         VNC (protocol 3.8)
| |_ssl-cert: ERROR: Script execution failed (use -d to debug)
| |_sslv2: ERROR: Script execution failed (use -d to debug)
| |_tls-alpn: ERROR: Script execution failed (use -d to debug)
| |_tls-nextprotoneg: ERROR: Script execution failed (use -d to debug)
| |_vnc-info: ERROR: Script execution failed (use -d to debug)
| 6001/tcp open  X11         (access denied)
| 6002/tcp open  X11         (access denied)
| 6003/tcp open  X11         (access denied)
| 6004/tcp open  X11         (access denied)
| MAC Address: 08:00:27:93:BC:47 (Oracle VirtualBox virtual NIC)
| Device type: general purpose
| Running: Linux 2.6.X
| OS CPE: cpe:/o:linux:linux_kernel:2.6
| OS details: Linux 2.6.9 - 2.6.30
| Uptime guess: 1.466 days (since Tue Apr 13 06:55:43 2021)
| Network Distance: 1 hop
| TCP Sequence Prediction: Difficulty=205 (Good luck!)
| IP ID Sequence Generation: All zeros
| Service Info: OS: Unix
| 
| Host script results:
| |_clock-skew: mean: -5h11m39s, deviation: 1h30m47s, median: -5h45m58s
| | nbstat: NetBIOS name: LAMPSEC, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| | Names:
| |   LAMPSEC<00>          Flags: <unique><active>
| |   LAMPSEC<03>          Flags: <unique><active>
| |   LAMPSEC<20>          Flags: <unique><active>
| |   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
| |   WORKGROUP<1d>        Flags: <unique><active>
| |   WORKGROUP<1e>        Flags: <group><active>
| |_  WORKGROUP<00>        Flags: <group><active>
| | smb-os-discovery: 
| |   OS: Unix (Samba 3.0.33-3.7.el5)
| |   Computer name: localhost
| |   NetBIOS computer name: 
| |   Domain name: localdomain
| |   FQDN: localhost.localdomain
| |_  System time: 2021-04-14T12:17:14-04:00
| | smb-security-mode: 
| |   account_used: guest
| |   authentication_level: user
| |   challenge_response: supported
| |_  message_signing: disabled (dangerous, but default)
| |_smb2-time: Protocol negotiation failed (SMB2)
| 
| TRACEROUTE
| HOP RTT     ADDRESS
| 1   0.43 ms 192.168.56.104
| 
| NSE: Script Post-scanning.
| Initiating NSE at 18:06
| Completed NSE at 18:06, 0.00s elapsed
| Initiating NSE at 18:06
| Completed NSE at 18:06, 0.00s elapsed
| Initiating NSE at 18:06
| Completed NSE at 18:06, 0.00s elapsed
| Read data files from: /usr/bin/../share/nmap
| OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
| Nmap done: 1 IP address (1 host up) scanned in 243.16 seconds
|            Raw packets sent: 1020 (45.626KB) | Rcvd: 1019 (42.674KB)
===================================================================================================
|
| Directory check:
| [+] CODE: 200 URL: http://192.168.56.104/contact/
| [+] CODE: 200 URL: http://192.168.56.104/icons/
| [+] CODE: 200 URL: http://192.168.56.104/includes/
| [+] CODE: 200 URL: http://192.168.56.104/mail/
| [+] CODE: 200 URL: http://192.168.56.104/modules/

```

##### Notes I take from these results

**Login**
```
| font color="green">Password
| input name="password" type="text" id="password">
| strong>Member Login 
```
> This initially tells me that I may need to perform some type of injections of some kind.



- Web Server Version
```
80/tcp  open     http    Apache httpd 2.2.14 ((Ubuntu))
| | http-methods: 
| |_  Supported Methods: GET HEAD POST OPTIONS
| |_http-server-header: Apache/2.2.14 (Ubuntu)
| |_http-title: Hackademic.RTB2
```
- This allows to see if there are any vulnerabilities for this specific version of Apache httpd

- Operating System
```
| Running: Linux 2.6.X
| OS CPE: cpe:/o:linux:linux_kernel:2.6
| OS details: Linux 2.6.17 - 2.6.36
```

- Services and Protols

```
| 21/tcp   open  ftp         vsftpd 2.0.5
| | ftp-anon: Anonymous FTP login allowed (FTP code 230)
| |_drwxr-xr-x    2 0        0            4096 Jun 05  2013 pub
| | ftp-syst: 
| |   STAT: 230
| |_Login successful.
```


```
| 22/tcp   open  ssh         OpenSSH 4.3 (protocol 2.0)
| | ssh-hostkey: 
| |   1024 5e:ca:64:f0:7f:d2:1a:a2:86:c6:1f:c2:2a:b3:6b:27 (DSA)
| |_  2048 a3:39:2d:9f:66:96:0d:82:ad:52:1f:a1:dc:b1:f1:54 (RSA)
```


```
| 25/tcp   open  smtp        Sendmail
| | smtp-commands: localhost.localdomain Hello [192.168.56.106], pleased to meet you, ENHANCEDSTATUSCODES, PIPELINING, EXPN, VERB, 8BITMIME, SIZE, DSN, ETRN, DELIVERBY, HELP, 
| |_ 2.0.0 This is sendmail 2.0.0 Topics: 2.0.0 HELO EHLO MAIL RCPT DATA 2.0.0 RSET NOOP QUIT HELP VRFY 2.0.0 EXPN VERB ETRN DSN AUTH 2.0.0 STARTTLS 2.0.0 For more info use "HELP <topic>". 2.0.0 To report bugs in the implementation see 2.0.0 http://www.sendmail.org/email-addresses.html 2.0.0 For local information send email to Postmaster at your site. 2.0.0 End of HELP info 
```


```
| 80/tcp   open  http        Apache httpd 2.2.3 ((CentOS))
| |_http-favicon: Drupal CMS
| | http-git: 
| |   192.168.56.104:80/.git/
| |     Git repository found!
| |     Repository description: Unnamed repository; edit this file 'description' to name the...
| |_    Last commit message: initial commit 
| | http-methods: 
| |_  Supported Methods: GET HEAD POST OPTIONS
| | http-robots.txt: 36 disallowed entries (15 shown)
| | /includes/ /misc/ /modules/ /profiles/ /scripts/ 
| | /sites/ /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt 
| | /INSTALL.pgsql.txt /install.php /INSTALL.txt /LICENSE.txt 
| |_/MAINTAINERS.txt
| |_http-server-header: Apache/2.2.3 (CentOS)
```


```
| 110/tcp  open  pop3        Dovecot pop3d
| |_pop3-capabilities: PIPELINING STLS RESP-CODES USER TOP SASL(PLAIN) CAPA UIDL
| |_ssl-date: 2021-04-14T16:17:38+00:00; -5h45m58s from scanner time.
```


```
| 111/tcp  open  rpcbind     2 (RPC #100000)
```


```
| 139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
```

```
| 143/tcp  open  imap        Dovecot imapd
| |_ssl-date: 2021-04-14T16:17:38+00:00; -5h45m58s from scanner time.
```


```
| 443/tcp  open  ssl/https?
| |_ssl-date: 2021-04-14T16:17:35+00:00; -5h45m58s from scanner time.
```



```
| 445/tcp  open  netbios-ssn Samba smbd 3.0.33-3.7.el5 (workgroup: WORKGROUP)
```

```
| 993/tcp  open  ssl/imaps?
| |_ssl-date: 2021-04-14T16:17:35+00:00; -5h45m58s from scanner time.
| | sslv2: 
| |   SSLv2 supported
| |   ciphers: 
| |     SSL2_RC4_128_WITH_MD5
| |     SSL2_DES_192_EDE3_CBC_WITH_MD5
| |     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
| |     SSL2_RC2_128_CBC_WITH_MD5
| |_    SSL2_RC4_128_EXPORT40_WITH_MD5
```



```
| 995/tcp  open  ssl/pop3s?
| |_ssl-date: 2021-04-14T16:17:35+00:00; -5h45m58s from scanner time.
```


```
| 3306/tcp open  mysql       MySQL (unauthorized)
```


```
| 5801/tcp open  vnc-http    RealVNC 4.0 (resolution: 400x250; VNC TCP port: 5901)
| | http-methods: 
| |_  Supported Methods: GET HEAD
| |_http-server-header: RealVNC/4.0
| |_http-title: VNC viewer for Java
```


```
| 5802/tcp open  vnc-http    RealVNC 4.0 (resolution: 400x250; VNC TCP port: 5902)
| | http-methods: 
| |_  Supported Methods: GET HEAD
| |_http-server-header: RealVNC/4.0
| |_http-title: VNC viewer for Java
```



```
| 5901/tcp open  vnc         VNC (protocol 3.8)
```


```
| 5902/tcp open  vnc         VNC (protocol 3.8)
```


```
| 5903/tcp open  vnc         VNC (protocol 3.8)
```


```
| 5904/tcp open  vnc         VNC (protocol 3.8)
```


```
| | nbstat: NetBIOS name: LAMPSEC, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| | Names:
| |   LAMPSEC<00>          Flags: <unique><active>
| |   LAMPSEC<03>          Flags: <unique><active>
| |   LAMPSEC<20>          Flags: <unique><active>
| |   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
| |   WORKGROUP<1d>        Flags: <unique><active>
| |   WORKGROUP<1e>        Flags: <group><active>
| |_  WORKGROUP<00>        Flags: <group><active>
```



```
| | smb-os-discovery: 
| |   OS: Unix (Samba 3.0.33-3.7.el5)
| |   Computer name: localhost
| |   NetBIOS computer name: 
| |   Domain name: localdomain
| |   FQDN: localhost.localdomain
| |_  System time: 2021-04-14T12:17:14-04:00
| | smb-security-mode: 
| |   account_used: guest
| |   authentication_level: user
| |   challenge_response: supported
| |_  message_signing: disabled (dangerous, but default)
| |_smb2-time: Protocol negotiation failed (SMB2)
```










- phpmyadmin
```
| [+] CODE: 200 URL: http://192.168.56.107/phpmyadmin/
```













> I took a break here and I restarted the VM so the IP address changed


`http://192.168.1.145`




