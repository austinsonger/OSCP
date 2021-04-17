## Linux Buffer Overflow

1) First we identify the port and service running

2) We load it up in edb using the following command - edb --run /usr/games/vulnapp/bin/vulnapp and play 'Run' TWICE!

3) First we send our PoC and crash the application

4) Check with netcat to see port is open

5) Once our PoC is sent we see it crashes with EIP as AAAA so we need to generate a unique pattern with pattern_create.rb

`Usage: pattern_create.rb -l 4379`

6) Load this up in our PoC and fire!

7) Copy the address in the EIP overwrite access fault message and run it through `pattern_pffset
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 46367046`

8) Now we know the exact offset match at 4368 in this case so we sent B's to make sure EIP is correctly overwritten

9) Keep in mind sending more than 4379 characters causes the app to crash differently

10) We see the EIP overwritten by BBBB and our offset is correct now we must check for bad characters.

11) In this case adding more values where 'C' is, is pointless we need to add it before that so we subtract the total from A's and add the bad chars there to make them appear and analyse them.

12) We COUNT BADCHARS = 255 so we must subtract that from the A's and push our badchars in there

13) IF ERROR CODE AND APPLICATION EXITED JUST KILL THE PROCESS AND RESTART!

14) Now, we get an error showing a broken pipe fault and we can go in and see where the badchars truncated.

15) To observe we click on ESI/EAX and follow in dump

16) Only badchars we found were \x00,\x20

17) Now we must find a JMP ESP instruction address to point to our shellcode we search it turns out at the time of crash the EAX register points to the start of our shellcode space.

18) However at crash our ESP points to our C's and so we have 7 bytes of space and we know that EAX points to our shellcode so we do the following (This is in normal left to right) not like return address!!!!!
NOTE: AS THE SPACE IS 7 BYTES AND WE USE ONLY TWO PLEASE ' +' ADD TWO \x90's as padding!!!!!

add eax,12 ("over write the 'setup sound'" )
jmp eax (Start from there and we can use No op sled there to push it to our shellcode!)

19) we use nasm shell for this
`root@kali:~# /usr/share/metasploit-framework/tools/exploit/nasm_shell.rb
anasm > add eax,12
00000000  83C00C            add eax,byte +0xc
nasm > jmp eax
00000000  FFE0              jmp eax`

20) Now we need to find a suitable return address for the EIP so we can point it to our shellcode

21) Navigate to Opcode Search and change option to ESP -> EIP and search for JMP ESP (Crash the service first and then look for the JMP ESP)

22) Now that we have an address let's try out our return address instead of BBBB.

23) We set a breakpoint (before running the program) on our return address and we run our new PoC! We "Step OVER" our JMP ESP and see it points to out instructions and STEP OVER twice and it goes to our A's

24) Let's generate our shellcode and stuff it in there before the A's