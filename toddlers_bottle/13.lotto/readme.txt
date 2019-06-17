* Challenge

Mommy! I made a lotto program for my homework.
do you want to play?


ssh lotto@pwnable.kr -p2222 (pw:guest)

* Analysis

The game opens urandom and gets 6 bytes which it then turns into 6
integers in the range [1, 45]. Then it will compare with the 6 bytes
that the user provided. But in this check each number will be matched
again every byte of the input.

If for example one of the numbers is 1 and the user has provided an
input which corresponds to 6 consecutive 1 then the game will consider
this a perfect match. It will then output the flag. The range is
relatively small and within a small number of tries it will print the
flag.

To exploit run

$> /tmp/foobar/exploit.py ./lotto

* Flag

sorry mom... I FORGOT to check duplicate numbers... :(
