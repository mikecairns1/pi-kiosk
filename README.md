# Pi Kiosk

<p align="center"><img alt="Raspberry Pi Touch Display 2 with HA Kiosk" src="/resources/pi-touch-display-kiosk.jpeg" height="auto" width="600"></p>

This project contains the simplest form of a persistent Raspberry Pi-based browser kiosk. This was forked from Jeff Geerling's project "pi-kiosk. I love how simple this solution is. 

Kiosks are useful if you want to build a web dashboard for something and have the Pi display it, or have a custom web page or online video play automatically when your Pi boots up, and take over the full screen.

This configuration is not meant to be highly secure, or extremely robust, I just use it when I want to pop a web page up on one of my Pis full-screen, like for a Home Assistant control panel.

For my own Kiosk, I'm using the following hardware to display a Home Assistant dashboard:

  - Raspberry Pi 3b
  - Raspberry Pi 7 in Touch Display

## Setup

Ensure you're running Pi OS with a graphical interface (the 'full install'), and Chromium is installed (it should be, by default).

Then open up Terminal and install a prerequisite:

```
sudo apt install unclutter
```

Create the script that will run the Kiosk:

```
mkdir -p /home/mike/kiosk
cp kiosk.sh /home/mike/kiosk/kiosk.sh
```

Copy over the SystemD unit file to run the `kiosk` service:

```
sudo cp kiosk.service /lib/systemd/system/kiosk.service
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

I ran into conflicts in services that I needed to resolve, so I had to find a previous service that was conflicting with my display.

```
sudo systemctl --all list-unit-files --type=service
```

I then found a service with status of "bad" that I then removed

```
sudo systemctl --all list-unit-files --type=service --status=bad
```

Otherwise, if you have access to the Pi itself, and can plug in a keyboard, you can press `Ctrl` + `F4`, and that _should_ quit the browser.

Another alternative is to set up the Pi with [Pi Connect](https://www.raspberrypi.com/documentation/services/connect.html), and when connected to the Pi remotely, press `Ctrl` + `F4`.


## License

GPL v3

## Author

This was forked from Jeff Geerling's project "pi-kiosk" so that I could maintain my own updates if needed. This project was created by Jeff Geerling in 2024, loosely following this guide from PiMyLifeUp: [Raspberry Pi Kiosk using Chromium](https://pimylifeup.com/raspberry-pi-kiosk/).
