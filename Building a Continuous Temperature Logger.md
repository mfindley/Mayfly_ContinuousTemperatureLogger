## Introduction  

This document describes what you will need to build a Continuous Temperature Logger with the [EnviroDIY Mayfly data logger](https://www.envirodiy.org/mayfly/).  
  
`https://www.envirodiy.org/mayfly/`    
  
**TABLE OF CONTENTS**  
  
{{TOC}}  

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
  
### Real-time Clock and Main Batteries  

There are two other main components that you will need to purchase before you data logger with be able to stand on its own.  

  * Real-time clock battery - CR1220 12 mm Diameter 3 volt lithium coin cell battery  
  * 3.7 volt lithium polymer battery with 2 pin JST-PH connector  

### One Wire Temperature Sensor  
  
Seeed Studio offers a [OneWire temperature sensor](https://www.seeedstudio.com/One-Wire-Temperature-Sensor-p-1235.html) that comes complete with a Grove connector, making it very simple to connect and use the sensor with little to no modification.  

`https://www.seeedstudio.com/One-Wire-Temperature-Sensor-p-1235.html`  

### Installing the Real-time clock battery  
  
Install the CR1220 battery for the real-time clock on your Mayfly board. Make sure that the orientation of the polarity (+ and -) is correct.    

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

You will need to nect need to go to **Tools > Board > Boards Manager** from the menu of the main screen. A new window will open and in the dropdown list on the top left of the screen ("**Type**"), select "**Contributed**" and then select the **EnviroDIY ATmega Boards** from the list provided. Finally, click  the "Install" button to complete your selection.
  
### Connecting to a Computer  
  
Follow the [instructions](https://www.envirodiy.org/mayfly-sensor-station-manual/#connecting-a-computer-to-the-mayfly-data-logger) on the EnviroDIY website to connect the Mayfly to your computer.  

`https://www.envirodiy.org/mayfly-sensor-station-manual/#connecting-a-computer-to-the-mayfly-data-logger`   
  
## Setting the date and time on the Real-time clock  

[Download](https://github.com/movingplaid/Mayfly_RealTimeClock) the code for setting the real-time clock and follow the instructions for compiling and uploading.  
  
  `https://github.com/movingplaid/Mayfly_RealTimeClock`
  
## Address Discovery of OneWire Temperature Sensor  
  
[Download](https://github.com/movingplaid/Mayfly_OneWireAddress) and compile the code for the OneWire sensor discovery.  
  
`https://github.com/movingplaid/Mayfly_OneWireAddress`  

Upload the compiled code to the Mayfly and record the sensor address found.

## Field Installation  
  
## MonitorMyWatershed.org  

## Definitions  

  * JST  
  * FTDI  
  * mAh  
