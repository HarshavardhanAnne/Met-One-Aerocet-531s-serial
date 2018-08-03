# Met One Instruments Aerocet 531s RS232 Communication Module

This is a python2.7 module for the Met One Instruments Aerocet 531s unit. The user manual
this module was written for can be found at:
[http://www.orionesrl.it/manuali/polveri/MET-ONE%20Aerocet%20531s_manual.pdf](http://www.orionesrl.it/manuali/polveri/MET-ONE%20Aerocet%20531s_manual.pdf)

This module is intended for use on Linux distributions, and has only been tested on Debian based distros using python 2.7.

## Getting Started

The Met One Instruments Aerocet 531s is a handheld particle counter that measures particle counts of fixed sizes. The Aerocet 531s communicates via two methods: USB port and RS232 serial port. This python module focuses on the RS232 serial communication and how to read/write data to the device.

## Prerequisites

### Materials

* RS232 to USB Adapter

### Hardware

The Aerocet 531s requires a proprietary RS232 serial cable that can be purchased from Met One Instruments. However, a standard DB9 connector can be modified to work with the Aerocet. The following modifications must be made:



On page 14 of the user manual, the PC Serial Interface shows an invalid wiring pinout. In order to properly interface with the SD-4023 using RS232 serial communication, the following connections must be made:
* 3.5mm TRS jack tip/top (left) goes to pin 4 DTR on the DB9 breakout
* 3.5mm TRS jack sleeve (ground) goes to pin 2 RX on the DB9 breakout

![alt text](wiring_diagram.jpg)

### Software

Clone this repository using SSH(or download the .zip file)

```
git clone git@github.com:HarshavardhanAnne/Met-One-Aerocet-531s-serial.git
```

Install the python2.7 serial library

```
sudo apt-get install python-serial
```

Navigate to the cloned directory and copy aerocet531s.py to the project directory you are working in

```
cp aerocet531s.py /path/of/project/
```

## How to Use

To use this module, the following inputs are required when creating an instance of this class:
* PORT: Linux serial port the unit is connected to, usually '/dev/ttyUSB0'
* PRINT_OPTION (optional): Enables or disables debug print statements (0 = Disabled, 1 = Enabled)

## Example

```
from aerocet531s import Aerocet531s  
aeroObject1 = Aerocet531s(38400,'/dev/ttyUSB0')  
#aeroObject2 = Aerocet531s(38400,'/dev/ttyUSB0',1)  

#Start serial connection to device  
aeroObject1.open()  

#Put the unit into comm mode  
aeroObject1.activate_comm_mode()  

#'SS' command returns the serial number  
results = aeroObject.command('SS')  

#'DYYYY-MM-DD' changes the date  
results = aeroObject.command('D2018-07-11')  

#'T1' changes the temperature units to F  
results = aeroObject.command('T1')  

#Get the current status of the unit  
stat = aeroObject1.get_status()  

#Start the aerocet sampling  
aeroObject1.start_sampling()  

#Stop the aerocet sampling  
aeroObject1.stop_sampling()  

#Close serial connection to device  
aeroObject1.close()  
```

## Notes

* You must call activate_comm_mode() before calling command().
* After calling activate_comm_mode(), the device is put into comm mode for about 30 seconds.
* The commands can be found in the above linked manual.
* THe commands are not case sensitive.
