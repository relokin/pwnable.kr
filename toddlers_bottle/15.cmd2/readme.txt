* Challenge

Daddy bought me a system command shell.
but he put some filters to prevent me from playing with it without his permission...
but I wanna play anytime I want!

ssh cmd2@pwnable.kr -p2222 (pw:flag of cmd1)

* Analysis

The binary in this task will take a string as an input and execute it
as long sa it doesn't contain the substrings "=", "PATH", "export",
"/", "`", "flag". Also the code will set the PATH environment variable
to a non existent directory. There are more than ways to get the
flag. One simple way is to execute a command that reads from stdin to
a variable and then executes the contents of the variable as a
command.

To get the flag simply run:

$> echo "/bin/cat ./flag" | ./cmd2 "read icmd && exec \${icmd}"

* Flag

FuN_w1th_5h3ll_v4riabl3s_haha
