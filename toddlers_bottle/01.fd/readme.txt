* Challenge

Mommy! what is a file descriptor in Linux?

try to play the wargame your self but if you are ABSOLUTE beginner,
follow this tutorial link: https://youtu.be/971eZhMHQQw

ssh fd@pwnable.kr -p2222 (pw:guest)

* Analysis

The source code has a call to read which reads input from fd and
stores the result to buf. If buf (char [32]) matches "LETMEWIN", it
prints the flag. fd is calculated from the following formula:

int fd = atoi( argv[1] ) - 0x1234;

If argv[1] is 0x1234 (4660) then fd is 0 and read will get its input
from stdin (keyboard).

* Solution

echo "LETMEWIN" | ./fd 4660

* Flag

mommy! I think I know what a file descriptor is!!
