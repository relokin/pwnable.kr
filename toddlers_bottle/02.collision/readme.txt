* Challenge

Daddy told me about cool MD5 hash collision today.
I wanna do something like that too!

ssh col@pwnable.kr -p2222 (pw:guest)

* Analysis

The binary reads the first command line argument which has to be 20
chars long. Then it passes to the check_password
function. Check_password will use the 20 bytes as 5 integers and
accumulate them. If the sum is equal to the hashcode 0x21DD09EC it
will print the flag.

* Solution

check solution.c

* Flag

daddy! I just managed to create a hash collision :)
