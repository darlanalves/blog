## Ender 3 Pro and BLTouch 3.1

So I've got a BLTouch from AliExpress to improve my 3d prints.

This is a journal of what I had to do in order to get it working.

>
> **Disclaimer**: if you don't wanna mess around with soldering, just get the Creality official kit for BLTouch.
> It comes with additional wires and plugs.
>

## The parts

- [BLTouch 3.1](https://www.aliexpress.com/item/32777786433.html?spm=a2g0s.9042311.0.0.27424c4djPjkd6)
- 5 wires of ~1m to extend the BLTouch connection to the printer board. I [use these](https://www.aliexpress.com/item/32909241627.html?spm=a2g0s.9042311.0.0.27424c4dZULoqG).
- An Arduino (Mega, Uno, Nano or any other [with an ISP header](https://www.arduino.cc/en/uploads/Tutorial/Uno_Connect.jpg)) if you have a Ender 3 with Melzi 1.1.4 or below

## The tools

- Soldering iron
- Tweezers
- Ruler or calipers
- Phillips screwdriver

## The printed mods

## The software

- Marlin [2.0.x with bugfixes](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x) (see [this reddit thread](https://www.reddit.com/r/ender3/comments/b59soj/psa_new_bltouch_v30_will_not_work_with_marlin_11x/))
- Arduino IDE
