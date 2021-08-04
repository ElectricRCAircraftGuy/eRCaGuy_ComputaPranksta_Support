
# How to reload firmware onto the device, in case it quits working

## On Windows:

1. Download the repository: https://github.com/ElectricRCAircraftGuy/eRCaGuy_ComputaPranksta_Support --> click "Code" near the top, then "Download ZIP". Extract it.
1. Install zadig-2.5.exe from the [bin/zadig](zadig) directory.
1. Run Zadig. The interface, as shown below, appears.
    1. ![](zadig/zadig_screenshot.gif)
1. In the menu, go to *Device->Load Preset Device*.  Select **micronucleus.cfg** from the "zadig" directory.
1. Click the "Install Driver" button. The driver is now installed and Micronucleus should be ready to use. 
1. In Windows Explorer, navigate to [eRCaGuy_ComputaPranksta_Support/bin/x86_64-mingw32](x86_64-mingw32). Hold down Shift, then right-click in some blank space in the window --> go to "Open PowerShell window here".
1. In the PowerShell window that comes up, run this command: 
    ```bash
    .\micronucleus.exe --run .\load_data_into_EEPROM_7_WORKS.ino.hex
    ````
    1. It will ask you to plug in the device. Plug it in at this time. You should see something like this. It will take just a few seconds:
    ```bash
    > Please plug in the device ... 
    > Press CTRL+C to terminate the program.
    > Device is found!
    connecting: 33% complete
    > Device has firmware version 2.2
    > Device signature: 0x1e930b 
    > Available space for user applications: 6522 bytes
    > Suggested sleep time between sending pages: 7ms
    > Whole page count: 102  page size: 64
    > Erase function sleep duration: 714ms
    parsing: 50% complete
    > Erasing the memory ...
    erasing: 66% complete
    > Starting to upload ...
    writing: 83% complete
    > Starting the user app ...
    running: 100% complete
    >> Micronucleus done. Thank you!
    ```
    1. Now, the LED will be blinking rapidly on the Pranksta device, indicating it is ready for the next step. If it is not blinking rapidly, do not proceed, as something has not worked up to this point. 
    1. Unplug it.
1. In the PowerShell window, run this command: 
    ```bash
    .\micronucleus.exe --run .\computaPranksta93_RELEASE_2.ino.hex
    ``` 
    1. It will ask you to plug in the device. Plug it in at this time. You should see something like what you saw above. It will take just a few seconds. The firmware is now flashed. About 10 to 30 seconds later it should start to drag the mouse cursor slowly to the lower-left of the screen, while typing random characters. Press Caps Lock repeatedly to cycle through the various mouse movements. If this works, the device firmware is restored.



### References/Sources:
1. https://github.com/micronucleus/micronucleus/tree/master/windows_driver_installer
1. Windows micronucleus.exe: https://github.com/micronucleus/micronucleus/tree/master/commandline/builds/x86_64-mingw32 


<!--

----------

Old info (don't use this)

1. Download and install this Arduino IDE (Integrated Development Environment): https://downloads.arduino.cc/arduino-1.8.15-windows.exe. Be sure to install all components it comes with, including all drivers.
1. Open Arduino. Click "Allow access" if Windows Defender tries to block it.
1. Go to Tools -> Manage Libraries -> 

-->