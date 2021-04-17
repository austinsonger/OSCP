# Nmap

```
nmap -sC -sV -O -iP
nmap -p- --min-rate 10000 -oA scans/alltcp XX.XX.XX.XX
nmap -p- -v <targetip>
nmap -sT -sV -p- XX.XX.XX.XX -oA XX.XX.XX.XX
```
  
------------------------

## TCP Top 1000

```
nmap -Pn -sC -sV -oA tcp -vv $ip
```

------------------------
## All TCP Ports:

```
nmap -Pn -sC -sV -oA all -vv -p- $ip
```

------------------------
## UDP Top 100:

```
nmap -Pn -sU --top-ports 100 -oA udp -vv $ip
```

```
unicornscan -mU -v -I XX.XX.XX.XX
```

------------------------
## No Ping 

```
nmap -sV -sC -O -Pn XX.XX.XX.XX
```

------------------------
## All Port 

```
nmap -sV -sC -O -Pn  -p 1-65535 XX.XX.XX.XX
```

------------------------
## Nmap Detect And Write Stand-up Servers

```
nmap -v -sn XX.XX.XX.XX-254 -oG upHost.txt 
```

------------------------
## UDP Scan

```
nmap --top-ports 200 -sU -A XX.XX.XX.XX
```

------------------------
# With Netcat

```
nc -nvv -w 1 -z XX.XX.XX.XX 3388-3390
```

