# AbletonLiveOnLinux
A Repository for All Things Related to Running Ableton Live on Linux, part of the newly formed [Ableton on Linux Discord Group](discord.gg/yM2Jjh8xYA).

## Live 12
### Install Documentation (WIP)
#### Enviroment Tested
* Arch Linux Stock Kernel 6.12.9
* wine-staging 9.22-1
* winetricks 20250102 - sha256sum: 53194dead910f8a5eb1deacaa4773d4e48f5873633d18ab1ecd6fdb0cb92243b
* wine-mono 9.3.0-1
* Ableton Live 12.1.5

#### Instructions
1. Create custom wineprefix to separate Ableton from default wine prefix. 
    * WINEPREFIX=/path/to/new/custom/prefix wineboot --init
    * WINEPREFIX=/path/to/new/custom/prefix winecfg
2. Install winetricks
    * WINEPREFIX=/path/to/new/custom/prefix winetricks
    * In the GUI select install components
    * Find vcrun2022 and dxvk from the list and install them
3. Install Ableton Live 12
    * WINEPREFIX=/path/to/new/custom/prefix wine /path/to/ableton_installer.exe
    * Leave all options default and proceed through installation
    * Once Installation is complete, **DO NOT LAUNCH** when installer completes, hit **"Close"**
4. Modify winecfg Settings
    * WINEPREFIX=/path/to/new/custom/prefix winecfg
    * Under graphics tab, enable Emulate virtual desktop, set the desktop size to something that will fit on your primary display.
    * Under graphics tab, Uncheck the box for "Allow the window manager to decorate the windows"
    * Under graphics tab, Uncheck box for "Allow the window manager to control the windows"
    * Under Input tab, Check the box for "Automatically capture the mouse in full-screen windows"
    * Under the staging tab, uncheck the box for "Enable CSMT for better graphic performance (deprecated)
    * Click the "Ok" button to close winecfg
5. Run Ableton Live 12 in the custom wineprefix
    * WINEPREFIX=/path/to/new/custom/prefix wine /path/to/new/custom/prefix/rive_c/ProgramData/Ableton/Live\ 12\ Suite/Program/Ableton\ Live\ 12\ Suite.exe
6. Ableton Live 12 should launch successfully and not have drawing issues on screen.
7. Run the demo project to verify live 12 is working as expected, this should be successfully playing audio through pulseAudio

## Workarounds and Troubleshooting
### Bypassing "Starting Max..." boot sequence Bug
***Note:*** This bug no longer seems to be relavant for versions starting at 12.1.5, this bug was observed in versions <=12.1.1

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

## Additional Resources
### Custom Wine and Linux Kernel for Audio Production
There is a [custom Wine Version](https://github.com/nine7nine/Wine-NSPA) and [Linux Kernel](https://github.com/nine7nine/Linux-NSPA-pkgbuild) for Professional Audio on Arch Linux written by user nine7nine on github.The kernel and wine version are Realtime Capable Pro Audio optimized versions, and specifically written with Ableton Live on Linux in mind.