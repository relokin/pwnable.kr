* Challenge:

Nana told me that buffer overflow is one of the most common software vulnerability.
Is that true?

Download : http://pwnable.kr/bin/bof
Download : http://pwnable.kr/bin/bof.c

Running at : nc pwnable.kr 9000

* Analysis

Buffer overflow attack

Binary: bof
Source: bof.c (elf32-i386)
compile: gcc -g -m32 bof.c -o bof

Inside function func, a call to puts reads an unbounded buffer. Puts
writes to overflowme, which is located at -0x2c(%ebp). Later on a
check to variable key will tun control flow to system("/bin/ls") when
it has the value 0xcafebabe. key is located at 0x8(%ebp). The layout
of the stack when func is called looks roughly like this:

key
----------- <- +0x8
ret addr
----------- <- ebp
old_ebp
----------- <- -4
stack guard
----------- <- -0xc

overflowme
----------  <- -0x2c

Two attacks are possible:

1) Use a buffer overflow attack to overwrite the return address with the
address of the system("/bin/sh"). The binary is compile with
StackGuard and a canary value is pushed in the stack and checked
before ret is called. The value depends on the loader and therefore is
hard to guess.

2) Use a buffer overflow attack to overwrite the value of key. This
way we can manipulate the contents of key and change it to
0xcafebabe. This is feasible.

To perform this attack:
( ./exploit; cat) | nc pwnable.kr 9000
and once we get a shell in the remote server we get the flag with
cat flag

* Flag
daddy, I just pwned a buFFer :)
