1. TO CRACK HASHES FOR LINux - we must first use unshadow with both /etc/passwd and /etc/shadow and feed to john the ripper
```
root@silentkiller-kali:/home/madman# unshadow
Usage: unshadow PASSWORD-FILE SHADOW-FILE
root@silentkiller-kali:/home/madman# unshadow pass.txt shadw.txt
root@silentkiller-kali:/home/madman# unshadow pass.txt shadw.txt > unshadow.txt

```