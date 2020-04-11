## Introduction  

This document outlines what you will need to build a Continuous Temperature Logger with the [EnviroDIY Mayfly Data  Logger](https://www.envirodiy.org/mayfly/).  It is meant to be simple and clear for new users, but those with more experience may want to expand on the application or experiment with different techniques.  There is no one specific way to do this, so if you feel that your site requires a taller or shorter mounting post, then do it. if you want a larger solar panel or bigger case, then buy them. Feel free to experiment and learn as you go.
  
Originally set up as a low cost experiment to evaluate possible uses for Citizen Science and Educators, the example data logger used for this document was assembled with off-the-shelf-components and has been [deployed](https://monitormywatershed.org/sites/STWTR2/) since February 16th, 2020. It does not utilize the Xbee cellular module which means that it will not offer real-time data via the [Monitor My Watershed](https://monitormywatershed.org/) portal. All data is saved to the on-board micro-SD card and uploaded manually.  
  
**TABLE OF CONTENTS**
  
- [Introduction](#introduction)
- [Hardware](#hardware)
  * [Starter Kit](#starter-kit)
  * [Real-time Clock Battery](#real-time-clock-battery)
  * [3.7 Volt Lithium Polymer Battery](#37-volt-lithium-polymer-battery)
  * [OneWire Temperature Sensor](#onewire-temperature-sensor)
  * [Additional Hardware](#additional-hardware)
- [Setting up the Arduino Desktop IDE Software](#setting-up-the-arduino-desktop-ide-software)
  * [Download the Arduino IDE](#download-the-arduino-ide)
  * [Adding the EnviroDIY board to Arduino](#adding-the-envirodiy-board-to-arduino)
  * [Connecting to a Computer](#connecting-to-a-computer)
- [Compiling and Uploading Code](#compiling-and-uploading-code)
  * [Setting the Date and Time on the Real-time clock](#setting-the-date-and-time-on-the-real-time-clock)
  * [Testing the OneWire Temperature Sensor](#testing-the-onewire-temperature-sensor)
  * [Continuous Temperature Logging Code](#continuous-temperature-logging-code)
- [Field Installation](#field-installation)
- [MonitorMyWatershed](#monitormywatershed)
  * [Creating an account](#creating-an-account)
  * [Registering a Site](#registering-a-site)
  * [Adding Sensors](#adding-sensors)
  * [Uploading Data](#uploading-data)
  * [Viewing Your Data](#viewing-your-data)  
  
## Hardware  

### Starter Kit  
  
The [Mayfly Starter Kit](https://www.envirodiy.org/mayfly/hardware/starter-kit/) supplies you with the essential components for building your own data logger. Stroud Water Research offers this kit through [Amazon](https://www.amazon.com/EnviroDIY-Mayfly-Arduino-Compatible-Starter/dp/B01FCVALDW/ref=sr_1_2?keywords=Stroud+Water+Research+Center&qid=1585584911&sr=8-2) for about $90 plus shipping cost.    
  
The Starter Kit includes the following items:  

  * EnviroDIY Mayfly Data Logger Board  
  * Waterproof enclosure with clear lid  
  * microUSB programming cable (1-meter long)  
  * 2 [Grove](http://wiki.seeedstudio.com/Grove_System/) cables (20cm long)  
  * 4GB [microSD](https://en.wikipedia.org/wiki/SD_card) memory card with SD card size adapter  
  * 0.5 watt solar panel with [JST connector](https://en.wikipedia.org/wiki/JST_connector)  
  * Mayfly micro-SD vertical adapter board  

Some additional items will need to be purchased separately from the Starter Kit to make this example logger fully operational:  

### Real-time Clock Battery  
 
The real-time clock battery is a small 3 volt button cell battery (like a watch battery) that helps the Mayfly retain the data and time when external power (Li-Po battery or USB) has been removed from the board. 
  
You will need to install a CR1220 battery for the real-time clock on your Mayfly board by sliding it into the circular metal battery holder located on the board. Make sure that the orientation of the polarity (+ and -) is correct so the board is not damaged.  
  
### 3.7 Volt Lithium Polymer Battery  
  
It is important to select a battery with enough storage capacity [mAh](https://www.google.com/search?rlz=1C1GCEU_enUS821US822&ei=NkePXomFAceGytMPhPGw2AI&q=mAh&oq=mAh&gs_lcp=CgZwc3ktYWIQAzIECAAQQzIHCAAQgwEQQzIECAAQQzIECAAQQzIECAAQQzIECAAQQzIECAAQQzIECAAQQzICCAAyAggAOgQIABBHOgUIABCDAUoPCBcSCzBnOTVnMjUxZzgzSgsIGBIHMGcxZzFnM1DZUlj_WWDPX2gAcAJ4AIAB8wGIAdQFkgEFMS4xLjKYAQCgAQGqAQdnd3Mtd2l6&sclient=psy-ab&ved=0ahUKEwiJ-N-C3NvoAhVHg3IEHYQ4DCsQ4dUDCAw&uact=5) to operate for a period of time without being charged or with very little opportunity to charge (not unlike your phone). This is important for installation sites that are well shaded or receive partial sunlight during the day (forests, shaded slopes or valleys).   
  
The battery must also have a 2 pin [JST-PH connector](https://www.google.com/search?rlz=1C1GCEU_enUS821US822&ei=REePXvuJIdmHytMP8_iH4AQ&q=JST-PH+connector&oq=JST-PH+connector&gs_lcp=CgZwc3ktYWIQAzICCAAyAggAMgYIABAWEB4yBggAEBYQHjIGCAAQFhAeMgYIABAWEB4yBggAEBYQHjIGCAAQFhAeMgYIABAWEB4yBggAEBYQHjoECAAQR0oOCBcSCjMtMTFnMTEtOTBKDAgYEggzLTJnMTEtMVDDhAZYw4QGYL-LBmgAcAJ4AIABU4gBU5IBATGYAQCgAQKgAQGqAQdnd3Mtd2l6&sclient=psy-ab&ved=0ahUKEwj7u9aJ3NvoAhXZg3IEHXP8AUwQ4dUDCAw&uact=5). This is a small (2mm) connector with a key or ridge on one side that mates with the connector on the Mayfly.   
  
**It is important to look at the polarity (positive & negative) of the wires coming from the battery in relation to the polarity on the Mayfly board.**  
  
  * Each battery connector on the Mayfly will have a `plus (+)` and `minus (-)` symbol printed on the circuit board  
  * The battery will have two wires attached to the connector, a `positive (red)` and a `negative (black)`  
  * These must match up when connecting the battery to the Mayfly `(red + & black -) `  
  * If your battery does not match up you will need to change the wires so they do  
  
**CONNECTING A BATTERY WITH INCORRECT POLARITY CAN BE HAZARDOUS TO YOUR WELL BEING AND TO THE MAYFLY**    
  
It is also a good measure to purchase some [additional JST connectors](https://www.digikey.com/catalog/en/partgroup/ph-series/8404). Having a few extra will help with changing the wiring on you LiPo batteries if you received them with the polarity reversed. They are very fragile and will become loose fitting if care is not taken when un-connecting them from the wiring.  

### OneWire Temperature Sensor   
  
Seeed Studio offers a [OneWire temperature sensor](https://www.seeedstudio.com/One-Wire-Temperature-Sensor-p-1235.html) that comes complete with a Grove connector, making it very simple to connect and use the sensor with little to no modification.  The only downside to have a sensor pre-wired with a connector is routing it through the waterproof enclosure.  This is where the a cable gland or silicone sealant is useful.
  
### Additional Hardware  
  
  * MicroSD card - If you plan to keep the logger in the field for an extended period of time, you may want to buy an additional microSD card so that you can swap cards whn retrieving your data.
  * Larger Solar Panel - Adafruit sells larger solar panels if you need more power for your logger.  This will also need to have a JST connector or an adapter to connect with the logger.
  * Cable Glands/Silicone Sealer - Needed to keep the sensor cable in place and to keep moisture from entering the enclosure.
  
## Setting up the Arduino Desktop IDE Software    
  
Use of the [Arduino Desktop IDE](https://www.arduino.cc/en/Main/Software) will be needed to compile and upload the [sketches](https://www.arduino.cc/en/Tutorial/Sketch) to the Mayfly. While the process is fairly straight-forward and all of the sketches are provided for you to use and modify, you may want to take some time to explore the [Arduino website](https://www.arduino.cc/) to better understand the concepts explained in this document.  
  
Detailed information on installing the Arduino software can be found on the Arduino website.  There is an [introduction](https://www.arduino.cc/en/Guide/Introduction) on what Arduino is and why you would want to use it as well as many other resources available to help you get started.
  
### Download the Arduino IDE
You will need to [download](https://www.arduino.cc/en/main/software) `version 1.6.5 or greater`. Please read the installation instructions for the corresponding operating system you are using:
  
  * [Windows](https://www.arduino.cc/en/Guide/Windows)  
  * [MacOS](https://www.arduino.cc/en/Guide/MacOSX)  
  * [Linux](https://www.arduino.cc/en/Guide/Linux)  

### Adding the EnviroDIY board to Arduino  

Before getting started in the section, you should have the following items ready:  
  
  * Arduino IDE software (installed)  
  * Mayfly board  
  * USB cable  
  * Clock battery (CR1220)  

First, we need to add some information to the Arduino software to tell it a little about the specifics of the board we are using. Start the Arduino software, then click on `File > Preferences` and paste the following URL into the text box labeled `Additional Boards Manager URLs`:  

`https://raw.githubusercontent.com/EnviroDIY/Arduino_boards/master/package_EnviroDIY_index.json` 
  
Click the "`OK`" button to close this window and return to the main screen when finished.  
  
You will need to make the Mayfly the active board in the IDE  by going to `Tools > Board > Boards Manager` from the menu of the main screen. 
  
  * A new window will open and in the drop-down list on the top left of the screen `Type > Contributed`" and then type in  `EnviroDIY` In the search bar.
  * You should see a search result for the `EnviroDIY ATmega Boards`. Click the "`Install`" button to complete your selection and click the "`Close`" button when it has finished.  
  * Now, from the `Tools > Board` menu in the main window of the IDE, select the `EnviroDIY Mayfly 1284P` from the list of available boards.  
  
### Connecting to a Computer  
  
**NOTE:** Give more explanation on how the process works (compiling, uploading, serial monitor, etc.)  
  
Follow the instructions in the [Sensor Station Manual](https://www.envirodiy.org/mayfly-sensor-station-manual/#connecting-a-computer-to-the-mayfly-data-logger) on the EnviroDIY website to connect the Mayfly to your computer. (`Section 4.2.Connecting a Computer to the Mayfly Data Logger`)  
    
  * Attach the USB cable to the Mayfly and to the computer  
  * Turn on the Mayfly using the `off/on` switch  
  * Select the `COM Port` in the Arduino software under the menu `Tools > Port`
  
Open the Serial Monitor to view the pre-loaded sketch that come with the Mayfly by pressing the `Serial Monitor` button, hold `Ctrl+Shift+M` or select `Tools > Serial Monitor` from the menu.  Make sure the "`baud`" rate option at the bottom right side of the window is set to "`57600`".
  
## Compiling and Uploading Code  
  
This section will help you get your Mayfly set up for the temperature logging code that will run once the unit has been deployed in the field. These sketches are provided with  the libraries that will be installed in each example.
  
### Setting the Date and Time on the Real-time clock    

One of the most important first steps that needs to be done is setting up the real-time clock (RTC) so that your time-stamps in your logger data are correct Make sure you have installed the clock battery before continuing with this section.      
  
In the Arduino software:  

  * Open up the Library Manager: `Tools > Manage Libraries`  
  * Type "`Sodaq_DS3231`" in the search bar   
  * Select the library named "`Sodaq_DS3231`"  
  * Click the "`Install`" button
  * Close this window when finished 
  
From the menu, select `File >  Examples > Sodaq_DS3231 > adjust` to open the example code from this library.  

Look at the following line of code in the `setup()` function. This is where the sketch sets the time:
  
`DateTime dt(2011, 11, 10, 15, 18, 0, 5);`   

This line of code translates to `year, month, date, hour, min, sec and week-day(starts from 0-Sunday and goes to 6-Saturday)`  
   
  * Change the time in this line of code to be a time in the near future (about a minute or two).  We will call this the "target time". 
  * Compile and upload the sketch, by clicking the `Upload` button, hold `Crtl+U` or select `Sketch > Upload` from then menu.   
  * Use a "reference" point like like [Time.gov](http://www.time.gov/) to view the current time.  When the reference is about 3 seconds from the "target time" we set in the code, press the `Reset` button on the Mayfly (this gives the Mayfly a few seconds to reset itself).  
  * Open the Serial Monitor by pressing the `Serial Monitor` button, hold `Ctrl+Shift+M` or select `Tools > Serial Monitor` from the menu.  Make sure the "`baud`" rate option at the bottom right side of the window is set to "`57600`".  
  * The Serial Monitor should output the current date-time set on the Mayfly.  If you are unsatisfied with the results or you missed the reference time, change the "target time" (`DateTime dt()`) in the code, upload the sketch to the Mayfly and try again.

At this point, the RTC is set but if you were to turn the Mayfly off and back on or press the `Reset` button a second time, the time would revert to the time we set in `DateTime dt()`. We want to change that line of code so that the time is NOT SET AGAIN in the `setup()` function.  
  
Close the Serial Monitor and return the the line where we set the date-time (`DateTime dt();`).  Use two slashes in front of the line of code so it looks like the following:  
  
`// DateTime dt(2011, 11, 10, 15, 18, 0, 5);`    

  * Compile and upload again by clicking the `Upload` button, hold `Crtl+U` or select `Sketch > Upload` from then menu.   
  * Open the Serial Monitor by pressing the `Serial Monitor` button, hold `Ctrl+Shift+M` or select `Tools > Serial Monitor` from the menu.  Make sure the "`baud`" rate option at the bottom right side of the window is set to "`57600`".  
  * The Serial Monitor should continue to output the correct date and time currently set on the real-time clock.  
  * When you are finished, you can close the Arduino window and select `No` when asked to save the file.  
  
### Testing the OneWire Temperature Sensor
  
If you purchased the Seeed Studio or similar OneWire temperature sensor, the following section will help you to verify that your sensor is functioning correctly.  

In the Arduino software:  

  * Open up the Library Manager: `Tools > Manage Libraries`  
  * Type "`DallasTemperature`" in the search bar   
  * Select the library named "`DallasTemperature`"  
  * Click the "`Install`" button
  * Close this window when finished   

Plug the OneWire Temperature Sensor into the Grove port marked `D4-5` on the Mayfly board.  
  
  * From the menu select `File > Examples > Dallas Temperature > tester' 
  * Search for the line: `#define ONE_WIRE_BUS 2`   
  * Change this to: `#define ONE_WIRE_BUS 4`  
  * Upload the sketch to the Mayfly by clicking the `Upload` button, hold `Crtl+U` or select `Sketch > Upload` from then menu  
  * Open the Serial Monitor by pressing the `Serial Monitor` button, hold `Ctrl+Shift+M` or select `Tools > Serial Monitor` from the menu  
  * Make sure the "`baud`" rate option at the bottom right side of the window is set to "`9600`"  
  
The Serial Monitor should output the current temperature that the sensor is reading.  This is also helpful in conducting QC tests to determine how accurate your sensor is.  
  
**Extra Credit**  

The output from the Serial Monitor scrolls by pretty quick but if you are comfortable with with making some changes, you can insert a `delay();` statement in the code to slow it down.  
  
In the first line of the `loop()` function, right after the first opening brace `{`, insert the following line of code: `delay(500);`. This will make the sketch wait briefly before printing the next temperature reading and make it easier to read.
  
When you are finished, you can close the Arduino window and select `No` when asked to save the file.   
  
### Continuous Temperature Logging Code  
  
Finally, this is the part where we get to the real world application of the Continuous Temperature Logger.  
  
**NOTE:** While it may appear to be a lot more code and more complex, there is very little you will have to change to get this running. The hard part has been done and you only need to add in your sensor address and configure the data header for the file that is saved to the SD card.  
  
The sketch for the Continuous Temperature Logger is currently set up to cycle through the following operations at one minute intervals:  
  
  * Wake up the Mayfly board
  * Get the current temperature from the sensor
  * Read the current voltage of the battery
  * Read the current temperature of the Mayfly
  * Write the information to the SD card
  * Go back to sleep (to conserve power)
  
Go to GitHub and [download](https://github.com/movingplaid/Mayfly_ContinuousTemperatureLogger) the Continuous Temperature Logging code. There will be a button on the page to "Clone or Download" the repository. Click on "Download ZIP" and save the file to your computer.  
  
You will need to extract these files to your computer before using them.  If you are unsure how do this, check this link for extracting files in [Windows 10](https://support.microsoft.com/en-us/help/14200/windows-compress-uncompress-zip-files).  If you are using something other than Windows (MacOS or Linux), the basic concept is the same.  You can do a quick Google search to find out more.  
  
Look for the line of code that looks like this:  
  
`// Simple Header`  
`#define   DATA_HEADER "Date and Time ...` 
  
This is the header information for the values that will be written to the SD card.  For now, we will keep it as it is:  
  
  * Date and Time in UTC-5  
  * Temperature (of the Sensor)  
  * Battery voltage  
  * Temperature (of the Mayfly)  
 
If you do not plan to upload your data to MonitorMyWatershed.org, you can leave these values as is, but the must stay in this order unless you make further changes to the sketch.  

**NOTE:** MonitorMyWatershed expects values to be in a specific order which is not covered in this section. Please refer to the section `Registering a Sensor Station` later in this document.  

The following libraries will need to be installed via the library manager found under `Tools > Manage Libraries`. 
  
  * OneWire 
  * Sodaq_DS3231  
  * Sodaq_PcInt
  * RTC_Timer  
  * DallasTemperature  
  
For each library you will need to do the following:  
  
  * In the Arduino IDE, go to `Tools > Manage Libraries` and type the name of the library in the search bar and press "`Enter`" on your keyboard.  
  * Select the library found and click the "`Install`" button.   
  * Click the `Close` button when you have finished instaling all of the libraries.
  
If you want to change the interval that the Mayfly wakes up to record data, search for the following line (in the `loop()` function):  
  
`if(currentminute % 1 == 0) {`  
  
You can change `currentminute % 1` to whatever value suits your needs.  For testing purposes, it is convenient to have the time set low so you do not have to wait long for each reading.  In the field you may want to increase the time to 5 or 15 minutes, depending on how precise you want your data to be.  

Upload the sketch to the Mayfly by clicking the `Upload` button, hold `Crtl+U` or select `Sketch > Upload` from then menu.  

Open the Serial Monitor by pressing the `Serial Monitor` button, hold `Ctrl+Shift+M` or select `Tools > Serial Monitor` from the menu. Make sure the "`baud`" rate option at the bottom right side of the window is set to "`9600`".  

## Field Installation  
  
Location  
  
Mounting Hardware  
  * Mounting pole  
  * Tie wraps  
  * Lanscaping pins  
  * Rebar stake  
  * PVC pipe  
  
## MonitorMyWatershed 
  
Getting started with MonitorMyWatershed is (almost) as easy as one-two-three.  
  
  * Create a new account (unless you already have one active)
  * Register a site (this is a physical location)
  * Add your sensors (temperature sensor, battery and the Mayfly)
  * Upload your data (in this example we are manually uploading data and it must be formatted correctly)
  * View your data  (fingers crossed)
  
### Creating an account  
  
  [Creating an account](https://wikiwatershed.org/help/sensor-help/sharing-sensor-data/#create-an-account-2)  

The very first step to getting your data on-line is to [register](https://monitormywatershed.org/register/) a new account.  Like many other websites, some basic information is required to sign up:
  
  * First name  
  * Last name  
  * Email address  
  * Username  
  * Password  
  * Organization (optional)  
  
Once you have enter this information, all you need to do is agree to the [Terms of Use](https://monitormywatershed.org/terms/) for the website and click the `Register` button.  
  
### Registering a Site  
  
  [Registering a Site](https://wikiwatershed.org/help/sensor-help/sharing-sensor-data/#add-a-site-2)  

On the menu bar at the top of the page, click on `My Sites`. This will open a new page where you can register a new site or display the sites you have already created.   
  
Click the button on this page that reads `Register a New Site` and a page will open where you can enter the physical information about your site such as location and type.  
  
   * Site code  
   * Site name  
   * Site type  
   * Stream name  
   * Major watershed and sub-basin  
   * Notes 
   * Latitude and longitude  
  
### Adding Sensors 
  
Once your site is set up, you can proceed to adding sensor information.  Look for the button `MANAGE SENSORS` and click on it.  
  
The page will open to show which sensors have been added to the site. If this is a new site there will be no sensors listed. This example shows the list for the logger on Stillwater Run.  
  
It is important to note that anything you want to record and display data for is considered a sensor. Even the Mayfly and the battery are sensors in this context.  
  
Click on the plus sign `+` to create a new sensor entry and fill out the required fields for each sensor you wish to add (in the order listed below).  You should have a total of three when you are finished:  
  
  * DS18B20 Waterproof Digital temperature sensor (Maxim_DS18B20_Temp)  
  * Mayfly Data Logger (EnviroDIY_Mayfly_Batt)  
  * Mayfly Data Logger (EnviroDIY_Mayfly_Temp)  

### Uploading Data
  
  [Sending/Uploading Sensor Data](https://wikiwatershed.org/help/sensor-help/sharing-sensor-data/#sensor-data)  
    
  You will also need to edit the header information (DATA_HEADER) to match your individual site (see `Customizing Header Information & Uploading the CSV Sensor Data`).  
  
  [Stroud Example File](https://wikiwatershed.org/wp-content/uploads/example-file-for-upload.csv)  
  
Example: 
  
`Sampling Feature UUID: [sampling feature UUID],,,\r\nSensor Name:,Maxim_DS18B20,EnviroDIY_Mayfly Data Logger,EnviroDIY_Mayfly Data Logger\r\nVariable Name:,Temperature_C,Battery_Voltage,Board_Temp_C\r\nResult Unit:,degreeCelsius,volt,degreeCelsius\r\nResult UUID:,[variable 1 UUID],[variable 1 UUID],[variable 1 UUID]\r\nDate and Time in UTC-5,Temperature,Battery voltage,Temperature`  
  
### Viewing Your Data  
  
  [View Site Data](https://wikiwatershed.org/help/sensor-help/sharing-sensor-data/#view-site-data)  
    
  [Sensor Observations](https://wikiwatershed.org/help/sensor-help/sharing-sensor-data/#sensor-observations)  










