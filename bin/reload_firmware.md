
# How to reload firmware onto the device, in case it quits working

<!-- MarkdownTOC -->

1. [On Windows:](#on-windows)
    1. [References/Sources:](#referencessources)
1. [On Linux Ubuntu:](#on-linux-ubuntu)
    1. [Notes to self](#notes-to-self)

<!-- /MarkdownTOC -->


<a id="on-windows"></a>
## On Windows:
_NB: these instructions are flaky and may not work on Windows right now, but this is the concept._

1. Download the repository: https://github.com/ElectricRCAircraftGuy/eRCaGuy_ComputaPranksta_Support/tree/reload_fw --> click "Code" near the top, then "Download ZIP". Extract it.
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



<a id="referencessources"></a>
### References/Sources:
1. Zadig install info: https://github.com/micronucleus/micronucleus/tree/master/windows_driver_installer
1. Windows micronucleus.exe: https://github.com/micronucleus/micronucleus/tree/master/commandline/builds/x86_64-mingw32 
1. better fork of the micronucleus firmware: https://github.com/ArminJo/micronucleus-firmware


<a id="on-linux-ubuntu"></a>
## On Linux Ubuntu:

_Tested and works in Ubuntu 20.04. This should also work in Ubuntu 16.04 and 18.04._

1. Open a terminal. Ex: via <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>T</kbd>. Now, copy and paste the following commands, one line at a time, into the terminal, pressing <kbd>Enter</kbd> after each command in order to run it. Note that pasting into the terminal can either be done via the mouse right-click menu, _or_ using <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>V</kbd>.
1. Install dependencies
    ```bash
    sudo apt update 
    sudo apt install build-essential libusb-dev git  
    ```
1. Build & install micronucleus
    ```bash
    cd /tmp
    git clone https://github.com/micronucleus/micronucleus.git
    cd micronucleus/commandline
    make
    sudo make install 
    ```
1. Copy the micronucleus udev rules over so that you can run `micronucleus` withOUT sudo; and reload the rules.
    ```bash
    cd /tmp/micronucleus/commandline
    sudo cp -i 49-micronucleus.rules /etc/udev/rules.d/
    sudo udevadm control --reload-rules
    sudo udevadm trigger
    ```
1. Upload the first set of firmware.
    ```bash
    micronucleus --run <(curl https://raw.githubusercontent.com/ElectricRCAircraftGuy/eRCaGuy_ComputaPranksta_Support/reload_fw/bin/x86_64-mingw32/load_data_into_EEPROM_7_WORKS.ino.hex)
    ````
    1. It will ask you to plug in the device. Plug it in at this time. You should see something like this. It will take just a few seconds:
        >     > Please plug in the device ... 
        >     > Press CTRL+C to terminate the program.
        >       % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
        >                                      Dload  Upload   Total   Spent    Left  Speed
        >     100  6944  100  6944    0     0  17989      0 --:--:-- --:--:-- --:--:-- 17943
        >     > Device is found!
        >     connecting: 33% complete
        >     > Device has firmware version 1.6
        >     > Available space for user applications: 6012 bytes
        >     > Suggested sleep time between sending pages: 8ms
        >     > Whole page count: 94  page size: 64
        >     > Erase function sleep duration: 752ms
        >     parsing: 50% complete
        >     > Erasing the memory ...
        >     erasing: 66% complete
        >     > Starting to upload ...
        >     writing: 83% complete
        >     > Starting the user app ...
        >     running: 100% complete
        >     >> Micronucleus done. Thank you!

    1. Several seconds after micronucleus finishes above and says `Micronucleus done. Thank you!`, the LED will begin blinking rapidly on the Pranksta device, indicating it is ready for the next step. If it is not blinking rapidly, do _not_ proceed, as something has not worked up to this point. 
    1. Unplug it.
1. Upload the second set of firmware:
    ```bash
    micronucleus --run <(curl https://raw.githubusercontent.com/ElectricRCAircraftGuy/eRCaGuy_ComputaPranksta_Support/reload_fw/bin/x86_64-mingw32/computaPranksta93_RELEASE_2.ino.hex)
    ``` 
    1. It will ask you to plug in the device. Plug it in at this time. You should see something like what you saw above. It will take just a few seconds. The firmware is now flashed. 
    1. About 10 to 30 seconds later it should start to drag the mouse cursor slowly to the lower-left of the screen, while typing random characters. Press <kbd>Caps Lock</kbd> repeatedly to cycle through the various mouse movements. If this works, the device firmware is restored.

<a id="notes-to-self"></a>
### Notes to self

<sub>See also my personal notes on my PC here: "\~/GS/dev/eRCaGuy_ComputaPranksta/README.md".</sub>


We may have to upgrade the bootloader to v2.5. IF that's the case, try the following. Official bootloader releases are located here: https://github.com/micronucleus/micronucleus/tree/master/firmware/upgrades. We will use "upgrade-t85_default.hex":

```bash
micronucleus --run <(curl https://raw.githubusercontent.com/micronucleus/micronucleus/master/firmware/upgrades/upgrade-t85_default.hex)
```

Wait about 30 seconds \~ 1 minute: do NOT unplug it during this time or you may "brick" the device and need to reprogram it with a special programmer (high voltage AVR programmer I think) instead of being able to upgrade it through its own bootloader. (To customers: this basically means you must buy a new one).

Now, the next time you flash a new program to it through the bootloader, it will say `Device has firmware version 2.5`. Ex:

>     > Please plug in the device ... 
>     > Press CTRL+C to terminate the program.
>     > Device is found!
>     connecting: 33% complete
>     > Device has firmware version 2.5

END.


<!--

----------

Old info (don't use this)

1. Download and install this Arduino IDE (Integrated Development Environment): https://downloads.arduino.cc/arduino-1.8.15-windows.exe. Be sure to install all components it comes with, including all drivers.
1. Open Arduino. Click "Allow access" if Windows Defender tries to block it.
1. Go to Tools -> Manage Libraries -> 

-->
