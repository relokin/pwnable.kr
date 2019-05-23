* Challenge

We all make mistakes, let's move on.
(don't take this too seriously, no fancy hacking skill is required at all)

This task is based on real event
Thanks to dhmonkey

hint : operator priority

ssh mistake@pwnable.kr -p2222 (pw:guest)

* Analysis

There is an obvious bug in how the code opens the password file and it
ends up reading the password from stdin. The code uses very simple
hashing (xor with a key) to verify the password and the user can
provide the password (2nd input) and the hash against which it is
checked (1st input). exploit.c computes a simple pair of password and
its hash.

* Flag
Mommy, the operator priority always confuses me :(
