# meta-dimitriOS

A BitBake layer for my Linux based projects.

## What

This is a Bitbake layer that includes recipes for my different Linux-based projects.
It is tested to work with Yocto `zeus` but may work with other versions as well.

## Why

Whenever I am work on a hobby project that involves Linux, I used to follow the
[golden image](https://www.linuxjournal.com/content/linux-iot-development-adjusting-binary-os-yocto-project-workflow)
approach, which involves getting the latest prebuilt image at the time (e.g. `Raspbian`),
installing the various dependencies and once everything is right, I
[document](https://github.com/platisd/cryptopuck#how-to-set-up) them as accurately as possible.

Unfortunately, this approach has several issues, including but not limited to:
1. A step may be omitted from the documentation
2. Dependencies and operating systems change
3. It takes time for persons to precisely reproduce each step

To tackle these problems, [Yocto Project](https://www.yoctoproject.org/) is used to build
a Linux distribution that includes, in a standardized and repeatable manner, the necessary
software.
Additionally, `dimitriOS` is a way to experiment with creating configurable platforms for
embedded Linux systems.

## How

To create a `dimitriOS` image you need to decide:
1. The hardware you are going to run your system on
2. The software image you want to use

### Supported hardware

If there is a Yocto Board Support Package (BSP) for the board you are interested in, then
you should be able to run `dimitriOS` images on it.

Currently, the supported boards are:

| Board                         | `MACHINE`           |
| ----------------------------- | ------------------- |
| Raspberry Pi                  | `raspberrypi`       |
| Raspberry Pi Zero             | `raspberrypi0`      |
| Raspberry Pi Zero Wifi        | `raspberrypi0-wifi` |
| Raspberry Pi 2                | `raspberrypi2`      |
| Raspberry Pi 3                | `raspberrypi3`      |
| Raspberry Pi 4                | `raspberrypi4`      |
| Raspberry Pi Compute Module   | `raspberrypi-cm`    |
| Raspberry Pi 3 Compute Module | `raspberrypi-cm3`   |


### Available images

To support different use cases for our Linux distribution, we have different images. Unlike the
software running on your laptop or mobile device at the moment, this one has a very specific
purpose and does not come with additional software.

Currently, there are images for the following projects:
| Project        | Recipe             |
| -------------- | ------------------ |
| [Cryptopuck][] | `cryptopuck-image` |
| [VasttraPi][]  | `vasttrapi-image`  |

### How-to: Build a `dimitriOS` image

1. Get the repo tool to fetch the sources
   1. `mkdir ~/bin`
   2. `PATH=~/bin:$PATH`
   3. `curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo`
   4. `chmod a+x ~/bin/repo`
2. Create a `dimitriOS` directory for the build
   1. `mkdir dimitriOS && cd dimitriOS`
3. Choose the `dimitriOS` manifest that includes the BSP for your board and load
the respective board configuration. E.g. For Raspberry Pi boards:
   1. `repo init -u git@github.com:platisd/dimitriOS-manifest.git -m dimitriOS-rpi.xml`
   2. `export TEMPLATECONF=/home/dimitris/dimitriOS/layers/meta-dimitriOS/rpi-conf/`
4. Source the build environment
   1. `. layers/poky/oe-init-build-env build`
5. Select the particular board you are targetting. E.g. For Raspberry Pi 4:
   1. `export MACHINE=raspberrypi4`
6. Build the image for the particular board. E.g. For [Cryptopuck][]:
   1. `bitbake cryptopuck-image`

To rebuild an image using a new shell session, repeat the last three steps.

[Cryptopuck]: https://github.com/platisd/cryptopuck
[VasttraPi]: https://github.com/platisd/vasttraPi
