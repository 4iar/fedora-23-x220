# Network

## Install drivers

not sure how different x220 wifi cards are

    dnf install iwl600g2a-firmware

rr or modprobe

# Monitor

## Drivers

Install drivers

    dnf install xorg-x11-drv-intel

## Backlight

Can use xbacklight for cli control and sxhkd 

Option 1: bind brightness keys to xbacklight -inc/-dec using sxhkd.
See http://blog.z3bra.org/2014/04/pop-it-up.html

Option 2 (lazy): install xfce4-power-manager and let it handle brightness shortcuts.
This seems more lightweight than the alternatives such as gnome-power-manager.

## Redshift (cli/open-source f.lux alternative)

Use this to automatically adjust colour temperature in order to prevent eyes from dying late at night.

Note that this doesn't work with Wayland yet, so no using sway or i3way :(

    dnf install redshift

Use geoclue2 to let redshift detect location 

    dnf install geoclue2

Instead, manually provide location through -l LAT:LON.
For example:
    redshift -l 40.1243:9.4040

# Power

## Power saving

Install tlp and powertop.
    dnf install tlp powertop

Check tlp-stat for suggestions and tlp status.
It recommends acpi-call - seems to be a debian/ubuntu package not dnf/fedora though.
     * Install acpi-call kernel module for ThinkPad advanced battery functions

## Suspend on close

Seemed to work without fiddling, but need to check if this was the doing of xfce4-power-manager

### Screen locking

Create a systemd service in /etc/systemd/system:

    [Unit]
    Description=Lock the screen upon suspend

    [Service]
    User=user
    Type=forking
    Environment=DISPLAY=:0
    ExecStart=/usr/bin/i3lock

    [Install]
    WantedBy=sleep.target

Enable the service: systemctl enable i3lock.service

Need to provide DISPLAY=:0 and set Type=forking otherwise i3lock doesn't seem to work.
Haven't tried other screen lockers such as xlock and slimlock.

Will need to take care if using multiple X sessions because DISPLAY is hardcoded, right?

Can also set i3lock colour:

    ExecStart=/usr/bin/i3lock -c 242424
