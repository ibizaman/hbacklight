# hbacklight [![Build Status](https://api.travis-ci.org/paroxayte/hbacklight.svg?branch=master)](https://travis-ci.org/paroxayte/hbacklight)
Backlight controller for Linux systems

## Installation
```
git clone https://github.com/paroxayte/hbacklight.git
cd hbacklight
cabal install
```

Alternatively, an aur package is available and can be installed with an aur-helper. Ex: (with yay)

**note:** ghc-8.6.5 is an unlisted build dependency of the aur package. See PKGBUILD for more information.
```
yay -S hbacklight-git
```

In order to use hbacklight without sudo, you may need to create a udev rule and ensure you are part of the group granted permissions. EG:
```
» cat /etc/udev/rules.d/10-backlight.rules
ACTION=="add", SUBSYSTEM=="backlight", KERNEL=="intel_backlight", RUN+="/bin/chgrp video /sys/class/backlight/%k/brightness"
ACTION=="add", SUBSYSTEM=="backlight", KERNEL=="intel_backlight", RUN+="/bin/chmod g+w /sys/class/backlight/%k/brightness"
» cat /etc/udev/rules.d/10-kbd_backlight.rules
ACTION=="add", SUBSYSTEM=="leds", KERNEL=="dell::kbd_backlight", RUN+="/bin/chgrp video /sys/class/leds/%k/brightness"
ACTION=="add", SUBSYSTEM=="leds", KERNEL=="dell::kbd_backlight", RUN+="/bin/chmod g+w /sys/class/leds/%k/brightness"
```
## Usage
```
Usage: hbacklight ((-e|--enum) | [-l|--led] (-i|--id TARGET) [-v|--verbose]
                    [-d|--delta [+,-,%,~]AMOUNT] [-f|--floor FLOOR])
  Adjust device brightness

Available options:
  -h,--help                Show this help text
  -e,--enum                List available devices
  -l,--led                 Target led device
  -i,--id TARGET           Identifier of backlight backlight
  -v,--verbose             Informative summary backlight backlight state
  -d,--delta [+,-,%,~]AMOUNT
                           Modify the backlight value, ~ sets the value to
                           AMOUNT, elseshift is relative. Defaults to ~
  -f,--floor FLOOR         Set the minimum value which the brightness may be
                           assigned. (default: 1)
```

## Examples
```
» hbacklight -vi intel_backlight
device            intel_backlight
bl_power                        0
brightness                    200
actual_brightness             200
max_brightness               7500
type                          raw

» hbacklight -vli dell::kbd_backlight
led            dell::kbd_backlight
brightness                       2
max_brightness                   2

» hbacklight -i intel_backlight -d 1000

# turns the screen off
» hbacklight -i intel_backlight -d 0 -f 0
```
