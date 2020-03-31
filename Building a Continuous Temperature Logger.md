## Introduction  

This document describes what you will need to build a Continuous Temperature Logger with the [EnviroDIY Mayfly data logger](https://www.envirodiy.org/mayfly/).  
  
From the EnviroDIY website:
  
> "The EnviroDIY Mayfly Data Logger is a powerful, user-programmable microprocessor board that is fully compatible with the Arduino IDE software."

> "The memory card socket, RTC, and solar charging features make it very easy to use this board for data logging and low-power operation. But it can also be used by itself, or with a wide variety of devices and accessories.  The Mayfly can be powered by a 3.7v lithium battery or through the microUSB port."  
  
![](/images/mayfly_board-only.jpg)    

The example data logger used in this document was meant to be assembled as easily as possible and for a low cost. It does not utilize the Xbee cellular modual which means that it will not offer real-time data via the [Monitor My Watershed](https://monitormywatershed.org/) portal. All data will be saved to the onboard microSD card and will have to be retrieved and uploaded manually.  

The use of the [Arduino Desktop IDE](https://www.arduino.cc/en/Main/Software) will be needed to compile and upload the [sketches](https://www.arduino.cc/en/Tutorial/Sketch) to the Mayfly. The process is fairly straight-forward and all of the sketches are provided.  
  
**TABLE OF CONTENTS**  
  
- [Introduction](#introduction)
- [Hardware](#hardware)
  * [Starter Kit](#starter-kit)
  * [Real-time Clock Battery](#real-time-clock-battery)
  * [3.7 Volt Lithium Polymer Battery](#37-volt-lithium-polymer-battery)
  * [OneWire Temperature Sensor](#onewire-temperature-sensor)
- [Setting up the Arduino Desktop IDE Software](#setting-up-the-arduino-desktop-ide-software)
  * [Adding the EnviroDIY board to Arduino](#adding-the-envirodiy-board-to-arduino)
  * [Connecting to a Computer](#connecting-to-a-computer)
- [Compiling and Uploading Code](#compiling-and-uploading-code)
  * [Setting the date and time on the Real-time clock](#setting-the-date-and-time-on-the-real-time-clock)
  * [Address Discovery of OneWire Temperature Sensor](#address-discovery-of-onewire-temperature-sensor)
  * [Testing the OneWire Temperature Sensor](#testing-the-onewire-temperature-sensor)
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
  
![](/images/mayfly_starter-kit.jpg)  
  
Stroud Water Research offers this kit through [Amazon](https://www.amazon.com/EnviroDIY-Mayfly-Arduino-Compatible-Starter/dp/B01FCVALDW/ref=sr_1_2?keywords=Stroud+Water+Research+Center&qid=1585584911&sr=8-2) for about $90 plus shipping cost.    
  
The Starter Kit includes the following items:  

  * EnviroDIY Mayfly Data Logger Board  
  * Waterproof enclosure with clear lid  
  * microUSB programming cable (1-meter long)  
  * 2 [Grove](http://wiki.seeedstudio.com/Grove_System/) cables (20cm long)  
  * 4GB [microSD](https://en.wikipedia.org/wiki/SD_card) memory card with SDcard-size adapter  
  * 0.5 watt solar panel with [JST connector](https://en.wikipedia.org/wiki/JST_connector)  
  * Mayfly microSD vertical adapter board  

Some additional items will need to be purchased separately from the Starter Kit to make the logger fully operational:  

### Real-time Clock Battery  
  
The Real-time clock (RTC) battery is a small 3 volt button cell battery (like a watch battery) that helps the Mayfly retain the data and time when external power (Li-Po battery or USB) has been removed from the board. They are fairly common and can be purchased from many retailers (online, grocery stores, drug stores, etc.).  
  
![](/images/battery_cr1220.jpg)
  
Install the CR1220 battery for the real-time clock on your Mayfly board. Make sure that the orientation of the polarity (+ and -) is correct.  
  
### 3.7 Volt Lithium Polymer Battery  
  
There are many different types of batteries available for use. It is important to select a battery with enough storage capacity to operate for a period of time without being charged or with very little opportunity to charge (not unlike your phone). This is important for installation sites that are well shaded or recieve partial sunlight during the day (forests, shaded slopes or valleys).  
  
![](/images/battery_3.7v-lipo.jpg)
  
Battery manufactures use a rating of `mAh` or milliampere-hours (A smartphone battery usually has between 2500 and 4000 milliampere-hours of electric capacity). The temperature logger that this document is based on is using a 1200 mAh but it may be wise to use something a little larger for optimal operation (2000-3000 mAh - **need verification**).    
  
The battery must also have a 2 pin [JST-PH](https://en.wikipedia.org/wiki/JST_connector) connector. This is a small (2mm) connector with a key or ridge on one side that mates with the connector on the Mayfly.   
  
![](/images/connector_jst-ph.jpg)
  
**It is important to look at the polarity (positive & negative) of the wires coming from the battery in relation to the polarity on the Mayfly board.**  
  
Not all manufacturers configure their batteries the same way and you might receive a battery thatis wired backwards.
  
Each battery connector on the Mayfly will have a `plus (+)` and `minus (-)` symbol printed on the circut board. The battery will have two wires attached to it, a `positive (red)` and a `negative (black)`. These must match up when connecting the battery to the Mayfly `(red + & black -)`. If your battery does not match up you will need to change the wires so they do.  
  
**CONNECTING A BATTERY WITH INCORRECT POLARITY CAN BE HAZARDOUS TO YOUR WELL BEING AND TO THE MAYFLY**    
  
The Li-Po battery is not immediately required for setting up the Mayfly board but it will be needed when you want to run the board without being connected to your computer (ie. in the field).  

### OneWire Temperature Sensor  
  
Seeed Studio offers a [OneWire temperature sensor](https://www.seeedstudio.com/One-Wire-Temperature-Sensor-p-1235.html) that comes complete with a [Grove](http://wiki.seeedstudio.com/Grove_System/) connector, making it very simple to connect and use the sensor with little to no modification.  
  
![](/images/seeed_onewire-sensor.jpg)  
  
Each temperature sensor has a unique address to identify it.  Use the code in the "**Address Discovery of OneWire Temperature Sensor**" section to find the address of your sensor. Be sure to keep this number on hand when you get to compiling the code for temperature logging. 

## Setting up the Arduino Desktop IDE Software    
  
Detailed information on setting up the Arduino software can be found on the Arduino website.  You will need to [download](https://www.arduino.cc/en/main/software) `version 1.6.5 or greater`. 
  
Please read the instructions for the coresponding operating system you have installed:
  
  * [Windows](https://www.arduino.cc/en/Guide/Windows)  
  * [MacOS](https://www.arduino.cc/en/Guide/MacOSX)  
  * [Linux](https://www.arduino.cc/en/Guide/Linux)  
  * [Portable IDE](https://www.arduino.cc/en/Guide/PortableIDE) (Windows and Linux)   
  
There is also an [introduction](https://www.arduino.cc/en/Guide/Introduction) on what Arduino is and why you would want to use it.  

### Adding the EnviroDIY board to Arduino  

Start the Arduino software, then click on `File > Preferences` and paste the following URL into the box labeled `Additional Boards Manager URLs`:  

`https://raw.githubusercontent.com/EnviroDIY/Arduino_boards/master/package_EnviroDIY_index.json`  
  
![](/images/arduino_preferences.jpg)  
  
You will need to go to `Tools > Board > Boards Manager` from the menu of the main screen. A new window will open and in the dropdown list on the top left of the screen ("`Type`"), select "`Contributed`" and then type in  `EnviroDIY ATmega Boards` In the search bar. 
  
You should see a search result for the `EnviroDIY ATmega Boards`. Finally, click  the "`Install`" button to complete your selection.
  
![](/images/arduino_board-manager.jpg) 
  
Now, from the `Tools > Board` menu, select the EnviroDIY Mayfly 1284P board.
  
![](/images/arduino_board-menu.jpg) 
  
### Connecting to a Computer  
  
Follow the [instructions](https://www.envirodiy.org/mayfly-sensor-station-manual/#connecting-a-computer-to-the-mayfly-data-logger) on the EnviroDIY website to connect the Mayfly to your computer.   
    
  * Attach the USB cable to the Mayfly and to the computer  
  * Turn on the Mayfly using the `off/on` switch  
  * Select the COM port in the Arduino software under the menu `Tools > Port`  
  
![](/images/arduino_com-port.jpg)  
  
  * Open the Serial Monitor in the Adruino software (top right corner of the window) to view the output from the pre-loaded sketch that come with the Mayfly
  
## Compiling and Uploading Code  

### Setting the date and time on the Real-time clock    

One of the first things that need to be done before using the Mayfly for data logging is setting the date and time on the real-time clock (RTC). 

[Download](https://github.com/movingplaid/Mayfly_RealTimeClock/blob/master/Mayfly_RealTimeClock.ino) the code for setting the real-time clock.  
  
This sketch is meant to be a simple as possible, but you need to do things in the right order to obtain the best possible time setting. Since we are setting the time based on the compile time of the sketch, the faster you compile and upload the less difference you will have between the system time and the Mayfly time.

  * Complile and upload the sketch
  * Comment out the line for setting the clock
  * Complile and upload again
  
We do a second compile and upload after commenting out the portion of the code that sets the time so that the next time you open the Serial Monitor or turn the Mayfly on, the time will not get reset to the compile time again.   
  
Open the file (`Mayfly_RealTimeClock.ino`) in the Arduino IDE. Before compiling, you will need to install the following library:  
  
  * `RTClib`
  
In the Arduino software, go to `Tools > Manage Libraries` and type "`RTClib`" in the serach bar and press "`Enter`" on your keyboard.  
  
![](/images/arduino_rtclib.jpg)  
  
Select the library named "`RTClib`" and click the "`Install`" button.  
  
To compile and upload the code you can click the `Upload` button, hold `Crtl+U` or select `Sketch > Upload` from then menu.   
  
Now, comment out the line of code:  
  
`rtc.adjust(DateTime(F(__DATE__),F(__TIME__)));`  
  
So it looks like this (Use // in front of the line):  
  
![](/images/arduino_rtc-time-commented-out.jpg)  
  
Compile and upload again.  

Open the Serial Monitor by pressing the `Serial Monitor` button, hold `Ctrl+Shift+M` or select `Tools > Serial Monitor` from the menu.  Make sure the "`baud`" rate option at the bottom right side of the widnow is set to "`9600`".
  
![](/images/arduino_baud.jpg)
  
The Serial Monitor should output the date and time currently set on the RTC.  

![](/images/arduino_serial-monitor-rtc.jpg)  

### Address Discovery of OneWire Temperature Sensor  
  
[Download](https://github.com/movingplaid/Mayfly_OneWireAddress/blob/master/Mayfly_OneWireAddress.ino) the code for the OneWire Device Address discovery.  
  
The following library is requires to compile this code:  
  
  * OneWire
  
In the Arduino software, go to `Tools > Manage Libraries` and type "`OneWire`" in the serach bar and press "`Enter`" on your keyboard.  

Select the library named "`OneWire`" and click the "`Install`" button. 

Plug the OnewWire Temperature Sensor into the Grove port marked `D4-5` on the Mayfly board.  
  
Upload the sketch to the Mayfly by clicking the `Upload` button, hold `Crtl+U` or select `Sketch > Upload` from then menu.  

Open the Serial Monitor by pressing the `Serial Monitor` button, hold `Ctrl+Shift+M` or select `Tools > Serial Monitor` from the menu. Make sure the "`baud`" rate option at the bottom right side of the widnow is set to "`9600`".
  
![](/images/arduino_baud.jpg)
  
The Serial Monitor should output a string of characters that is your sensors address. Write this address down or save it somewhere to access it later when building the Temperature Logging Code.  

Example output from this sketch:  

`Start oneWireSearch.ino`

`0x28, 0x48, 0x98, 0xD6, 0x0B, 0x00, 0x00, 0x8A	// CRC OK`  
`Number of Devices Found: 1`

`End oneWireSearch.ino`

If you need to run the sketch again, press the "`reset`" button on the Mayfly or upload the sketch again.
  
### Testing the OneWire Temperature Sensor
  
[Download](https://github.com/movingplaid/Mayfly_OneWireAddress/blob/master/Mayfly_OneExample.ino) the code for the OneWire Example.  
 
Change theh DeviceAddress to match your temperature sensor.  

The following library is requires to compile this code:  
  
  * OneWire
  
In the Arduino software, go to `Tools > Manage Libraries` and type "`OneWire`" in the serach bar and press "`Enter`" on your keyboard.  

Select the library named "`OneWire`" and click the "`Install`" button. 

Plug the OnewWire Temperature Sensor into the Grove port marked `D4-5` on the Mayfly board.  
  
Upload the sketch to the Mayfly by clicking the `Upload` button, hold `Crtl+U` or select `Sketch > Upload` from then menu.  

Open the Serial Monitor by pressing the `Serial Monitor` button, hold `Ctrl+Shift+M` or select `Tools > Serial Monitor` from the menu. Make sure the "`baud`" rate option at the bottom right side of the widnow is set to "`9600`".
  
![](/images/arduino_baud.jpg)
  
The Serial Monitor should output the current temperature that the sensor is reading. 
  
### Continuous Temperature Logging Code  
  
[Download](https://github.com/movingplaid/Mayfly_TempProbeLogging) the Temperature Logging code from GitHub   
  
You will need to edit the TempProbLogging code to replace the DeviceAddres with the address discoved for your sensor (see `Address Discovery of OneWire Temperature Sensor`)  

Open the code in the Arduino software and serach for the line:  

`DeviceAddress TempSensor = `   

Copy the DeviceAddress you found previously and paste it into your code between the curly braces {}.    
  
Example:  
  
`DeviceAddress TempSensor = { 0x28, 0x48, 0x98, 0xD6, 0x0B, 0x00, 0x00, 0x8A };`

The following libraries will need to be installed via the library manager found under `Tools > Manage Libraries`. 
  
  * OneWire 
  * Sodaq_DS3231  
  * Sodaq_PcInt
  * RTC_Timer  
  * DallasTemperature  
  
For each library you will need to do the following:  
  
In the Arduino software, go to `Tools > Manage Libraries` and type the name of the library in the serach bar and press "`Enter`" on your keyboard.  
  
Select the library found and click the "`Install`" button.  

You will also need to edit the header information (DATA_HEADER) to match your individual site (see `Customizing Header Information & Uploading the CSV Sensor Data`).  
  
Example: 
  
`Sampling Feature UUID: [sampling feature UUID],,,\r\nSensor Name:,Maxim_DS18B20,EnviroDIY_Mayfly Data Logger,EnviroDIY_Mayfly Data Logger\r\nVariable Name:,Temperature_C,Battery_Voltage,Board_Temp_C\r\nResult Unit:,degreeCelsius,volt,degreeCelsius\r\nResult UUID:,[variable 1 UUID],[variable 1 UUID],[variable 1 UUID]\r\nDate and Time in UTC-5,Temperature,Battery voltage,Temperature`  
  
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
  
