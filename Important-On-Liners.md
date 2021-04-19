
## 1st. Constant Ping Test

> Replace the IP address with your targets IP address. This has been useful for hackthebox to prevent my target machine from going down while I do stuff.

```
#------                      Constant Ping                       ------#
# You can add `&` at the end of the command to push to the background  #
#----------------------------------------------------------------------#

ip=<IP Address>

for i in {1..1000}; do ping -c 2 $ip | grep "0% packet loss"; echo "sleep for 60 sec and try again"; sleep 60; done
```


