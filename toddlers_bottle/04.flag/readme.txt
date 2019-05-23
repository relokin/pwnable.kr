* Challenge

Papa brought me a packed present! let's open it.

Download : http://pwnable.kr/bin/flag

This is reversing task. all you need is binary

* Analysis

The binary is a compressed executable. The compression is done with
UPX and it can be decompressed:
upx -d -o flag.dec flag

The resulting binary is an elf64-x86-64 elf file

Running the binary gives the following message:

I will malloc() and strcpy the flag there. take it.

Main calls malloc and then strcpy, the first argument of strcpy is the
in the .data section at location: 6c2070.

readelf -x .data  ./flag.dec

It's value is 28664900 which in little endian is the address
0x496628. This, an address in the .rodata segment, is the address of
the flag.

readelf -x .rodata  ./flag.dec

* Flag
UPX...? sounds like a delivery service :)
