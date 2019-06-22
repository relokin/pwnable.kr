* Challenge

Mommy told me to make a passcode based login system.
My initial C code was compiled without any error!
Well, there was some compiler warning, but who cares about that?

ssh passcode@pwnable.kr -p2222 (pw:guest)

* Analysis

To get the flag the login function uses system to cat the flag to
stdout. However, to execute the code there are two local variables
passcode1 and passcode2 that have to be:

passcode1==338150
passcode2==13371337

Prior to checking the values of the two passcodes two calls to scanf
attempt to read from the user two integers. These two integers should
have been passcode1 and passcode2 but due to a missing & they now
store the users input to *passcode1 and *passcode2.

Before calling login() the code calls another function welcome which
reads 100 bytes from the user and stores them in the local variable
name. What is interesting is that the calls to welcome() and login()
are very close and the stack when login() is called, it uses the same
memory for its stack.

Part of the stack layout for the two functions looks like this:

<welcome>

----------
ret addr
---------- <- ebp
old ebp
---------- <- -0x4
stack guard
---------- <- -0x10
name
---------- <- -0x74

<login>

----------
ret addr
---------- <- ebp
old ebp
---------- <- -0x4
passcode2
---------- <- -0x10
passcode1
---------- <- -0x14

As a result the last 4 bytes of name will be the value of passcode1
when login() is called. This allows us to write to arbitrary location
to the memory as we can initialize passcode1 with any address and then
the first scanf will write to *passcode1. The code exploit uses this
vulnerability and write to the PLT - the PLT is used to resolve
dynamic symbols such as fflush. fflush() is called right after the
first call to scanf and therefore if we patch its PLT entry to jump to
desired code, we redirect the execution flow and get the flag.

To get the flag, copy exploit to /tmp/foo and then run:
$> cd ~
$> /tmp/foo/exploit.py passcode

* Flag

Sorry mom.. I got confused about scanf usage :(
