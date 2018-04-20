Moitessier HAT
==============

The Moitesser HAT is named after the famous sailor [Bernard Moitessier](https://en.wikipedia.org/wiki/Bernard_Moitessier).
It is a single-board navigation solution for the Raspberry Pi and provides the following features:
* High-sensitivity dual AIS receiver (159.000 MHz to 162.025 MHz, -114dBm)
* High-performance GNSS receiver (embedded patch antenna and external antenna support)
* Sensors (depending on assembly option):
  * Temperature
  * Pressure
  * Humidity
  * 9-axis (gyroscope, accelerometer and magnetometer)
* I2C boot loader (firmware update via Raspberry Pi)
* Standalone usage or in combination with Raspberry Pi (standalone usage requires 3.3V power supply)
* Fully compatible with Raspberry Pi models supporting 40-pin IO header
* 3 status LEDs
* IO headers to interface with spare GPIOs of the Raspberry Pi
* UART signals of Raspberry Pi available on header
* Supports ID EEPROM and automatic driver loading

The Moitessier HAT is fully compatible with [OpenPlotter](http://sailoog.com/openplotter), but might be used with any navigation 
software that can use TTY as input source.

The communication with the HAT is established via the device files /dev/moitessier.spi or /dev/moitessier.tty, depending 
on whether you want to use TTY or SPI compatible access.
To configure the HAT (e.g. receiver frequency, GNSS enable/disable etc.) or read device information you have to use 
/dev/moitessier.ctrl.


Source Code
===========

Content
-------

The respository consists of several submodules that include the following:
* Linux device driver to communicate with the HAT
* User tools/applications to configure the operation mode and to update the firmware of the HAT
* Firmware of the HAT microcontroller
* Device tree and appropriate tools to write the ID EEPROM

Each submodules includes a README.md file for detailed information.


Getting the source code
-----------------------

It is assumed, that the cross-compilation is done on a host machine (development PC). This ensures faster
compilation cycles.

```
host> git clone https://github.com/mr-rooney/moitessier.git
host> cd moitessier
```

only the first time after cloning...
```
host> git submodule update --init --recursive
```

...otherwise
```
host> git submodule update
```

The following steps need to be repeated for each submodule. _\<SUBMOUDLE>_ is a placeholder for the proper submodule name.
```
host> cd <SUBMODULE>
host> git checkout master
host> cd ..
```       

Compilation
-----------

The compilation/generation process is managed by the bash script _create_, if you want to compile all the sources at once.
However, you might one to build the sources step by step. In this case use the proper scripts/Makefiles in the subdirectories.
You'll get the necessary instructions in the README.md files included in the subdirectories.
 
If you make changes on the source code file structure or you want to use a different compiler, you might need to adopt 
the configuration in this script.

Call the script without any parameter to create a debian package. This package needs to be copied to the Raspberry Pi
and installed afterwards. Use the commands below with proper IP address to access the Raspberry Pi in your network.
```
host> ./create
host> scp -r deploy/moitessier_*_armhf.deb pi@10.10.10.1:/home/pi/Desktop
```

```
pi> sudo dpkg -i ~/Desktop/moitessier_*_armhf.deb
```

After package installation, you'll find all the relevant files in /home/pi/moitessier.
Be aware that the system will reboot automatically after installation. 
