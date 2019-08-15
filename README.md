# npiet-GMP
Fork of Erik Schoenfelder's piet implementation, adapted to use arbitrary-precision numbers through GMP.

If you don't know what Piet is, its specification is [here](http://www.dangermouse.net/esoteric/piet.html), some examples are [here](http://www.dangermouse.net/esoteric/piet/samples.html), and the original npiet is [here](https://www.bertnase.de/npiet/).

The specification said that the maximum integer size was "notionally infinite", but implementations had the option to limit the size. In practice it seemed that all implementations exercised that option (for example npiet simply used C's `long` type.)

This was kind of getting in the way of my idea to make a fancy painting of a six-sided die that also implement a large-state high period Linear Congruential Generator. I guess within the constraints of the existing versions I could have done like a crappy 16-bit one or something, but that seemed lame :p

Now it's possible, and it will be posted here when it's finished.

Several of the examples listed in the above link have already successfully run under this version, including under conditions that would have had overflow errors in the original (such as the power function that looks like a bus, and the factorial example)

##Building

You'll need the Gnu Multi-Precision library (GMP) installed along with its headers. The `-lgmp` compiler/linker flag is mandatory. 

Because it's organized under a "Single Compilation Unit" model (everything is crammed into one source file), it's recommended to use the `-fwhole-program` flag to enhance optimization and produce a smaller, more efficient executable.

Recommended build command:

```
gcc -O3 -march=native -fwhole-program npiet-1.3e.c -lgmp
```
