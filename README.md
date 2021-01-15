# eLITe-Board Documentation

## Prerequisites for own Builds
* Toolchain:

## Initial Flashing and Testing
* If you want to avoid installing a toolchain, you can use the "bmflash" tool. Prebuilt binaries are available for Linux and Windows under the [releases section](https://github.com/rf-eng/Black-Magic-Probe-Book/releases/latest) of its repository.
### Flash the onboard debugger unit (*internal* Black Magic Probe)
* First, you need to flash the onboard debugger of the eLITe-board using an *external* Black Magic Probe adapter.
    1) Download the firmware files (two *.elf files) for the onboard debugger from [here](https://github.com/rf-eng/blackmagic/releases/latest)
    2) Connect your external Black Magic Probe to your computer and to the tag connector on the eLITe-board
    3) Enable the power supply of the eLITe-board over the USB-C connector
    4) Start the bmflash tool and make sure that it recognizes the external Black Magic Probe
    5) Flash both firmware files to the eLITe-board
* Verify that the internal debugger is working
    1) Disconnect the external Black Magic Probe adapter
    2) Power cycle the eLITe-board. (Re-)connecting it to the computer should show a virtual serial port. Also the "bmflash" tool should recognize the *internal* Black Magic Probe which is now available on the eLITe-board
    
### Flash MicroPython and run self tests
* Flash a current MicroPython firmware using the internal Black Magic Probe
    1) Make sure that the internal debugger is working (see [here](https://github.com/rf-eng/eliteboard_docs#flash-the-onboard-debugger-unit-internal-black-magic-probe))
    2) Download a MicroPython release (firmware_eliteboard.elf) from [here](https://github.com/rf-eng/micropython/releases/latest)
    3) Flash firmware_eliteboard.elf to the eLITe-board using the bmflash tool (see the [releases section](https://github.com/rf-eng/Black-Magic-Probe-Book/releases/latest) of the corresponding repository)
* Run self tests
    1) A self test script is available [here](https://github.com/rf-eng/eliteboard_upyscripts/blob/master/self_test.py)

## Tools for development in C

## Tools for development in MicroPython
The [Thonny IDE](https://thonny.org/) offers features like a file explorer or an integrated plotter, which are very useful when working with MicroPython devices. To use Thonny with MicroPython on the eLITe-board the following settings need to be made:
1. Go to Run > Select Interpreter and select the Interpreter tab.
2. Choose MicroPython (generic)
3. Select the correct port. This can be tricky, because the eLITe-board appears as multiple devices. Simple trial and error for all available options is the fastest method to find the correct port right now (in non-Windows versions a device name will appear for the different ports which simplifies things a lot).
4. If you found the correct port you should see the MicroPython prompt on the Shell tab (MicroPython + version string and some additional hardware information)
5. You can now enter commands into the Shell tab, or you can run Python files on the board. If your project consists of multiple files, make sure that modules etc. are available on the eLITe-board memory (either flash or SD-card). Local files on your PC cannot be found by the MicroPython running on the eLITe-board.

## Tips for MicroPython
The board appears as mass storage device and this function can be used to upload *.py files to the board.  
It might be required to perform a soft-reset (press CTRL-D) before the files become visible. A list of files can be printed by
```
import os
os.listdir()
```
Execute a file by ```import filename``` (without the py-extension)
