# eLITe-Board Documentation

## Prerequisites for own Builds
* Toolchain:

## Initial Flashing and Testing
* If you want to avoid installing a toolchain, you can use the "bmflash" tool from its [repository](https://github.com/rf-eng/Black-Magic-Probe-Book). Prebuilt binaries are available for Linux and Windows under the [Releases section](https://github.com/rf-eng/Black-Magic-Probe-Book/releases/latest)
* First you need to use an external Black Magic Probe adapter to flash the onboard debugger of the eLITe-board.
    1) Download the firmware files (two *.elf files) for the onboard debugger from [here](https://github.com/rf-eng/blackmagic/releases/latest)
    2) Connect your external Black Magic Probe to your computer and to the tag connector on the eLITe-board
    3) Enable the power supply of the eLITe-board over the USB-C connector
    4) Start the bmflash tool and make sure that it recognizes the Black Magic Probe
    5) Flash both firmware files to the eLITe-board

## Tools for development in C

## Tools for development in Micropython
