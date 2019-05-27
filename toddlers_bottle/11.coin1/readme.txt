* Challenge

Mommy, I wanna play a game!
(if your network response time is too slow, try nc 0 9007 inside pwnable.kr server)

Running at : nc pwnable.kr 9007

* Analysis

The game gives you a set of coins where one of the them weighs less (9
units) than the others (10 units). The goal is to find the coin that
weighs less (9 units) by making queries to the server where in each
query you can weigh a subset of the set of coins. You always have more
than enough attempts to do a binary search. To get the flag run:

$> ./exploit.py

* Flag

b1NaRy_S34rch1nG_1s_3asy_p3asy
