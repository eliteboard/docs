# eLITe-Board Software Documentation

## Tools for Development in C
It is recommended that you install
* A toolchain
* Build tools like ```make```
* An IDE like VSCodium (or VSCode). Theia could be an option later

### Manual installation of the development environment
Development is done using Visual Studio Code
* Install VSCodium or VSCode
* Then you have to install the following extensions:
    * Cortex-Debug
        * https://open-vsx.org/extension/marus25/cortex-debug
        * https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug
        * https://github.com/Marus/cortex-debug
        * https://marcelball.ca/projects/cortex-debug/
    * C/C++ (could be already built in)
        * https://open-vsx.org/extension/vscode/cpp
        * https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools
        * https://github.com/Microsoft/vscode-cpptools

* You need the ```make``` command to process the Makefile(s):
    * Windows: install MinGW https://sourceforge.net/projects/mingw-w64/
    * MacOS: Install Xcode Command Line Tools (CLT) using ```xcode-select --install```
    * Linux: It is very likely that the ```make``` command is already included in your distribution

* You need a suitable GCC Compiler
    * Windows versions can be found here:
        * directly from arm (bug when creating bin/hex in windows under version 2018q4, https://bugs.launchpad.net/gcc-arm-embedded/+bug/1810274):
          - https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads
        * alternative build probably without the bug: https://github.com/gnu-mcu-eclipse/arm-none-eabi-gcc/releases/tag/v8.2.1-1.2
    * MacOS: Install Homebrew (follow instructions available on brew.sh). Then run ```brew install homebrew/cask/gcc-arm-embedded```
    * Linux: Also see https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads

* You need to define an environmental variable ```ELITEBOARDDIR``` which points to your gcc installation
    * Check your OS documentation for instructions

* Further comments:
The MacOS descriptions are taken from: https://github.com/glegrain/STM32-with-macOS#0---installing-the-toolchain

To use COM port numbers above 9 in Windows you have to use the syntax 
```\\.\COM10*/``` otherwise you can simply use "COMx" in the ```"BMPGDBSerialPort"```
setting within the ```.vscode/launch.json``` file.

## Tools for Development in MicroPython
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

# eLITe-Board Hardware Documentation

## Pinout
The pinout of the board looks like this:
![Pinout of eLITe-Board](/images/pinout.svg "Pinout of eLITe-Board")

Note that these pin descriptions are only valid if you use the pin configuration as it is also used in the [template project](https://github.com/eliteboard/baremetal_c/tree/master/Template) and NOT if you use the default configuration of CubeMX. The available ```template.ioc``` file should be used as a starting point when you want to create a new project from scratch.

## I2C Addresses of Onboard Peripherals
Note, that these are 8-bit addresses (including the R/W bit). Shift right by one bit if you need the 7-bit address.

| Designator | Manufacturer Identifier | Description                 | Read Address | Write Address | Read Address | Write Address |
| -------------- | --------------------------- | ------------------------------- | ---------------- | ----------------- | ---------------- | ----------------- |
| IC19           | M24512-DFMC6TG              | I2C EEPROM                      | 0xA1             | 0xA0              | 0xA3             | 0xA2              |
| IC8            | DS2482S-100+T&R             | I2C One Wire Bridge             | 0x31             | 0x30              | \-               | \-                |
| IC2            | LT3582EUD#PBF               | Programmable DC/DC Converter    | 0x63             | 0x62              | \-               | \-                |
| IC23           | TCA9534APWR                 | Port Expander ADC2              | 0x41             | 0x40              | \-               | \-                |
| IC6            | WM8731CLSEFLR               | Audio Codec                     | 0x35             | 0x34              | \-               | \-                |
| IC14           | LSM6DSOXTR                  | 3D Accelerometer + 3D Gyroscope | 0xD7             | 0xD6              | \-               | \-                |
| IC10           | HTS221TR                    | Relative Humidity + Temperature | 0xBF             | 0xBE              | \-               | \-                |
| IC12           | LIS3MDLTR                   | 3D Magnetometer                 | 0x3D             | 0x3C              | \-               | \-                |
| IC13           | LPS22HHTR                   | Barometer                       | 0xBB             | 0xBA              | \-               | \-                |
| IC11           | ISL28023FR60Z-T7A           | Digital Power Monitor           | 0x8B             | 0x8A              | \-               | \-                |
| IC20           | STUSB4500LQTR               | USB-C Controller                | 0x51             | 0x50              | \-               | \-                |

# Flashing Guides
## Initial Flashing and Testing
* If you want to avoid installing a toolchain, you can use the "bmflash" tool. Prebuilt binaries are available for Linux and Windows under the [releases section](https://github.com/rf-eng/Black-Magic-Probe-Book/releases/latest) of its repository.
    
### Flash MicroPython for the eLITe-Board and run self tests
* Flash a current MicroPython firmware using the internal Black Magic Probe
    1) Make sure that the internal debugger is working (see [here](https://github.com/rf-eng/eliteboard_docs#flash-the-onboard-debugger-unit-internal-black-magic-probe))
    2) Download a MicroPython release (firmware_eliteboard.elf) from [here](https://github.com/rf-eng/micropython/releases/latest)
    3) Flash firmware_eliteboard.elf to the eLITe-board using the bmflash tool (see the [releases section](https://github.com/rf-eng/Black-Magic-Probe-Book/releases/latest) of the corresponding repository)
    	* A problem might appear if an old version of MicroPython was used on the board before. In this case the flash partitioning needs to be rebuilt completely. This process can be only triggered by a complete "mass_erase" run from gdb (using the erase function from bmflash is not enough) with a subsequent flash of MicroPython followed by a reset of the board. Wait until the state of the seven-segment LEDs changes before continuing.
* Run self tests
    1) A self test script is available [here](https://github.com/rf-eng/eliteboard_upyscripts/blob/master/self_test.py)
	
### Flash eLITe-Connect
* Install esptool in a Python installation using pip install ```python -m pip install git+git://github.com/eliteboard/esptool.git@master``` (has to be run in an Anaconda prompt or the like). Make sure to NOT use the official version!
* Make sure the the Micropython on the eLITe-Board is recent enough. Then you can execute ```from eliteconnect_uart_passthrough import *``` and run ```eliteconnect_pass_through()```. Note that the command will run forever and the only possibility to cancel it is pressing Ctrl+C.
* As long as ```eliteconnect_pass_through()``` is running you will be able to access the UART of eLITe-Connect using the virtual UART of the eLITe-Board
* You can then flash eLITe-Connect with either:
	1) A MicroPython firmware (from https://github.com/rf-eng/micropython/releases) or
	2) An ESP32-AT firmware	(from https://github.com/eliteboard/esp-at/releases)
* In both cases run the commands to erase the flash and to upload the *.bin file as described at https://github.com/rf-eng/micropython/releases/tag/v20210527_eliteconnect (adjust your COM-port accordingly)

### Only for Advanced Users: Flash the onboard debugger unit (*internal* Black Magic Probe)
* First, you need to flash the onboard debugger of the eLITe-board using an *external* Black Magic Probe adapter.
    1) Download the firmware files (two *.elf files) for the onboard debugger from [here](https://github.com/rf-eng/blackmagic/releases/latest)
    2) Connect your external Black Magic Probe to your computer and to the tag connector on the eLITe-board
    3) Enable the power supply of the eLITe-board over the USB-C connector
    4) Start the bmflash tool and make sure that it recognizes the external Black Magic Probe
    5) Flash both firmware files to the eLITe-board
* Verify that the internal debugger is working
    1) Disconnect the external Black Magic Probe adapter
    2) Power cycle the eLITe-board. (Re-)connecting it to the computer should show a virtual serial port. Also the "bmflash" tool should recognize the *internal* Black Magic Probe which is now available on the eLITe-board
