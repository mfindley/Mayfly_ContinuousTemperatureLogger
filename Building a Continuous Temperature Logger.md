## Introduction  

This document describes what you will need to build a Continuous Temperature Logger with the [EnviroDIY Mayfly data logger](https://www.envirodiy.org/mayfly/).  
  
`https://www.envirodiy.org/mayfly/`    
  
The example data logger used in this document was meant to be assembled as easily as possible and for a low cost. It does not utilize the Xbee cellular modual which means that it will not offer real-time data via the Monitor My Watershed portal. All data will be saved to the onboard microSD card and will have to be retrieved and uploaded manually.  

In addition, the use of the Adrduio programming environment will be needed to compile and upload code to the Mayfly. The process is fairly straight-forward and  all of the code has been provided.  
  
**TABLE OF CONTENTS**  
  
- [Introduction](#introduction)
- [Hardware](#hardware)
  * [Starter Kit](#starter-kit)
  * [Real-time Clock (RTC) Battery](#real-time-clock--rtc--battery)
  * [3.7 Volt Lithium Polymer (Li-Po) Battery](#37-volt-lithium-polymer--li-po--battery)
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
  * [Creating an account](#creating-an-account)
  * [Registering a Sensor Station](#registering-a-sensor-station)
  * [Customizing Header Information & Uploading the CSV Sensor Data](#customizing-header-information---uploading-the-csv-sensor-data)
- [Definitions](#definitions) 

## Hardware  

### Starter Kit  
The [Mayfly Starter Kit](https://www.envirodiy.org/mayfly/hardware/starter-kit/) supplies you with the essential components for building your own data logger.  

`https://www.envirodiy.org/mayfly/hardware/starter-kit/`  

Stroud Water Research offers this kit through [Amazon](https://www.amazon.com/EnviroDIY-Mayfly-Logger-Arduino-Compatible/dp/B01F9B4WCG) for about $90 plus shipping cost.  

`https://www.amazon.com/EnviroDIY-Mayfly-Logger-Arduino-Compatible/dp/B01F9B4WCG`  

The Starter Kit includes the following items:  

  * EnviroDIY Mayfly Data Logger Board
  * Waterproof enclosure with clear lid
  * microUSB programming cable (1-meter long)
  * 2 Grove cables (20cm long)
  * 4GB microSD memory card with SDcard-size adapter
  * 0.5 watt solar panel with JST connector
  * Mayfly microSD vertical adapter board  

Some additional items will need to be purchased separately from the Starter Kit to make the logger fully operational:  

### Real-time Clock (RTC) Battery  
  
The Real-time clock battery is a small 3 volt button cell battery (like a watch battery) that helps the Mayfly retain the data and time when external power (Li-Po battery or USB) has been removed from the board. They are very common and can be purchased from many retailers (Amazon, grocery stores, big box stores).  
  
Install the CR1220 battery for the real-time clock on your Mayfly board. Make sure that the orientation of the polarity (+ and -) is correct.  
  
### 3.7 Volt Lithium Polymer (Li-Po) Battery  
  
There are many different types of batteries available for use. It is important to select a battery with enough storage capacity to operate for a period of time without being charged or with very little opportunity to charge (not unlike your phone). This is important for installation sites that are well shaded or recieve partial sunlight during the day (forests, shaded slopes or valleys).  
  
Battery manufactures use a rating of mAh or milliampere-hours (A smartphone battery usually has between 2500 and 4000 milliampere-hours of electric capacity). The temperature logger that this document is based on is using a 1200 mAh but it may be wise to use something a little larger for optimal operation (2000-3000 mAh - **need verification**).    
  
The battery must also have a 2 pin JST-PH connector. This is a small (2mm) connector with a key or ridge on one side that mates with the connector on the Mayfly.   

**It is important to look at the polarity (positive & negative) of the wires coming from the battery in relation to the polarity on the Mayfly board.**
  
Each connector on the Mayfly will have a plus (+) and minus (-) symbol printed on the circut board. The battery will have two wires attached to it, a positive (red) and a negative (black). These must match up when connecting the battery to the Mayfly (red + & black -). If your battery does not match up you will need to change the wires so they do.  
  
**Conecting a battery with incorrect polarity can be hazardous to your well being and to the Mayfly.**    
  
The Li-Po battery is not immediately required for setting up the Mayfly board but it will be needed when you want to run the board without being connected to your computer (ie. in the field).  

### One Wire Temperature Sensor  
  
Seeed Studio offers a [OneWire temperature sensor](https://www.seeedstudio.com/One-Wire-Temperature-Sensor-p-1235.html) that comes complete with a Grove connector, making it very simple to connect and use the sensor with little to no modification.  

`https://www.seeedstudio.com/One-Wire-Temperature-Sensor-p-1235.html`  

Each temperature sensor has a unique address to identify it.  Use the code in the "**Address Discovery of OneWire Temperature Sensor**" section to find the address of your sensor. Be sure to keep this number on hand when you get to compiling the code for temperature logging. 

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
  
  * Attach the USB cable to the Mayfly and to the computer  
  * Turn on the Mayfly  
  * Select the COM port in the Arduino software ( **Tools > Port** )  
  * Open the Serial Monitor in the Adduino software (top right corner of the window) to view the output from the pre-loaded sketch
  
## Compiling and Uploading Code  

### Installing Libraries  
  
In the Arduino IDE software, go to **Tools > Library Manager**.  In the search bar at the top of the library window, type in the name of the library you want to install and press the "Enter" key. Pick the library that best matches your needs and click the "Install" button.

### Setting the date and time on the Real-time clock  

One of the first things that need to be done before using the Mayfly for data logging is setting the correct date and time on the real-time clock. 

[Download](https://github.com/movingplaid/Mayfly_RealTimeClock) the code for setting the real-time clock.  
  
  `https://github.com/movingplaid/Mayfly_RealTimeClock`  
  
The following library is requires to compile this code:  
  
  * Sodaq_DS3231 (Sodaq_DS3231.h)
  
In the Arduino software, go to **Tools > Manage Libraries** and type "**Sodaq_DS3231**" in the serach bar and press "**Enter**" on your keyboard.  

Select the library named "**Sodaq_DS3231**" and click the "**Install**" button.  

### Address Discovery of OneWire Temperature Sensor  
  
[Download](https://github.com/movingplaid/Mayfly_OneWireAddress) and compile the code for the OneWire sensor discovery.  
  
`https://github.com/movingplaid/Mayfly_OneWireAddress`  

The following library is requires to compile this code:  
  
  * OneWire (OneWire.h)
  
In the Arduino software, go to **Tools > Manage Libraries** and type "**OneWire**" in the serach bar and press "**Enter**" on your keyboard.  

Select the library named "**OneWire**" and click the "**Install**" button. 

Upload the compiled code to the Mayfly and record the sensor address found.
  
### Continuous Temperature Logging Code  
  
[Download](https://github.com/movingplaid/Mayfly_TempProbeLogging) the Temperature Logging code from GitHub   
  
  `https://github.com/movingplaid/Mayfly_TempProbeLogging`  
  
You will need to edit the TempProbLogging code to replace the DeviceAddres with the address discoved for your sensor (see **Address Discovery of OneWire Temperature Sensor**)  

Open the code in the Arduino software and serach for the line:  

`DeviceAddress TempSensor = { 0x28, 0x48, 0x98, 0xD6, 0x0B, 0x00, 0x00, 0x8A };`  
  
**Note:** The address may be slightly differect from the one show above.  

The following libraries will need to be installed via **Manage Libraries** found under **Tools > Manage Libraries**. This procedure can be found under the section **Installing Libraries** in this document.  

Upload to Mayfly  
  
## Field Installation  
  
Location  
Mounting Hardware  

## MonitorMyWatershed.org  
  
### Creating an account  

`https://monitormywatershed.org/register/`  

### Registering a Sensor Station  
  
`https://monitormywatershed.org/sites/register/`  

### Customizing Header Information & Uploading the CSV Sensor Data  
  
  `https://wikiwatershed.org/help/sensor-help/sharing-sensor-data/#sensor-data`  
  
## Definitions  

  * JST   
  * mAh  
