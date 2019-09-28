# Orange PI + 3D Printing over WiFi

I recently bought an Ender Pro 3 to learn more about 3D printing and eventually make all the plastic cases for my HomeBots.

The first thing that bothered me is either swap around a TF Card or hook an USB cable for serial communication.

And you know that connecting and disconnecting a USB is too much work for a nerd, so why not hook my OrangePI to the side
of the printer and make it print over WIFI?

Luckily for me, I already have **Armbian** running on OPi and I found [a quick tutorial on the webz](http://deloarts.com/en/3d-printing/octoprint-on-orange-pi-zero/) to do it.
We're gonna setup some packages, install PySerial and build [OctoPrint](https://octoprint.org/download/) to start our prints.

At the moment the stable version of OctoPrint is 1.3.11. Check the latest stable version [here](https://github.com/foosel/OctoPrint/releases)

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
  git checkout 1.3.11 && \
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

Easy peasy, 3d-print squeezy!

Ciao.
