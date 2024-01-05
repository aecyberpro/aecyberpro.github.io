---
published: true
---
This post discusses how a compiled C program (Elf) on Linux resolves the location in memory for shared libraries and includes a walkthrough of the Procedure Linkage Table (PLT) and Global Offset Table (GOT).

## Procedure Linkage Table (PLT)

The Procedure Linkage Table (PLT)  is a read-only section responsible for calling the dynamic linker during and after program runtime to resolve addresses of requested functions. 

## Global Offset Table

At runtime, the dynamic linker populates the Global Offset Table (GOT). The dynamic linker obtains the absolute addresses of requested functions for locations from the PLT. This is not always handled during compile time. 

## Lazy binding and the PLT

Symbol resolution for many functions is performed when the first call to the requested function is made. (Lazy Linking) You can force the linker to perform all relocations immediately at runtime for performance reasons by setting the environmental variable “LD_BIND_NOW”.

ELF binaries contain a seperate GOT section named ".got.plt". This section is similar to the regular .got section. The difference is the .got.plt is runtime-writable but .got is not if RELRO is enabled. RELRO is relocations read-only. To enable it, use the ld option `-z relro`. RELRO places GOT entries that must be runtime-writable for lazy binding in `.got.plt` and all others in the read-only `.got` section. [1]

If you would like to read more about the PLT, GOT, and security features, see page 45 of "Practical Binary Analysis". [1]

## High level overview of symbol resolution

1. The program makes a call to printf()
2. The request is made to the PLT which refers to the GOT entry for the requested symbol.
3. If this is the first request for the function, control is passed by the GOT back to the PLT. This is done by first pushing the address for the relative entry within the relocation table, which is used by the dynamic linker for symbol resolution. The PLT then calls the dynamic linker to resolve the symbol Upon successful resolution, the address of the requested function is placed in the GOT entry.
4. If the GOT holds the address of the requested function, control can be passed immediately without the involvement of the dynamic linker.

## Commands:

### Objdump

`objdump -d`  disassembles an object file

`objdump -h`  displays section headers

`objdump -j <section name>`  allows you to specify a section, example: `objdump -d .text -d 
./program_name`

`objdump -R ./program_name`  Lists the Global Offset Table (GOT) for each function.

### Readelf

`readelf -S [program name]` Display the sections' header

`readelf -x [section name/number] [program name]` Hex dump a section name, example: `readelf -x [section to display] [program name]`

## Example: walking through dynamic relocation

```
(gdb) disas main
Dump of assembler code for function main:
<snip>
   0x080484a4 <+83>:	call   0x8048374 <puts@plt>
<snip>
```

```
$ objdump -d -j .text memtest | grep puts
 80484a4:	e8 cb fe ff ff       	call   8048374 <puts@plt>
```

Since we haven't called the puts function at this point, it has not been resolved.

Now we will use objdump to dump the GOT:

```
$ objdump -R [program name]

program_name:     file format elf32-i386

DYNAMIC RELOCATION RECORDS
OFFSET   TYPE              VALUE 
080497cc R_386_GLOB_DAT    __gmon_start__
080497dc R_386_JUMP_SLOT   getpid@GLIBC_2.0
080497e0 R_386_JUMP_SLOT   __gmon_start__
080497e4 R_386_JUMP_SLOT   __libc_start_main@GLIBC_2.0
080497e8 R_386_JUMP_SLOT   scanf@GLIBC_2.0
080497ec R_386_JUMP_SLOT   printf@GLIBC_2.0
080497f0 R_386_JUMP_SLOT   puts@GLIBC_2.0

```

We notice above that address 0x8048374 is no longer shown. It shows address 0x080497f0 for puts function. Lets examine three instructions from that address in gdb:

```
(gdb) x/3i 0x8048374
   0x8048374 <puts@plt>:	jmp    *0x80497f0
   0x804837a <puts@plt+6>:	push   $0x28
   0x804837f <puts@plt+11>:	jmp    0x8048314
```

Notice above the jmp to address 0x80497f0.

Let's examine in objdump:

```
$ objdump -j .got.plt -d [program name] | grep 80497f0
 80497f0:	7a 83 04 08  
 ```
 
 The "7a 83 04 08" is in little-endian format, so we reverse it to get address 0x804837a.
 
 We see that address above, which shows that by default, the GOT entry points back to the PLT. Now the dynamic linker will be called so that it can write the real address of the function into it's GOT location.
 
Open memtest again in gdb, enter run to run it, enter any number and press enter, then enter Ctrl-C. Now enter the following in GDB:

```
(gdb) x/x 0x80497f0
0x80497f0 <puts@got.plt>:	0xf7e453d0
```

Now we have the real address of the puts function:

```
(gdb) x/3i 0xf7e453d0
   0xf7e453d0 <puts>:	push   ebp
   0xf7e453d1 <puts+1>:	mov    ebp,esp
   0xf7e453d3 <puts+3>:	push   edi
```

## References

[1] Andriesse, Dennis. Practical Binary Analysis. San Francisco. No Starch Press, 2019
