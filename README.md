# AbletonLiveOnLinux
A Repository for All Things Related to Running Ableton Live on Linux, part of the newly formed [Ableton on Linux Discord Group](https://discord.gg/yM2Jjh8xYA).

# Live 12
## Install Documentation (WIP)
#### Enviroment Tested
* Linux 6.13.6
* DE: KDE Plasma 6.3.3
* WM: KWin (Wayland)
* CPU: AMD Ryzen 7 7735HS
* GPU 1: AMD Radeon RX 7700S
* GPU 2: AMD Radeon 680M
* wine-tkg-valve-exp-bleeding-9.0.174637.20250316-327-x86_64
* Ableton Live 12.1.10

## Instructions
**Credits to user actondev in the discord for the new method which works far better than the old way!**

1. Install your distro's lutris package, for Arch Linux;`sudo pacman -S lutris` **DO NOT USE THE FLATPACK VERSION AS THIS IS UNTESTED** 

2. Install Lutris' needed Wine dependencies, see [the lutris documentation for Arch](https://github.com/lutris/docs/commit/898ca03715d2d5f170af83e714dfcc549820aa9f)

**NOTE:** *Lutris changed their distribution dependencies page recently to say, just use proton-ge. This is not how I had setup my system when I had tested and got it working. Above, I am linking back to the previous commit before this change. To see the information in a readable view, change the view from RAW to Markdown in the WYSIWYG menu at the top of the file.*

3. ### Install Ableton Live 12 via Lutris
    * Download Ableton Live 12 from your account downloads
    * Extract the zip file to your `~/Downloads` folder
    * In Lutris click on the `+` button in the top left corner
    * In the popup window click on `Install a Windows Game from an Executable`
    * Enter the name of the application `AbletonLive12` in the field for `Game Name`
    * Leave the Installer preset and Locale as default `Windows 10` and `System`
    * Click the `Install` button in the top right
    * Click on the `Install` button for `Wine Setup File`
    * For `Select Installation Directory` set this to a sane place for your wine prefix **EXAMPLE:** `/home/$USER$/myWinePrefixes/abletonLive12`
    * Click `Continue`
    * When selecting the setup file, click the `...` and point it to where your Ableton Live 12 installer .exe is ***it should be in your downloads folder where you extracted it earlier***
    * Once the Installer.exe is selected, click `Install` in the top right
    * The Ableton Live 12 installer should appear, accept the terms and conditions, leave all options as default
    * Once Installation is complete, **DO NOT LAUNCH** when installer completes, hit **"Close"**
    * Lutris should now show finished installation in the log window. 
    * **Do not launch Ableton yet, we have more work to do!**

4. ### Installing Wine-TKG via ProtonUpQT
    * Install ProtonUpQT, either the appimage or the flatpak should be fine, I used the appimage. [See the ProtonUpQT GitHub for information on installation](https://github.com/DavidoTek/ProtonUp-Qt)
    * Open ProtonUpQT
    * From the `Install for` drop down, select Lutris
    * Click the `Add Version` button
    * For the `Compatability Tool` drop down, select `Wine TKG (Valve Wine Bleeding Edge)`
    * The version that I used on the date of getting this working `13895248208`, but you can ***probably*** use the latest version. If it doesn't work, fallback to the working version I just mentioned
    * Click the `Install` button
    * Wine TKG is now a runner in Lutris

5. ### Configuring Ableton Live 12 in Lutris
    * Back in Lutris, Select the Ableton Live tile and click the arrow pointing up next to the `Play` button
    * Select `Browse Files`
        * Copy `EXAMPLE_fixStartingMax.sh` to the folder that opens and then open the script with any text editor of your liking.
        * Replace `myBottles/abletonLive12` with the path to the prefix we created earlier **EXAMPLE** `myWinePrefixes/abletonLive12`
        * Save the script and exit your text editor
    * Back in Lutris, Right Click on the Ableton Live tile, click `configure`
    * Go to the `Game Options` tab and enable the Advanced toggle at the top right of the window
        * For `Executable` find and select the Ableton Live exe, it should be in the ProgramData folder in your wineprefix we made earlier, **EXAMPLE:** `/home/$USER/myWinePrefixes/abletonLive12/drive_c/ProgramData/Ableton/Live 12 Suite/Program/Ableton Live 12 Suite.exe`
        * For `Wine prefix` ensure that this is the prefix we created earlier **EXAMPLE** `/home/$USER/myWinePrefixes/abletonLive12`
    * Go to the `Runner options` tab
        * For `Wine Version` select the wine-tkg-valve-exp-bleeding that we installed with ProtonUpQT before **EXAMPLE** `wine-tkg-valve-exp-bleeding-9.0.174637-x86_64.pkg`
        * Scroll Down and turn off the following (we don't need these);
            * AMD FSR
            * BattleEye Anti-Cheat
            * Easy Anti-Cheat
        * Scroll down and enable `Windowed (virtual desktop)`. This is optional, however, it fixes issues related to drag and dropping in Ableton.
        * Scroll Down and for the `Audio Driver` drop down, select `ALSA`
    * Go to the `System options` tab
        * Scroll down to `Environment variables` and click on the `Add` button.
            * Set the key to `WINE_FULLSCREEN_INTEGER_SCALING`
            * Set the value to `1` 
        * Scroll down to `Pre-launch script`, find and select the script you just copied and modified **EXAMPLE** `/home/YOUR USER HERE/myWinePrefixes/abletonLive12/EXAMPLE_fixStartingMax.sh`

6. Ableton Live 12 should launch successfully

7. Press `F11` on your keyboard to make Ableton Live fullscreen if it isn't already (the app needs to be fullscreen because of drawing/scaling issues and missing menu bar in windowed mode)

8. Play the Demo Project to ensure that Ableton is working and playing audio

9. Now we need to quit out of Ableton for the next part where we install wine-asio into the wine-tkg-valve-exp build

10. ### Installing wineasio for low latency audio
    * Build wineasio yourself, follow the instructions from their [github page](https://github.com/wineasio/wineasio)
    * **NOTE:** ***make sure to recursively clone the repository: `git clone --recursive https://github.com/wineasio/wineasio`.***
    * **NOTE:** ***if you have a custom wine build (example: `wine-tkg`) installed as your system wine, `wineasio` needs `wine-staging` to be installed to compile successfully. After you finish compiling wineasio, you can reinstall your custom wine build.***
    * Once compiled, we need to copy the `wineasio64.dll` and `wineasio64.dll.so` into the wine-tkg-valve-exp runner
    * for Arch Linux
        * **NOTE:** ***for other linux distros the location of wine dll's may be different, consult your distro's wine documentation***
        * **NOTE:** ***the version of wine-tkg may be different if you installed a different version, use tab complete to help you, or check the directory manually***
        * `cd /usr/lib/wine/x86_64-windows`
        * `cp wineasio64.dll /home/$USER/.local/share/lutris/runners/wine/wine-tkg-valve-exp-bleeding-9.0.174637.20250316-327-x86_64.pkg/lib/wine/x86_64-windows/wineasio64.dll`
        * `cd /usr/lib/wine/x86_64-unix/`
        * `cp wineasio64.dll.so /home/$USER/.local/share/lutris/runners/wine/wine-tkg-valve-exp-bleeding-9.0.174637.20250316-327-x86_64.pkg/lib/wine/x86_64-unix/wineasio64.dll.so`
    * Once wineasio is installed, follow the instructions on their github page to register wineasio to the AbletonLive12 wineprefix we created earlier
        * **EXAMPLE** `env WINEPREFIX=/home/$USER/myWinePrefixes/abletonLive12 ./wineasio-register` 
        * You should get a confirmation dialogue that indicates that it was registered successfully
        * **NOTE:** ***if this didn't work, and you get an error message along the lines of `wine64 not found`, run this command instead:*** `WINEPREFIX=/home/$USER/myWinePrefixes/abletonLive12 wine regsvr32 /usr/lib/x86_64-windows/wineasio64.dll`
    * For wineasio to work properly:
        * Your user needs to be in the `realtime`, `audio` and `pipewire` groups
            * run `sudo usermod -aG audio $USER` and `sudo usermod -aG realtime $USER` and `sudo usermod -aG pipewire $USER`
            * **NOTE:** ***if any of these groups don't exist, run*** `sudo groupadd GROUP_GOES_HERE` 
        * Your audio output needs to be set to `Pro mode`:
            * For KDE Plasma, you can go to the `Sound` tab in the `System Settings`
            * Locate your current playback device (audio output) and set the profile to `Pro Audio`
        * Reboot your system
    * wineasio should now be working in Ableton Live
    * open Ableton Live 12, go to the settings, and select Wine ASIO from the settings
    * you won't be able to change the sample rate for Wine ASIO directly in the Ableton Live settings, we'll go over that in the Troubleshooting and work around section

11. You should now have Ableton Live 12 installed, working, and with Wine ASIO installed for low latency audio. If you have any problems with this, please consider visiting the [Ableton on Linux Discord server]()

## Workarounds and Troubleshooting

### Bypassing "Starting Max..." boot sequence Bug

*Credits to user dsnvs on the [discord](discord.gg/yM2Jjh8xYA)!*

There is something wrong with how Max 8's maxpreferences.maxpref file is being processed, so what I did is the following:

1. Create a shell script file called live12.sh and store it in a folder of your choice, then insert the following (replace yourusername with your actual machine's user name):

**NOTE:** *the location of your wineprefix will be different depending on where you set it up earlier in the instructions* **PLEASE CHANGE THE $WINEPREFIX LOCATION BEFORE SAYING THIS SCRIPT DOESNT WORK**
```
#!/bin/bash

rm "/home/$USER/$WINEPREFIX/drive_c/users/$USER/AppData/Roaming/Cycling '74/Max 8/Settings/maxpreferences.maxpref"
```

2. Make the script executable
`cd` to the location of the script,
run `chmod +x ./max4liveScript.sh`

3. Open Lutris

4. Right click on Ableton Live 12, click on `Configure`

5. Go to the `System Options` tab
    * Scroll down to `Pre-Launch Script`
    * Click on the three dots, and add the script we created above

6. Just below `Pre-Launch Script` turn on the slider for `Wait for pre-launch script completion`

7. This should now remove the maxpreferences.maxpref everytime we launch AbletonLive12, preventing Ableton from settings stuck on "Starting Max..."

### Ableton Crashes when selecting WineASIO in the audio settings
1. Add your user to the `audio`, `pipewire` and `realtime` groups, run these commands in your terminal.
    ```
    sudo usermod -aG audio $(whoami)
    sudo usermod -aG pipewire $(whoami)
    sudo usermod -aG realtime $(whoami)
    ```
2. Reboot the computer. 
3. This should solve the issue, if not, you may be missing a dependency. We are still trying to identify all of the dependencies needed, but here are some to try. **Note: Please report in the Ableton on Linux discord group, which dependency fixed the issue so that we can narrow down the needed dependencies**
    ```
    libwireplumber
    lib32-pipewire-jack
    libpulse
    wireplumber
    ```

### How do I change the wineasio sample rate?
1. Open Lutris
2. Select Ableton Live
3. In the bottom menu, next to the wineglass icon, click on the up arrow
4. Click on `Wine registry` in the menu
5. Navigate to `HKEY_CURRENT_USER\Software\Wine\WineASIO`
6. Double click on the `Prefered Buffer Size` attribute
7. Change the Base to Decimal
8. Enter the preffered sample rate; e.g. 64,128,256,512,etc...
9. Double click on the `Fixed Buffer Size` attribute
10. Set the value to `0` 
11. Click on Ok
12. Launch Ableton
13. If Ableton is already running, you have to turn off the audio engine and turn it back on
    * Open the options menu from the menu bar
    * Click on Audio Engine
    * Repeat the steps again to turn the audio engine back on
14. WineASIO should now be set to the sample rate you just change in wine registry

### Problems Noted So Far
1. Menu bar is not visible in windowed mode, can be accessed wth ALT+F and arrow keys
2. Program needs to be fullscreen, non fullscreen window cannot be controlled/resized and mouse location data is innacurate
3. Multiple Windows are a struggle because the program is fullscreen
4. Sometimes the program becomes uncontrollable, even in fullscreen mode, the work around is press ctrl + , to open settings and then close settings, then the fullscreen program becomes controllable again
5. Max for live doesn't work or works inconsistently, your millage may vary

### What hasn't been tested yet
1. **External plugins!** please try to use external plugins and report findings and solutions in the Discord. I would assume yabridge will work, but that is yet to be determined

### What if I run into a problem that isn't documented here?
* Please join the discord server and participate in conversation to try to figure out the problem with other members of the community! When we work together we all win!

## Additional Resources
### Custom Wine and Linux Kernel for Audio Production
If this is not sufficient and you want more of a challenge, there is a [custom Wine Version](https://github.com/nine7nine/Wine-NSPA) and [Linux Kernel](https://github.com/nine7nine/Linux-NSPA-pkgbuild) for Professional Audio on Arch Linux written by user nine7nine on github.The kernel and wine version are Realtime Capable Pro Audio optimized versions, and specifically written with Ableton Live on Linux in mind.
Current version is for Live 11 and 10, but nine7nine is currently rebasing to Wine10, and updating for Ableton Live 12
