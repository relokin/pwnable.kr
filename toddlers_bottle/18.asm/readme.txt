* Challenge

Mommy! I think I know how to make shellcodes

ssh asm@pwnable.kr -p2222 (pw: guest)

* Analysis

Pretty straightforward challenge. The server will read a buffer of up
to 1000 bytes and then run the input assuming amd64 code. The
shellcode is limited to using the syscalls: open, read, write and
exit, but these are more than enough to open the file with the flag
and cat to the stdout.

To get the flag:

$> ./exploit.py

* Flag

Mak1ng_shelLcodE_i5_veRy_eaSy
