# Echo

> We can view the contents of a given environment variable with the echo command followed by the `$` character and an environment variable name. Take a look at the contents of the PATH environment variable.

```bash
austin@songer:-$ echo $PATH
/usr/local/sbi n:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

```bash
austin@songer:-$ echo $USER

```

```bash
austin@songer:-$ echo $PWD
```


```bash
austin@songer:-$ echo $HOME
```

```bash
austin@songer:-$ export b=le.11.1.220
```

# Export

> The export command makes the variable accessible to any subprocesses we might spawn from our current Bash instance. If we set an environment variable without export it will only be available in the current shell.
> `$$` variable to display the process ID of the current shell instance to make sure that we are indeed issuing commands in two different shells:


```bash
austin@songer:-$ echo"$$"
1827
```

```bash
austin@songer:-$ var="My Var"
```

```bash
austin@songer:-$ echo $var
My Var
```


```bash
austin@songer:-$ bash
austin@songer:-$ echo"$$"
1908
```

```bash
austin@songer:-$ echo $var
```


```bash
austin@songer:-$ exit
exit
```


```bash
austin@songer:-$ echo $var
My Var
```


```bash
austin@songer:-$ export othervar="Global Var"
```


```bash
austin@songer:-$ echo $othervar
Global Var
```


```bash
austin@songer:-$ bash
```


```bash
austin@songer:-$ echo $othervar
Global Var
```


```bash
austin@songer:-$ exit
exit
austin@songer:-$
```

# Env

> env at the command line:

```bash
austin@songer:-$ env
SHELL=/bin/bash
...
PWD=/home/songer
XDG_SESSION_DESKTOP=lightdm-xsession
LOGNAME=songer
XDG_SESSION_TYPE=xll
XAUTHORITY=/home/songer/.Xauthority
XDG_GREETER_DATA_DIR=/var/lib/lightdm/data/songer
HOME= /home/songer
...
TERM=xte rm-256color
USER=kali
```















