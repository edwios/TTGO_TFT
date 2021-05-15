## ST7789 driver

Changes 
======

These are the changes made to the original st7789_mpy repo for use with Micropython 1.15

Copy directory of `st7789/` into `modules` directory inside the `TTGO_TFT` directory.

Inside the `TTGO_TFT/modules` directory, add a file called `micropython.cmake` with the following content:

`include(${CMAKE_CURRENT_LIST_DIR}/st7789/micropython.cmake)`

The st7789 driver in this version will determine if it is a landscape orientation or portrait orientation according to the width and height parameter given when the display was initialised.

If width > height, then it is assumed landscape, otherwise portrait.

Install
======

Checkout this repo into the `boards` subdirectory of the ESP32 micropython port. E.g.

```
cd micropython/ports/esp32/boards
git clone git@github.com:edwios/TTGO_TFT.git
```

Update the submodules used:

`git submodule update --init --recursive`

Directory structure:

```
micropython/
├── ports/
│   ├── esp32/
│   │   ├── boards/
│   │   │   ├── TTGO_TFT/
│   │   │   |   ├── mpconfigboard.cmake
│   │   │   |   ├── mpconfigboard.h
│   │   │   │   └── modules/
│   │   │   │       ├── micropython.cmake
│   │   │   │       └── st7789_mpy/
|   │   │   │           └── st7789/
│   │   │   │               ├── st7789.c
│   │   │   │               ├── st7789.h
│   │   │   │               ├── micropython.mk
│   │   │   │               └── micropython.cmake
... 

```

Build
======

```
cd micropython/ports/esp32
make BOARD=TTGO_TFT USER_C_MODULES=../boards/TTGO_TFT/modules/micropython.cmake
```

Todo
=====

Have `make` to automatically include the module without specifying using `USER_C_MODULES`

Example
======

```
# ESP32

import machine
import st7789

bl = machine.Pin(4, machine.Pin.OUT)
bl.value(1)

spi = machine.SPI(1, baudrate=20000000, polarity=1, phase=1, sck=machine.Pin(18), mosi=machine.Pin(19))
display = st7789.ST7789(spi, 240, 135, reset=machine.Pin(23, machine.Pin.OUT), cs=machine.Pin(5, machine.Pin.OUT), dc=machine.Pin(16, machine.Pin.OUT))
display.init()
display.fill(st7789.color565(0,255,0))

```
