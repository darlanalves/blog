# Orange PI + 3D Printing over WiFi

I recently bought an [Ender Pro 3](https://www.aliexpress.com/item/32918302452.html?spm=a2g0s.9042311.0.0.5cb64c4dK88xrA) to learn more about 3D printing and eventually make all the plastic cases for my [HomeBots](https://github.com/homebots).

The first thing that bothered me is either swap around a TF Card or hook an USB cable for serial communication.

And you know that connecting and disconnecting a USB is too much work for a nerd, so why not hook my OrangePI to the side
of the printer and make it print over WIFI?

# What you're gonna need/use

- [Orange Pi](http://www.orangepi.org/orangepizero/) with [Armbian](https://www.armbian.com/orange-pi-pc/) installed on an SD Card
- USB Cable for your printer
- SSH with root access to your OPi
- Octoprint software from GitHub

Luckily for me, I already have **Armbian** running on OPi and I found [a quick tutorial on the webz](http://deloarts.com/en/3d-printing/octoprint-on-orange-pi-zero/) to do it.
We're gonna setup some packages, install PySerial and build [OctoPrint](https://octoprint.org/download/) to start our prints.

At the moment the stable version of OctoPrint is 1.3.12. Check the latest stable version [here](https://github.com/foosel/OctoPrint/releases)


**If you are like me and cannot read through a whole article here's a quick sequence of commands to do everything:**

_NOTE: I assume you are logged in as root here. If not, run "sudo sh" first_

```bash

apt-get install -y \
  python-pip \
  python-dev \
  git \
  virtualenv \
  python-setuptools \
  psmisc

adduser octoprint && \
  usermod -a -G tty octoprint && \
  usermod -a -G dialout octoprint &&\
  adduser octoprint sudo

echo "octoprint ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/octoprint && \
  chmod 0440 /etc/sudoers.d/octoprint

git clone https://github.com/foosel/OctoPrint.git /opt/octoprint && \
  cd /opt/octoprint && \
  git checkout 1.3.12 && \
  virtualenv venv && \
  ./venv/bin/python setup.py install

cp /opt/octoprint/scripts/octoprint.init /etc/init.d/octoprint && \
  chmod a+x /etc/init.d/octoprint && \
  cp /opt/octoprint/scripts/octoprint.default /etc/default/octoprint

echo "DAEMON=/opt/octoprint/venv/bin/octoprint" >> /etc/default/octoprint

echo "OCTOPRINT_USER=octoprint" >> /etc/default/octoprint

update-rc.d octoprint defaults
```

If everything went well, you can start OctoPrint now: `service octoprint start`

The server will be available at your machine's IP + ':5000', like so: `192.168.1.100:5000`

You can change the server port (and other configs) in `/etc/default/octoprint`. 
After saving your changes, run `update-rc.d octoprint defaults && service octoprint restart`

## Notes

- After changing the port sometimes Octoprint will get stuck. I just pulled the plug of OrangePI and put it back. 
After reboot it was back online :)

- If you have an Ender 3 Pro like me, here's the Octoprint profile for it:
```
Print bed & build volume:
  Form factor: Rectangular
  Origin: Lower Left
  Heated Bed: Yes
  Heated Chambere: No
  
  Width: 220mm
  Depth: 220mm
  Height: 250mm
  Custom Bounding Box: No
  
Axes -  use default values:
  6000, 6000, 200, 300

Hotend & extruder
  Nozzle Diameter: 0.4mm
  Number of Extruders: 1
```

Easy peasy, 3d-print squeezy!

## What now?

Do you know OctoPrint also has a bunch of plugins?

Yes! It does! And there's a bunch of them!

Here's my favourite: [exclude a region during print](https://plugins.octoprint.org/plugins/excluderegion/).

Take a look at this and other plugins [here](https://plugins.octoprint.org/by_date/).

Ciao!
