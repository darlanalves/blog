---
title: ESP8266 Boot Mode numbers
---

When you power up your esp8266 a few numbers are printed to tell you in which mode the chip is right now.

If you can't read the text via UART, try changing your baud rate to `74880`.
Otherwise this is what you will see:

```
ets Jan  8 2013,rst cause:2, boot mode:(1,6)
```

Two things to note here: the **reset cause** and the **boot mode.**

From a [forum post](https://www.esp8266.com/viewtopic.php?p=2096#) I've got the following: 

```
reset causes:
    0: 
    1: normal boot
    2: reset pin
    3: software reset
    4: watchdog reset

boot source:
    0: maybe SD card? 
    1: ram
    3: flash
```

### Boot Mode

That's an easy one:
- if you see `boot mode:(1,x)`, that means the **esp8266** is waiting for a firmware flash
- if you see `boot mode:(3,x)` instead, your board is running your firmware.

The `x` can change, but that's not relevant to know the boot mode.

**TL;DR**: boot mode `1` means wait for flash, while `3` means run the sketch.

### Reset Cause

Usually it shows `2`, even if you unplug the power and plug again.
But can also be something else, as shown in the table above.
