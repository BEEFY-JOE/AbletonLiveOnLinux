# AbletonLiveOnLinux
A Repository for All Things Related to Running Ableton Live on Linux, part of the newly formed [Ableton on Linux Discord Group](discord.gg/yM2Jjh8xYA).

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
1. `sudo pacman -S lutris` **DO NOT USE THE FLATPACK VERSION AS THIS IS UNTESTED**

2. Install Lutris' needed Wine dependencies, see [the lutris documentation for Arch](https://github.com/lutris/docs/blob/master/WineDependencies.md#archendeavourosmanjaroother-arch-derivatives)

3. ### Install Ableton Live 12 via Lutris
    * Download Ableton Live 12 from your account downloads
    * Extract the zip file to your `~/Downloads` folder
    * In Lutris click on the `+` button in the top left corner
    * In the popup window click on `Install a Windows Game from an Executable`
    * Enter the name of the application `AbletonLive12` in the field for `Game Name`
    * Leave the Installer preset and Locale as default `Windows 10` and `System`
    * Click the `Install` button in the top right
    * Click on the `Install` button for `Wine Setup File`
    * For `Select Installation Directory` set this to a sane place for your WinePrefix, **EXAMPLE:** `/home/$USER$/myWinePrefixes/abletonLive12`
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
    * Back in Lutris Right Click on the Ableton Live tile, click `configure`
    * Go to the `Game Options` tab
        * For `Executable` find and select the Ableton Live exe, it should be in the ProgramData folder in your wineprefix we made earlier, **EXAMPLE:** `/home/$USER/myBottles/abletonLive12/drive_c/ProgramData/Ableton/Live 12 Suite/Program/Ableton Live 12 Suite.exe`
        * For `Wine prefix` ensure that this is the prefix we created earlier **EXAMPLE** `/home/$USER/myBottles/abletonLive12`
    * Go to the `Runner options` tab
        * For `Wine Version` select the wine-tkg-valve-exp-bleeding that we installed with ProtonUpQT before **EXAMPLE** `wine-tkg-valve-exp-bleeding-9.0.174637-x86_64.pkg`
        * Scroll Down and turn off the following (we don't need these);
            * AMD FSR
            * BattleEye Anti-Cheat
            * Easy Anti-Cheat
        * Scroll Down and for the `Audio Driver` drop down, select `ALSA`
    * Go to the ``

6. Ableton Live 12 should launch successfully

7. Press `F11` on your keyboard to make Ableton Live fullscreen (the app needs to be fullscreen because of drawing/scaling issues and missing menu bar in windowed mode)

8. Play the Demo Project to ensure that Ableton is working and playing audio

9. Now we need to quit out of Ableton for the next part where we install wine-asio into the wine-tkg-valve-exp build

10. ### Installing wineasio for low latency audio
    * Build wineasio yourself, follow the instructions from their [github page](https://github.com/wineasio/wineasio)
    * Once wineasio is built, follow the instructions on their github page for installing wineasio to your system
    * Once wineasio is installed, follow the instrcutions on their github page to register wineasio to the AbletonLive12 wineprefix we created earlier
        * **EXAMPLE** `env WINEPREFIX=/home/$USER/myBottles/abletonLive12 ./wineasio-register` 
        * You should get a confirmation dialogue that indicates that it was registered successfully
    * Once registered, we need to copy the `wineasio64.dll` and `wineasio64.dll.so` into the wine-tkg-valve-exp runner
    * for Arch Linux

        **NOTE:** ***for other linux distros the location of wine dll's may be different, consult your distro's wine documentation***

        **NOTE:** ***the version of wine-tkg may be different if you installed a different version, use tab complete to help you, or check the directory manually***
        * `cd /usr/lib/wine/x86_64-windows`
        * `cp wineasio64.dll /home/joe/.local/share/lutris/runners/wine/wine-tkg-valve-exp-bleeding-9.0.174637.20250316-327-x86_64.pkg/lib/wine/x86_64-windows/wineasio64.dll`
        * `cd /usr/lib/wine/x86_64-unix/`
        * `cp wineasio64.dll.so /home/joe/.local/share/lutris/runners/wine/wine-tkg-valve-exp-bleeding-9.0.174637.20250316-327-x86_64.pkg/lib/wine/x86_64-unix/wineasio64.dll.so`
    * wineasio should now be working in ableton live
    * open Ableton Live 12, go to the settings, and select Wine ASIO from the settings
    * you won't be able to change the sample rate for Wine ASIO directly in the Ableton Live settings, we'll go over that in the Troubleshooting and work around section

11. You should now have Ableton Live 12 installed, working, and with Wine ASIO installed for low latency audio. If you have any problems with this, please consider visiting the [Ableton on Linux Discord server]()

## Workarounds and Troubleshooting

### Bypassing "Starting Max..." boot sequence Bug

*Credits to user dsnvs on the [discord](discord.gg/yM2Jjh8xYA)!*

There is something wrong with how Max 8's maxpreferences.maxpref file is being processed, so what I did is the following:

1. create a shell script file called live12.sh and store it in a folder of your choice, then insert the following (replace yourusername with your actual machine's user name):
```
#!/bin/bash

rm "/home/yourusername/.wine/drive_c/users/yourusername/AppData/Roaming/Cycling '74/Max 8/Settings/maxpreferences.maxpref"
cd "/home/yourusername/.wine/drive_c/ProgramData/Ableton/Live 12 Suite/Program/"
wine "Ableton Live 12 Suite.exe"

wineserver -k
```

2. create a new .desktop entry in `~/local/share/applications/wine/Programs` called `live12.desktop` and insert the following (point the Exec to where your `live12.sh` file is / the icon is optional):
```
[Desktop Entry]
Name=Ableton Live 12 Suite
Exec=/path/to/live12.sh
Type=Application
StartupNotify=true
Icon=/path/to/live12.svg
```

3. log out and log back in your machine and launch Live 12 Suite

### Problems Noted So Far
1. Severe CPU Penalty, out of the box Live 12 is using anywhere from 10-20% CPU not doing anything, the demo project sometimes exceeds 60% CPU usage with 256 quantum in pipewire, 128 quantum results in missed audio packets, exceeding 100% on demo project.
2. Menu bar is not visible, can be accessed wth ALT+F and arrow keys
3. Program needs to be fullscreen, non fullscreen window cannot be controlled/resized
4. Multiple Windows very inconvenient to use inside of Wine Virtual Desktop

## Additional Resources
### Custom Wine and Linux Kernel for Audio Production
There is a [custom Wine Version](https://github.com/nine7nine/Wine-NSPA) and [Linux Kernel](https://github.com/nine7nine/Linux-NSPA-pkgbuild) for Professional Audio on Arch Linux written by user nine7nine on github.The kernel and wine version are Realtime Capable Pro Audio optimized versions, and specifically written with Ableton Live on Linux in mind.