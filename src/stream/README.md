---
title: STREAM tests with Ubuntu 24.04
tags:
    - x86
    - fullsystem
shortdoc: >
    Resources to build a x86-ubuntu 24.04 disk image with shared GAPBs benchmark suite
    This resource is based on ubuntu generic by Harshil Patel and GAPBS disk image by Marjan Fariborz
authors: ["kg"]
---

This document provides instructions to create a shared version of GAP Benchmark Suite (GAPBS) disk image, which, along with an example script, may be used to run GAPBS within gem5 simulations. The example script uses a pre-built disk-image. It is based on Ubuntu 24.04 disk image.

These images have their .bashrc files modified to execute a script passed from the gem5 configuration files (using the m5 readfile instruction).
The user needs to specify what it needs to execute via the python script.

The benchmarks needs to be run as root as the HOOKS open /dev/mem file.

## What's on the disk?

- shared GAPBS (https://github.com/darchr/shared-gapbs)
- username: gem5
- password: 12345

- The `gem5-bridge`(m5) utility is installed in `/usr/local/bin/gem5-bridge`.
- `libm5` is installed in `/usr/local/lib/`.
- The headers for `libm5` are installed in `/usr/local/include/`.

Thus, you should be able to build packages on the disk and easily link to the gem5-bridge library.

Network is enabled on this disk image.

If you want to disable networking, you need to modify the disk image and move the file `/etc/netplan/00-installer-config.yaml` or `/etc/netplan/50-cloud-init.yaml` to `/etc/netplan/00-installer-config.yaml.bak` or `/etc/netplan/50-cloud-init.yaml.bak` depending on which config file the disk image contains. The image should have ``/etc/netplan/50-cloud-init.yaml`.
For example you can use the following commands to re-enable network:

```sh
# reverse example
sudo mv /etc/netplan/50-cloud-init.yaml.bak /etc/netplan/50-cloud-init.yaml
sudo netplan apply
```

### Installed packages

- `build-essential`
- `git`
- `scons`
- `vim`
- `cmake`
- `make`
- `g++`
- `gcc`
- `libboost-all-dev`
- `ndctl`

Rest of the structure of this disk image is the same as generic-ubuntu-disk-images.

## STREAM

The artifacts are compiled in the `simple-vectorizable-microbenchmarks` directory.

### Kernel

Please use kernel version 6.9.9 (src/linux-kernel) with this disk image.
