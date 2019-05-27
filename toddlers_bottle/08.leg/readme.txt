* Challenge

Daddy told me I should study arm.
But I prefer to study my leg!

Download : http://pwnable.kr/bin/leg.c
Download : http://pwnable.kr/bin/leg.asm

ssh leg@pwnable.kr -p2222 (pw:guest)

* Analysis

key1 function returns through r0 the value of the pc at instruction
0x00008cdc and therefore:

key1 = 0x00008cdc + 0x8 = 0x00008cdc = 0x8ce4

key2 function switches to thumb an then return the contents of r3
which is set to the value fo the PC register at instruction 0x00008d04
and then 4 is added to it. Therefore:

key2 = 0x00008d04 + 0x4 + 0x4 =  0x00008d08 + 0x4 = 0x8d0c

key3 function returns through r0 the contents of register r3 in
the contents of lr is copied. Therefore:

key3 = 0x8d80

To get the flag execute:

$> key1=0x8ce4 key2=0x8d0c key3=0x8d80; echo $((key1 + key2 + key3)) | ./leg

* Flag

My daddy has a lot of ARMv5te muscle!
