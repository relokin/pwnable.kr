* Challenge

Mom? how can I pass my input to a computer program?

ssh input2@pwnable.kr -p2222 (pw:guest)

* Analysis

Nothing really tricky, need to provide the right inputs in different
ways.

The only problem is that you can't write to the home directory so you
need to copy the binary to /tmp/ and create links to the binary and
the flag.

$> mkdir /tmp/niknik
$> cd /tmp/niknik
$> ln -s ~/flag flag
$> ls -s ~/input input
$> ./exploit

* Flag
Mommy! I learned how to pass various input in Linux :)
