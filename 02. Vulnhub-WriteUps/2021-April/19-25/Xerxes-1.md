




```
Nmap scan report for 10.0.3.2
Host is up (0.0014s latency).
Not shown: 65534 closed ports
PORT   STATE SERVICE VERSION
53/tcp open  domain  dnsmasq 2.78
| dns-nsid: 
|_  bind.version: dnsmasq-2.78
MAC Address: 00:50:56:E9:96:A6 (VMware)
Device type: specialized|general purpose|webcam
Running (JUST GUESSING): VMware Player (99%), Microsoft Windows XP|7|2012 (92%), DVTel embedded (89%)
OS CPE: cpe:/a:vmware:player cpe:/o:microsoft:windows_xp::sp3 cpe:/o:microsoft:windows_7 cpe:/o:microsoft:windows_server_2012
Aggressive OS guesses: VMware Player virtual NAT device (99%), Microsoft Windows XP SP3 or Windows 7 or Windows Server 2012 (92%), Microsoft Windows XP SP3 (91%), DVTel DVT-9540DW network camera (89%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=249 (Good luck!)
IP ID Sequence Generation: Busy server or unknown class

TRACEROUTE
HOP RTT     ADDRESS
1   1.42 ms 10.0.3.2

Nmap scan report for 10.0.3.128
Host is up (0.00033s latency).
Not shown: 65530 closed ports
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 6.0p1 Debian 4+deb7u2 (protocol 2.0)
| ssh-hostkey: 
|   1024 7f:0a:0d:81:50:3b:73:15:6b:9c:5e:09:a2:fc:82:91 (DSA)
|   2048 0d:eb:14:6d:b0:c5:eb:fc:84:2d:e8:a2:4e:9f:14:b4 (RSA)
|_  256 c1:ca:ae:c3:5d:7a:5b:9d:cf:27:a4:48:83:1e:01:84 (ECDSA)
80/tcp    open  http    lighttpd 1.4.31
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD POST
|_http-server-header: lighttpd/1.4.31
|_http-title: xerxes2
111/tcp   open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100024  1          33960/udp6  status
|   100024  1          34690/udp   status
|   100024  1          35384/tcp6  status
|_  100024  1          60751/tcp   status
4444/tcp  open  krb524?
| fingerprint-strings: 
|   GetRequest, NULL: 
|     //OAxAAAAAAAAAAAAEluZm8AAAAPAAAB+AABnD0AAwYICw0QEhUXGhwfISQmKSsuMDM1ODo9QUNG
|     SEtNUFJVV1pcX2FkZmlrbnBzdXh6fYGDhoiLjZCSlZeanJ+hpKapq66ws7W4ur3Bw8bIy83Q0tXX
|     2tzf4eTm6evu8PP1+Pr9AAAAOUxBTUUzLjk5cgFuAAAAAAAAAAAUQCQEdSIAAEAAAZw9c+tDPgAA
|     AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/zgMQAJiwOAClPQAEKEPV6vQ80
|     y3mm7PwTcTcXMesnZpqNXv3jxgVjyJ8MAAcI2DgPBxYueiIlbvfu73/y7vwn+7u7/+737u7v+6Ii
|     Ilf/7u///6O7u73/6IiIjv///+7u4oYiJ/u7u98ILi573//oiVuiInvoLnvoiECguLi57u7i4oKC
|     goKJUuKCgoZUu9y73wiI5Yue9wHAKAAYAMBcG4uLuHrhC0gNGRNptRqIxGIF9hY0ZCM8qOkBjB0f
|     z4qGhjW3XS8TqAboGJvfwWD/84LEKjYLupm/maACFJixkWAohA3YkmgtyB3CQDRcSgRcB7AAYyKY
|     AaPGy4sstmhFxWglI0GVLxFUi4cJwihomIWErHeQEghuZlE6YGi3fmJIFAyWXDxw3ZFM3dPW6e5c
|     J9TGB9I0SdqBmedSdkFWdkOnL5oZm5mYGajSim9NFaka6SC7tN1IX6btnETBj5oal90kVJLSTq1W
|     U6a9Ro/Vrut3QoJpKZ6bmizc86BumpN1rQnAkhKW4JOIVQ6q0th/lKggBGAGgLSsKgRLlO9qcv/z
|_    gsQVLLvqfCncgAGp63vV7uMMyHQFDBiFIHAAmYcCoAUAvCliDGiFV
60751/tcp open  status  1 (RPC #100024)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port4444-TCP:V=7.80%I=7%D=4/18%Time=607CD1C3%P=x86_64-pc-linux-gnu%r(NU
SF:LL,36A0,"//OAxAAAAAAAAAAAAEluZm8AAAAPAAAB\+AABnD0AAwYICw0QEhUXGhwfISQmK
SF:SsuMDM1ODo9QUNG\nSEtNUFJVV1pcX2FkZmlrbnBzdXh6fYGDhoiLjZCSlZeanJ\+hpKapq
SF:66ws7W4ur3Bw8bIy83Q0tXX\n2tzf4eTm6evu8PP1\+Pr9AAAAOUxBTUUzLjk5cgFuAAAAA
SF:AAAAAAUQCQEdSIAAEAAAZw9c\+tDPgAA\nAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
SF:AAAAAAAAAAAAP/zgMQAJiwOAClPQAEKEPV6vQ80\ny3mm7PwTcTcXMesnZpqNXv3jxgVjyJ
SF:8MAAcI2DgPBxYueiIlbvfu73/y7vwn\+7u7/\+737u7v\+6Ii\nIlf/7u///6O7u73/6IiI
SF:jv///\+7u4oYiJ/u7u98ILi573//oiVuiInvoLnvoiECguLi57u7i4oKC\ngoKJUuKCgoZU
SF:u9y73wiI5Yue9wHAKAAYAMBcG4uLuHrhC0gNGRNptRqIxGIF9hY0ZCM8qOkBjB0f\nz4qGh
SF:jW3XS8TqAboGJvfwWD/84LEKjYLupm/maACFJixkWAohA3YkmgtyB3CQDRcSgRcB7AAYyKY
SF:\nAaPGy4sstmhFxWglI0GVLxFUi4cJwihomIWErHeQEghuZlE6YGi3fmJIFAyWXDxw3ZFM3
SF:dPW6e5c\nJ9TGB9I0SdqBmedSdkFWdkOnL5oZm5mYGajSim9NFaka6SC7tN1IX6btnETBj5
SF:oal90kVJLSTq1W\nU6a9Ro/Vrut3QoJpKZ6bmizc86BumpN1rQnAkhKW4JOIVQ6q0th/lKg
SF:gBGAGgLSsKgRLlO9qcv/z\ngsQVLLvqfCncgAGp63vV7uMMyHQFDBiFIHAAmYcCoAUAvCli
SF:DGiFV")%r(GetRequest,8000,"//OAxAAAAAAAAAAAAEluZm8AAAAPAAAB\+AABnD0AAwY
SF:ICw0QEhUXGhwfISQmKSsuMDM1ODo9QUNG\nSEtNUFJVV1pcX2FkZmlrbnBzdXh6fYGDhoiL
SF:jZCSlZeanJ\+hpKapq66ws7W4ur3Bw8bIy83Q0tXX\n2tzf4eTm6evu8PP1\+Pr9AAAAOUx
SF:BTUUzLjk5cgFuAAAAAAAAAAAUQCQEdSIAAEAAAZw9c\+tDPgAA\nAAAAAAAAAAAAAAAAAAA
SF:AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAP/zgMQAJiwOAClPQAEKEPV6vQ80\ny3mm7PwTcTcX
SF:MesnZpqNXv3jxgVjyJ8MAAcI2DgPBxYueiIlbvfu73/y7vwn\+7u7/\+737u7v\+6Ii\nIl
SF:f/7u///6O7u73/6IiIjv///\+7u4oYiJ/u7u98ILi573//oiVuiInvoLnvoiECguLi57u7i
SF:4oKC\ngoKJUuKCgoZUu9y73wiI5Yue9wHAKAAYAMBcG4uLuHrhC0gNGRNptRqIxGIF9hY0Z
SF:CM8qOkBjB0f\nz4qGhjW3XS8TqAboGJvfwWD/84LEKjYLupm/maACFJixkWAohA3YkmgtyB
SF:3CQDRcSgRcB7AAYyKY\nAaPGy4sstmhFxWglI0GVLxFUi4cJwihomIWErHeQEghuZlE6YGi
SF:3fmJIFAyWXDxw3ZFM3dPW6e5c\nJ9TGB9I0SdqBmedSdkFWdkOnL5oZm5mYGajSim9NFaka
SF:6SC7tN1IX6btnETBj5oal90kVJLSTq1W\nU6a9Ro/Vrut3QoJpKZ6bmizc86BumpN1rQnAk
SF:hKW4JOIVQ6q0th/lKggBGAGgLSsKgRLlO9qcv/z\ngsQVLLvqfCncgAGp63vV7uMMyHQFDB
SF:iFIHAAmYcCoAUAvCliDGiFV");
MAC Address: 00:0C:29:6A:38:F5 (VMware)
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3
OS details: Linux 3.2 - 3.10, Linux 3.2 - 3.16
Uptime guess: 198.841 days (since Fri Oct  2 00:31:55 2020)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=256 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.33 ms 10.0.3.128
```




