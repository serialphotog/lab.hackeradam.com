---
title: "Building Serenity OS on Windows with WSL 2"
description: "Some of the trials and tribulations I've encountered building Serenity OS under Windows with WSL 2."
date: '2021-04-18'
---

I've been following the development of [Serenity OS](http://www.serenityos.org/) with a fair bit of interest for the past few months. As such, I've been itching to try building the system and playing around with it. I finally got a bit of time and decided that, for a bit of an extra challenge, I'd attempt to get it running through WSL 2 on Windows 10.

This is something that is actually [officially documented](https://github.com/SerenityOS/serenity/blob/master/Documentation/BuildInstructions.md) in the GitHub repository, but I ran into a few roadblocks along the way. I should also mention that I am doing this on an Ubuntu 20.10 install in WSL. 

# Missing Libraries

One of the first steps you need to take to build Serenity is to setup the build toolchain. Conveniently enough, there is a script, *BuildIt.sh* that takes care of this for you. During the step that build GCC, however, I ran into problems with missing libraries. I resolved each of these as follows:

### libgmp

```bash
wget https://gcc.gnu.org/pub/gcc/infrastructure/gmp-6.1.0.tar.bz2
tar -xf gmp-6.1.0.tar.bz2 && cd gmp-6.1.0
./configure
make -j5
make check
sudo make install 
```

### libmpc

```bash
wget https://gcc.gnu.org/pub/gcc/infrastructure/mpc-1.0.3.tar.gz
tar -xf mpc-1.0.3.tar.gz && cd mpc-1.0.3
./configure
make -j5
sudo make install
```

### libmpfr

```bash
wget https://gcc.gnu.org/pub/gcc/infrastructure/mpfr-3.1.4.tar.bz2
tar -xf mpfr-3.1.4.tar.bz2 && cd mpfr-3.1.4
./configure
make -j5
sudo make install
```

# Failure to Link libmpfr.so.4

Installing the above libraries got me a lot further into the build process than before, but the process still failed when it tried to link libmpfr.so.4. Looking at `/usr/lib/x86_64-linux-gnu` I could see that I had libmpfr.so.6. I resolved the linking issue simply by doing the following:

```bash
sudo ln -s /usr/lib/x86_64-linux-gnu/libmpfr.so.6 /usr/lib/x86_64-linux-gnu/libmpfr.so.4
```

# qemu-img Not Found

This one was simple to fix:

```bash
sudo apt install qemu-utils
```

# Sort of Running

At this point Serenity will sort of run, but without actually displaying anything in QEMU:

![Serenity seemingly booting](/blog/SerenityBoot.png)

At this point I am uncertain of the issue and will have to wait until I can investigate further...