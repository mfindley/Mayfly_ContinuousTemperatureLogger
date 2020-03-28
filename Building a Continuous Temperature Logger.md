## Introduction  

This document describes what you will need to build a Continuous Temperature Logger with the [EnviroDIY Mayfly data logger](https://www.envirodiy.org/mayfly/).  
  
`https://www.envirodiy.org/mayfly/`    
  
## Hardware  
  
### Starter Kit  
The [Mayfly Starter Kit](https://www.envirodiy.org/mayfly/hardware/starter-kit/) supplies you with the essential components for getting started in EnviroyDIY programming and building your own data logger.  

`https://www.envirodiy.org/mayfly/hardware/starter-kit/`  

Stroud Water Research offers this kit through [Amazon](https://www.envirodiy.org/mayfly/hardware/starter-kit/) for $90 plus shipping cost.  

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

## Installing the Real-time clock battery  
  
Install the CR1220 battery for the real-time clock on your Mayfly board. Make sure that the orientation of the polarity (+ and -) is correct.    

## Setting up the Arduino IDE Software    
  
  `https://www.envirodiy.org/mayfly-sensor-station-manual/#preparing-the-mayfly-data-logger`  

### Download the Arduino IDE (version 1.6.5 or newer) 

`https://www.arduino.cc/en/main/software`  

### Install the Arduino IDE  
  
Run th installer software and follow the instructions provided.  

### Adding the EnviroDIY board to Arduino  

Start the Arduino software, then click on **File > Preferences** and paste the following URL into the box labeled **Additional Boards Manager URLs**:  

`https://raw.githubusercontent.com/EnviroDIY/Arduino_boards/master/package_EnviroDIY_index.json`
  
### Connecting to a Computer  
  
`https://www.envirodiy.org/mayfly-sensor-station-manual/#connecting-a-computer-to-the-mayfly-data-logger`   

Download the code for setting the real-time clock and follow the instructions for compiling and uploading.  

## Definitions  

  * JST  
  * FTDI  
  * mAh  
