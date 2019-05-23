* Challenge

Daddy, teach me how to use random value in programming!

ssh random@pwnable.kr -p2222 (pw:guest)

* Analysis

The code calls rand() to get a "random" number without providing a
seed. This random number is always:

random = 1804289383

Then it uses scanf to read a key and it uses the following check to
get to the flag

(key ^ random) = 0xdeadbeef

This condition will evaluate to true when

key = random ^ 0xdeadbeef = 1804289383 ^ 0xdeadbeef = 3039230856
