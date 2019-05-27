* Challenge

Mommy! what is PATH environment in Linux?

ssh cmd1@pwnable.kr -p2222 (pw:guest)

* Analysis

The cmd1 binary has the guid bit which means that it has the right
permissions to read the flag. From the source code it seems that it
will execute any command that a user provides through argv[1] unless
it has the strings "flag", "sh" or "tmp".

To read the flag we create a program exploit.c which simply reads the
file /home/cmd1/flag and prints its contents. Due to permission
restrictions this binary can only be in a directory in /tmp say /tmp/foo.

To get the flag we then simple execute:

$> ~/cmd1 ./exploit

* Flag

mommy now I get what PATH environment is for :)
