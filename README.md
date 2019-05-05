# ScreenPi

ScreenPi is a small project to run a video player on a Raspberry Pi, mounted in a wall plug frame using a Waveshare 3.5" LCD Display https://www.waveshare.com/wiki/3.5inch_RPi_LCD_(A)

## Getting Started

These instructions will get you a copy of the project up and running.

### Prerequisites

Install a clean raspberry from scratch by downloading the latest raspian full image and flashing it to an SD card. The LCD driver does not work with lite (event though waveshare claims it does with a special parameter "lite", does not work with current versions). Regarding the hardware, it is recommended to use one of the more powerful Raspberries, they cost only little more but have much more reserves in their computing power, e.g the A+ or B+. The A+ is a very good choice as it is nice and small to fit well behind the socket.

After flashing it but before booting it up, add a wpa_supplicant.conf with your WiFi details and an empty ssh file (to enable telnet on it) before you boot it up.

Upon the first boot, change the root password,  configure it, and upgrade it to the latest and install git:
```
passwd
sudo apt-get -y update && sudo apt-get -y upgrade
sudo raspi-config
```
In raspi-config, set the GPU memory to 256 MB to boost up the GPU performance (under advanced options), give it a nice hostname - e.g. 'screenpi' and set your timezone + locales correctly. Also, set the boot up option to "splash screen" so it looks tidier when it boots up.

If you have never done the above steps before, google around for more details on each step.

### Installing

Now pull the installation script to the pi:
```
cd /tmp
wget https://raw.githubusercontent.com/dpsimon/screenpi/master/install.sh
```
Now prepare and execute the installation script:

```
sudo chmod a+x install.sh
sudo ./install.sh
```
Attention, the script overwrites some system config files. Even though it creates backups in the process: If you are installing this to a Raspi Pi with more stuff already on it, better do the steps manually. In that case check out the script and enter it one by one / edit the config files manually. Also bear in mind, even if you do that - the waveshare driver is also agressively and overwrites config files, so pay attention before applying it to an existing system, too.

## Configuring the stream

Edit the configuration file 
```
sudo nano /home/pi/screenpi/screen-api-conf.py
```
to configure your stream and the parameters. To optimally configure the parameters of your omxplayer, check out especially its win, crop and aspect-ratio parameters: [omxplayer](https://github.com/huceke/omxplayer/)

## Using the interface

After installing & rebooting the pi, you can switch the screen on and off by calling the API:
```
curl http://screenpi:5000/screen/on
curl http://screenpi:5000/screen/off
curl http://screenpi:5000/screen/timer?duration=120
```

### Integrating it into your home automation

You can call the API to start up the screen from within your favorite home automation, e.g. in a node.js script 
```
var request     = require('request');  // http requests to set outbound webhooks
request.get("http://screen:5000/screen/timer?duration=300");
```

## Built With

* [omxplayer](https://github.com/huceke/omxplayer/) - Video Player
* [flask](https://github.com/pallets/flask) - API
* [Waveshare LCD](https://github.com/waveshare/LCD-show) - Waveshare LCD driver
**Manual for config.txt for more options: https://www.raspberrypi.org/documentation/configuration/config-txt/video.md  
**WIKI for the LCD: https://www.waveshare.com/wiki/3.5inch_RPi_LCD_(A)

## Ideas for further improvements

The screen currently just blanks when not used, as this particular Waveshare LCD does not support backlight power-off. There are hardware-tweaks, but if someone finds a simple solution (e.g. cheap LCD with power-off with same dimensions), this would be very interesting.
The omxplayer is currently hard killed with a killall. All other solutions did not work reliably (timing the playtime, handing a process ID to the python script and killing only that process). Again a cleaner solution would be great. 
The API is very barebone and does not feedback nice responses, also its behaviour in the timer mode, sending the response only after the time is up could be improved.

## Contributing

Pull requests welcome. The project has been done in some spare time on the side, please do understand if it takes a while for me to respond.

## Authors

* **Simon R** - *Initial work* - [dpsimon](https://github.com/dpsimon)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details