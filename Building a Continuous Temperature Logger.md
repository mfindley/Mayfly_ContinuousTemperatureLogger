## Introduction  

This document describes what you will need to build a Continuous Temperature Logger with the [EnviroDIY Mayfly data logger](https://www.envirodiy.org/mayfly/).  
  
`https://www.envirodiy.org/mayfly/`    
  
**TABLE OF CONTENTS**  
  
- [Introduction](#introduction)
- [Hardware](#hardware)
  * [Starter Kit](#starter-kit)
  * [Real-time Clock (RTC) Battery](#real-time-clock--rtc--battery)
  * [3.7 Volt Lithium Polymer (Lipo) Battery](#37-volt-lithium-polymer--lipo--battery)
  * [One Wire Temperature Sensor](#one-wire-temperature-sensor)
- [Setting up the Arduino Desktop IDE Software](#setting-up-the-arduino-desktop-ide-software)
  * [Adding the EnviroDIY board to Arduino](#adding-the-envirodiy-board-to-arduino)
  * [Connecting to a Computer](#connecting-to-a-computer)
- [Compiling and Uploading Code](#compiling-and-uploading-code)
  * [Installing Libraries](#installing-libraries)
  * [Setting the date and time on the Real-time clock](#setting-the-date-and-time-on-the-real-time-clock)
  * [Address Discovery of OneWire Temperature Sensor](#address-discovery-of-onewire-temperature-sensor)
  * [Continuous Temperature Logging Code](#continuous-temperature-logging-code)
- [Field Installation](#field-installation)
- [MonitorMyWatershed.org](#monitormywatershedorg)
- [Definitions](#definitions)  

## Hardware  

### Starter Kit  
The [Mayfly Starter Kit](https://www.envirodiy.org/mayfly/hardware/starter-kit/) supplies you with the essential components for getting started in EnviroyDIY programming and building your own data logger.  

`https://www.envirodiy.org/mayfly/hardware/starter-kit/`  

Stroud Water Research offers this kit through [Amazon](https://www.amazon.com/EnviroDIY-Mayfly-Logger-Arduino-Compatible/dp/B01F9B4WCG) for $90 plus shipping cost.  

`https://www.amazon.com/EnviroDIY-Mayfly-Logger-Arduino-Compatible/dp/B01F9B4WCG`  

The starter kit includes the following items:  


  * EnviroDIY Mayfly Data Logger Board
  * Waterproof enclosure with clear lid
  * microUSB programming cable (1-meter long)
  * 2 Grove cables (20cm long)
  * 4GB microSD memory card with SDcard-size adapter
  * 0.5 watt solar panel with JST connector
  * Mayfly microSD vertical adapter board
  * 6-pin header adapter for FTDI devices
  
### Real-time Clock (RTC) Battery  
  
Install the CR1220 battery for the real-time clock on your Mayfly board. Make sure that the orientation of the polarity (+ and -) is correct.    
  
### 3.7 Volt Lithium Polymer (Lipo) Battery  
  
It is important to look at the polarity (positive & negative) of the wires coming from the battery in relation to the polarity on the Mayfly board. 

### One Wire Temperature Sensor  
  
Seeed Studio offers a [OneWire temperature sensor](https://www.seeedstudio.com/One-Wire-Temperature-Sensor-p-1235.html) that comes complete with a Grove connector, making it very simple to connect and use the sensor with little to no modification.  

`https://www.seeedstudio.com/One-Wire-Temperature-Sensor-p-1235.html`  

Each temperature sensor has a unique address to identify it.  Use the code in the "Address Discovery of OneWire Temperature Sensor" section to find the address of your sensor. Be sure to keep this number on hand when you get to compiling the code for temperature logging. 

## Setting up the Arduino Desktop IDE Software    
  
Detailed information on setting up the Arduino software can be found on the Arduino website.  You will need to [download](https://www.arduino.cc/en/main/software) version 1.6.5 or greater.

  `https://www.arduino.cc/en/main/software`  
  
Please read the instructions for the coresponding operating system you have installed:
  
  * [Windows](https://www.arduino.cc/en/Guide/Windows)  
  * [MacOS](https://www.arduino.cc/en/Guide/MacOSX)  
  * [Linux](https://www.arduino.cc/en/Guide/Linux)  
  * [Portable IDE](https://www.arduino.cc/en/Guide/PortableIDE) (Windows and Linux)   
  
There is also an [introduction](https://www.arduino.cc/en/Guide/Introduction) on what Arduino is and why you would want to use it.  

### Adding the EnviroDIY board to Arduino  

Start the Arduino software, then click on **File > Preferences** and paste the following URL into the box labeled **Additional Boards Manager URLs**:  

`https://raw.githubusercontent.com/EnviroDIY/Arduino_boards/master/package_EnviroDIY_index.json`  

You will need to go to **Tools > Board > Boards Manager** from the menu of the main screen. A new window will open and in the dropdown list on the top left of the screen ("**Type**"), select "**Contributed**" and then select the **EnviroDIY ATmega Boards** from the list provided. Finally, click  the "Install" button to complete your selection.
  
### Connecting to a Computer  
  
Follow the [instructions](https://www.envirodiy.org/mayfly-sensor-station-manual/#connecting-a-computer-to-the-mayfly-data-logger) on the EnviroDIY website to connect the Mayfly to your computer.  

`https://www.envirodiy.org/mayfly-sensor-station-manual/#connecting-a-computer-to-the-mayfly-data-logger`   
  
## Compiling and Uploading Code  

### Installing Libraries  
  
In the Arduino IDE software, go to **Tools > Library Manager**.  In the search bar at the top of the library window, type in the name of the library you want to install and press the "Enter" key. Pick the library that best matches your needs and click the "Install" button.

### Setting the date and time on the Real-time clock  

One of the first things that need to be done before using the Mayfly for data logging is setting the correct date and time on the real-time clock. 

[Download](https://github.com/movingplaid/Mayfly_RealTimeClock) the code for setting the real-time clock.  
  
  `https://github.com/movingplaid/Mayfly_RealTimeClock`  
  
The following library is requires to compile this code:  
  
  * Sodaq_DS3231 (Sodaq_DS3231.h)
  
### Address Discovery of OneWire Temperature Sensor  
  
[Download](https://github.com/movingplaid/Mayfly_OneWireAddress) and compile the code for the OneWire sensor discovery.  
  
`https://github.com/movingplaid/Mayfly_OneWireAddress`  

The following library is requires to compile this code:  
  
  * OneWire (OneWire.h)

Upload the compiled code to the Mayfly and record the sensor address found.
  
### Continuous Temperature Logging Code  

Download  
Change Sensor Address  
Install Libraries  
Upload to Mayfly  
  
## Field Installation  
  
Location  
Mounting Hardware  

## MonitorMyWatershed.org  
  
Creating an account  
Registering a Sensor Station  
Customizing Header Information
Uploading the Sensor Data  

## Definitions  

  * JST  
  * FTDI  
  * mAh  
