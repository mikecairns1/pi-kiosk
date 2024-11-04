# Pi Kiosk

This project contains the simplest form of a persistent Raspberry Pi-based browser kiosk.

Kiosks are useful if you want to build a web dashboard for something and have the Pi display it, or have a custom web page or online video play automatically when your Pi boots up, and take over the full screen.

This configuration is not meant to be highly secure, or extremely robust, I just use it when I want to pop a web page up on one of my Pis full-screen, like for a Home Assistant control panel.

## Setup

Ensure you're running Pi OS with a graphical interface (the 'full install'), and Chromium is installed (it should be, by default).

Then open up Terminal and install a prerequisite:

```
sudo apt install unclutter
```

Create the script that will run the Kiosk:

```
mkdir -p /home/pi/kiosk
cp kiosk.sh /home/pi/kiosk/kiosk.sh
```

Copy over the SystemD unit file to run the `kiosk` service:

```
cp kiosk.service /lib/systemd/system/kiosk.service
```

Enable the Systemd `kiosk` service so it will automatically run at system boot:

```
sudo systemctl enable kiosk.service
```

(Optional) Fire up the `kiosk` service immediately:

```
sudo systemctl start kiosk
```

## Debugging

You can view the `kiosk` service logs by running the command:

```
journalctl -u kiosk
```

(Follow the logs live by appending `-f` to that command.)

## Stopping the service

If you can log in via SSH in the background, you can stop the service with:

```
sudo systemctl stop kiosk
```

Otherwise, if you have access to the Pi itself, and can plug in a keyboard, you can press `Ctrl` + `F4`, and that _should_ quit the browser.

Another alternative is to set up the Pi with [Pi Connect](https://www.raspberrypi.com/documentation/services/connect.html), and when connected to the Pi remotely, press `Ctrl` + `F4`.

> Note: On a Mac, you need to press `Fn` + `Ctrl` + `F4`.

If you want to get _really_ fancy, you can [use something like `xdotool`](https://unix.stackexchange.com/a/703023/16194) and wire up a button to emulate keypresses to quit the browser.

## Author

This project was created by Jeff Geerling in 2024, loosely following this guide from PiMyLifeUp: [Raspberry Pi Kiosk using Chromium](https://pimylifeup.com/raspberry-pi-kiosk/).
