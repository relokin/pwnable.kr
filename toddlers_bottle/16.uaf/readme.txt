* Challenge

Mommy, what is Use After Free bug?

ssh uaf@pwnable.kr -p2222 (pw:guest)

* Analysis

This challenge requires a bit of knowledge about the heap and how
dynamic memory management works. For details on that, there are many
excellent resources online. I highly recommend the following post
although understanding all of it is not necessary for this task:

https://0x00sec.org/t/heap-exploitation-abusing-use-after-free/3580

The code allocates in the heap two objects m (Man) and w (Woman), each
occupying 24 bytes of memory. They are dynamically allocated and as a
result they are placed in the heap. The program then gives 3 options
to the user:

1) invoke the introduce() method of m (Man) and w (Woman).
2) allocate a buffer (char *) of a specified length and copy to it
data from a file or
3) free these two objects.

An additional method in Human, the base class of Man and Woman will
give the user shell that can be used to read the flag.

The Man::introduce() and Woman::introduce() methods are virtual and
when invoked a lookup in the vtable points to the right version (base
class or sub class version) of introduce(). One obvious way to solve
this challenge is to manipulate the m or w vtable such that when we
call m->introduce(), we invoke give_shell() instead.

Initially the vtable of Man looks like this:

 00401570 Human::give_shell
 00401578 Man::introduce

And the assemble code to call m->introduce() is the following:

  00400fcd 48 8b 45 c8     MOV        RAX,qword ptr [RBP + local_40]
  00400fd1 48 8b 00        MOV        RAX,qword ptr [RAX]
  00400fd4 48 83 c0 08     ADD        RAX,0x8
  00400fd8 48 8b 10        MOV        RDX,qword ptr [RAX]
  00400fdb 48 8b 45 c8     MOV        RAX,qword ptr [RBP + local_40]
  00400fdf 48 89 c7        MOV        RDI,RAX
  00400fe2 ff d2           CALL       RDX

The first 4 lines load the vptr of m to rax then add 0x8 to get the
second entry in the vtable and load the address of M::introduce() to
rdx. The next 2 lines load the address of m to rax. The call
instruction will invoke the introduce function (rdx) with m as its
argument (rax).

One way to change this, to call the give_shell() method would be to
manipulate the vtable to point to 0x401568 instead of 0x401570 and as
a result the second entry of the vtable would now point to
give_shell(). This means that we could read the flag if manipulated
the vptr of m (or w).

If you know the basics of how dynamic memory management works, you'll
know that glibs malloc uses the first fit policy. This means that if
we free an object and then allocate memory for another object of the
same size (or smaller if it maps in the same bin) malloc will allocate
and return the same memory space. In this case this means that if we
free m and w, and then allocate an object of same size, malloc will
return and reuse the space of w, if we call malloc again it will reuse
the space of m.

This means that we can get a pointer to m. Once we have that we can
copy to the data we read from an input file. The memory layout of Man
is very simple and looks like this:

Man
------------
void *vptr;
int age;
string name;

As a result, all we need to do is make sure that we copy the contains
of a file to the memory pointed by m, where the first 64 bits of the
file contain the address of vtable of Man - 0x8 (0x401568).

This is what exploit.py does. To get the flag simply run:

$> /tmp/foo/exploit.py uaf

* Flag

yay_f1ag_aft3r_pwning
