## MicroPython on an ESP32

The following is a collection of notes on the installation and the 
usage of MicroPython on an ESP32, here tested using an **Adafruit Huzzah32**. Note that while the Huzzah32 was created by Adafruit, their
fork of MicroPython ("CircuitPython") is not yet available for 
this board.

_Acknowledgements:_ These notes heavely relied on the following earlier, very helpful resources:
- Installation:  
  - https://lemariva.com/blog/2017/10/micropython-getting-started  
  - https://github.com/pvanallen/esp32-getstarted
- Jupyter notebook for MicroPython:
  - https://github.com/goatchurchprime/jupyter_micropython_kernel/

### Setting up MicroPython on the ESP32 under Windows 10:

1. Install USB to Serial driver for the ESP32:  
    [Download driver here](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)
    
    
2. Restart computer


3. Update pip and install some tools:
   ```
   python -m pip install --upgrade pip
   pip install msgpack
   pip install esptool
   pip install adafruit-ampy
   ```
   
4. Download the latest Micropython software binary for ESP32:  
    [Download firmware here](https://micropython.org/download/#esp32)
  
  
5. Connect the device to the PC using an USB cable, and check the serial
   interface number where the ESP32 is connected (e.g. COM5).


6. In a (power)shell, erase the flash of the ESP32, with `<xx>` indicating
   the current COM port and `<yyy>` the name of the firmware file.
   ```
   python -m esptool --port COM<xx> erase_flash
   python -m esptool --port COM<xx> --baud 460800 write_flash --flash_mode  dio --flash_size=detect 0x1000 <yyy>.bin
   ```
   
7. Now you should be ready to go. Start a terminal program (e.g. PuTTY) and 
   connect to the COM port (at 115200 BAUD). You should see the chevrons 
   (`>>>`) of the [REPL prompt](https://docs.micropython.org/en/latest/esp8266/esp8266/tutorial/repl.html),
   indicating that the device is ready. Try something like 
   `print("Hello")` to see if the device responds.
   

8. In a powershell (or bash), use the tool `ampy` (in a powershell or bash)
   to manipulate files on the ESP32. See https://github.com/adafruit/ampy for details.
   For example, copy a python file to the flash of the ESP32 using:
   ```
   ampy --port COM<xx> put <pythonfilename>.py
   ```
   List the files in the ESP32's flash memory:
   ```
   ampy --port COM<xx> ls
   ```
   View a file in the flash:
   ```
   ampy --port COM<xx> get <filename>
   ```
   Remove a file from the flash:
   ```
   ampy --port COM<xx> rm <filename>
   ```
   Set default COM port to use shorter commands:
   ```
   $env:AMPY_PORT = 'COM<xx>'
   ampy ls
   ```

   _Note:_ In principle, similar commands ae also available from a jupyter 
   notebook connected to a python kernel running on the device (see below, 
   and see `%lsmagic` for commands), but I could not get them to work reliably.


### Using Atom to interact with MicroPython on an ESP32

The `pymakr` plugin for [Atom](https://atom.io/) allows to interact with a 
ESP32 via the REPL. In addition, one can also use it to upload all files of 
a project (in a specified folder) or run individual `.py` files directly.


### Using Jupyter to interact with a ESP32 over its serial REPL

For instructions see [this](https://github.com/teuler/micropython_ESP32/blob/master/MicroPython_Huzzah32_FirstSteps.ipynb)
exemplary notebook.

### Recompiling the firmware

For the "official way", see [MicroPython](https://micropython.org/) website.

In addition, there is a fork by [Boris Lovosevic](https://github.com/loboris/MicroPython_ESP32_psRAM_LoBo) that is useful in several ways. For example:

- It supports different ESP32 boards, also ones with more RAM (psRAM)
- The [wiki](https://github.com/loboris/MicroPython_ESP32_psRAM_LoBo/wiki)
  describes the hardware-related API specifically with respect to the ESP32.
- There are different [pre-compiled versions](https://github.com/loboris/MicroPython_ESP32_psRAM_LoBo/wiki/firmwares)
  readily available. For example, the version that includes all libraries, also supports [displays](https://github.com/loboris/MicroPython_ESP32_psRAM_LoBo/wiki/display), such as the TFT feather from Adafruit out of the box. Also, it is possible
  to compile to firmware to use both ESP32 cores, 
- ...
  
The instructions for [building the firmware under Windows](https://github.com/loboris/MicroPython_ESP32_psRAM_LoBo/wiki/winsetup)
worked (for me) using the MINGW32 toolchain; in principle, it should also work with the Linux subsystem under Windows 10. Some comments concerning the instructions:

- Installing the esptools with `pip install esptool` for me worked only after `./BUILD.sh menuconfig` was excecuted once and
  the PC was rebooted. 
- Like windows, the COM port connected to the ESP is defined just as `COMx`, that is without any path.
- The same environment can also be used to install prebuild firmware following the instructions [there](https://github.com/loboris/MicroPython_ESP32_psRAM_LoBo/wiki/firmwares).

### More resources

- https://github.com/lemariva/ESP32MicroPython
- https://github.com/pvanallen/esp32-getstarted
  (Useful starting up info)
- https://github.com/espressif/esptool
- https://learn.adafruit.com/micropython-hardware-ili9341-tft-and-featherwing/micropython
- https://github.com/loboris/MicroPython_ESP32_psRAM_LoBo/wiki
- On using with Jupyter notebooks:
  https://medium.com/@rovai/micropython-on-esp-using-jupyter-6f366ff5ed9
  

