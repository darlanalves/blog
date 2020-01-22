# esp8266 interrupts

One very important piece of information [I came across](https://bbs.espressif.com/viewtopic.php?t=63) today:

> All the GPIOs share a same interrupt source. In the interrupt handler read the GPIO_STATUS_ADDRESS register to decide which GPIO triggered the interrupt signal.
> e.g. `uint32 gpio_status = GPIO_REG_READ(GPIO_STATUS_ADDRESS);`
