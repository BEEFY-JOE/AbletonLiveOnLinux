# AbletonLiveOnLinux
A Repository for All Things Related to Running Ableton Live on Linux, part of the newly formed Ableton on Linux Discord Group.

## Live 12
### Bypassing "Starting Max..." boot sequence Bug
*Credits to user dsnvs on the discord!*

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

### Custom Wine and Linux Kernel for Audio Production

There is a [custom Wine Version](https://github.com/nine7nine/Wine-NSPA) and [Linux Kernel](https://github.com/nine7nine/Linux-NSPA-pkgbuild) for Professional Audio on Arch Linux written by user nine7nine on github.
The kernel and wine version are Realtime Capable Pro Audio optimized versions, and specifically written with Ableton Live on Linux in mind. 