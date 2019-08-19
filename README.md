# npiet-GMP
Fork of Erik Schoenfelder's piet implementation, adapted to use arbitrary-precision numbers through GMP.

If you don't know what Piet is, its specification is [here](http://www.dangermouse.net/esoteric/piet.html), some examples are [here](http://www.dangermouse.net/esoteric/piet/samples.html), and the original npiet is [here](https://www.bertnase.de/npiet/).

The specification said that the maximum integer size was "notionally infinite", but implementations had the option to limit the size. In practice it seemed that all implementations exercised that option (for example npiet simply used C's `long` type.)

This was kind of getting in the way of my idea to make a fancy painting of a six-sided die that also implement a large-state high period Linear Congruential Generator. I guess within the constraints of the existing versions I could have done like a crappy 16-bit one or something, but that seemed lame :p

Now it's possible, and hopefully it will be finished and posted here sometime soon.

Several of the examples listed in the above link have already successfully run under this version, including under conditions that would have had overflow errors in the original (such as the power function that looks like a bus, and the factorial example)

## Building

You'll need the Gnu Multi-Precision library (GMP) installed along with its headers. The `-lgmp` compiler/linker flag is mandatory. 

Because it's organized under a "Single Compilation Unit" model (everything is crammed into one source file), it's recommended to use the `-fwhole-program` flag to enhance optimization and produce a smaller, more efficient executable.

Recommended build command:

```
gcc -O3 -march=native -fwhole-program npiet-1.3e-gmp.c -lgmp
```

## Troubleshooting

Sometimes if you have large and irregular blocks of color, deep recursion can occur, which has the potential to overflow the maximum stack size, which crashes the program with a segmentation fault. This was also the case on original npiet. If you have enough memory, you can usually fix this by removing the stack size limit. For example, on linux you would run

`ulimit -s unlimited`

in the shell you use to run the program, before running it. Note that you may need to invoke administrative privilege and edit the settings in `/etc/security/limits.conf` before this is allowed. If you change those settings, be aware they may not take effect until you log out and back in (or at least run `su -l <your-username>` to get a "refreshed" shell).

There is also another possible "segmentation fault" crash that sometimes occurs with long programs of >1000 steps. It seems to have surfaced when I added GMP support, though I can't figure out why. Valgrind ruled out most of the common kinds of memory errors, yet the stacktrace said the crash occurred in the `realloc()` function. It may have just been an obscure bug in my copy of GLIBC 2.28, I don't know. In any case, Jemalloc is optionally supported as an alternative memory allocator, and if you get a weird crash it may fix it to build with Jemalloc support. It did fix it for me, though again I'm not really sure why 🤷

Install jemalloc and its headers, and build with:

```
gcc -DHAVE_JEMALLOC_H -O3 -march=native -fwhole-program npiet-1.3e-gmp.c -lgmp -ljemalloc
```
