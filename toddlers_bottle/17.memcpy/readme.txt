* Challenge

Are you tired of hacking?, take some rest here.
Just help me out with my small experiment regarding memcpy performance.
after that, flag is yours.

http://pwnable.kr/bin/memcpy.c

ssh memcpy@pwnable.kr -p2222 (pw:guest)

* Analysis

After connecting to the daemon, the user provides a set of sizes for
buffer which will be used to test the implementation of memcpy_slow()
and memcpy_fast(). The sizes have to be between i and i*2 where i
starts from 8 and goes up to 4096. The flag will be printed only if
the experiment completes.

By providing the lowest allowed value for every iteration, the daemon
dies half way through the experiment. After compiling the binary (in
the server) and running it, it seems that one of the memory
instructions segfaults. gdb shows that the faulting instruction is
movntps, a SIMD instruction, which is in memcpu_fast() and is used to
move a register value to the destination array.

The source array remains the same across all iterations but a new
destination array is allocated on every iteration. A quick search on
movntps reveals that the instruction requires its memory operand to be
aligned on a 16-byte boundary, something which won't happen if the
provided sizes are not carefully crafted.

Providing always aligned destination buffers is quite easy. We just
make sure that the size is within the allowed range and, at the same
time, that the already allocated space always finishes at a 16-byte
aligned boundary. In that we have to take into account that apart from
the requested size malloc requires an extra 8 bytes. With these
constraints in mind we can now provide sizes that memcpy_fast() can
handle and as a result get to the end of the execution.

To get the flag simple run:

$> ./exploit.py

* Flag

1_w4nn4_br34K_th3_m3m0ry_4lignm3nt
