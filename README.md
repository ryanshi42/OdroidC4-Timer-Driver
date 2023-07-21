<!--
   Copyright 2023, UNSW
   SPDX-License-Identifier: CC-BY-SA-4.0
-->

This is a simple system to demonstrate a passieve timer driver and a client on top of the [seL4 Core
Platform](https://github.com/BreakawayConsulting/sel4cp) on an odroidC2. The client application
requests a timeout every second. The API to set a timeout or get the current time can be found
in `timer.h`.

## Build

Note that while any ARM GCC toolchain should work, all testing and
so far has been done with the ARM GCC toolchain version 10.2-2020.11.

If you wish to use the default toolchain you can download it from here:
https://developer.arm.com/-/media/Files/downloads/gnu-a/10.2-2020.11/binrel/gcc-arm-10.2-2020.11-x86_64-aarch64-none-elf.tar.xz?revision=79f65c42-1a1b-43f2-acb7-a795c8427085&hash=61BBFB526E785D234C5D8718D9BA8E61.

Otherwise, you can change the Makefile to accept another toolchain or pass the prefix
to the Makefile using the argument `TOOLCHAIN=<PREFIX>`.

```
    $ make BUILD_DIR=<path/to/build> \
        SEL4CP_SDK=<path/to/core/platform/sdk> \
        SEL4CP_BOARD=odroidc2 SEL4CP_CONFIG=(debug)
```

## Limitations

The timer driver is currently limited to a single timeout per client, with a maximum of 6 clients. 
This can easily be reconfigured to support more clients (see `MAX_TIMEOUTS` in `timer.c`).
The timer is not the most efficient driver, as it searches through an array to find the next timeout (if any). 

## To do
- Use an ordered linked list instead of an array to store timeout requests. This would require a memory pool to
allocate the structs from, but would remove the limitation of a single timeout per client as well as the O(n)
search on receiving an IRQ. 
