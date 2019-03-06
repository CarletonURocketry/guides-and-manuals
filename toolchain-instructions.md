## Installing openocd

In order to interface with the boards, you need to download and install
openocd. The version of openocd you install must have been compiled with
cmsis-dap support.

Debian/Debian-based Linux Instructions:
    `sudo apt install openocd`

MacOS Instructions:
    Install from MacPorts:
    `sudo port install openocd`

Windows Instructions:
    Spin up a Linux virtual machine and pass the usb devices through to it.

Run `openocd` in a directory with the `openocd.cfg` file.
  >>> If you get the error: "Error: unable to open CMSIS-DAP device", run openocd
  with root privileges.

## Compilation and Flashing

In order to compile the source code for the SAMD21 chip, a custom ARM toolchain
is required. This can be downloaded from Microchip's website (this is an
open-source toolchain consisting of various patches to a particular version of
the gcc compiler)
[here](https://www.microchip.com/mplab/avr-support/avr-and-arm-toolchains-c-compilers).
Download the version for your operating system under the heading: "ARM GNU
Toolchain (32-bit)". Extract this to a desired location.

Openocd must be running in the background for the following commands to work.

Run `make build CORTEX_TOOLCHAIN_BIN=<location_of_compiler_binary>` to build
the `.elf` file. For example: `make build -j12
CORTEX_TOOLCHAIN_BIN=~/Downloads/arm-none-eabi/bin`

Note: You can parallelize the build process by using the flag
`-j<number_of_threads>`.

Run `make upload CORTEX_TOOLCHAIN_BIN=~/Downloads/arm-none-eabi/bin` to flash
the Xplained board.
