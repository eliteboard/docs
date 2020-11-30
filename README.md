# eLITe-Board Documentation

## Prerequisites for own Builds
* Toolchain:

## Initial Flashing and Testing
* If you want to avoid installing a toolchain, you can use the "bmflash" tool. Prebuilt binaries are available for Linux and Windows under the [releases section](https://github.com/rf-eng/Black-Magic-Probe-Book/releases/latest) of its repository.
* First, you need to flash the onboard debugger of the eLITe-board using an external Black Magic Probe adapter.
    1) Download the firmware files (two *.elf files) for the onboard debugger from [here](https://github.com/rf-eng/blackmagic/releases/latest)
    2) Connect your external Black Magic Probe to your computer and to the tag connector on the eLITe-board
    3) Enable the power supply of the eLITe-board over the USB-C connector
    4) Start the bmflash tool and make sure that it recognizes the Black Magic Probe
    5) Flash both firmware files to the eLITe-board
* Verify that the debugger is working
    1) Disconnect the external Black Magic Probe adapter
    2) Power cycle the eLITe-board. (Re-)connecting it to the computer should show a virtual serial port. Also the "bmflash" tool should recognize the internal Black Magic Probe which is now available on the eLITe-board

## Tools for development in C

## Tools for development in Micropython
