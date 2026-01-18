# Raspberry Pi - Power on and Shutdown button HAT

## Mini hut with a push button to **power on** the Rasperry Pi also **power off** after the shutdown sequence finish. 

## The HUT:

<p float="left">
<img src="Docs/front1.jpeg" width="45%">
<img src="Docs/Rasp_hut.jpeg" width="45%">
</p>


- USB C power connector. Use only this connector to power the Raspberry PI.
- Push button:
  - When the Raspberry Pi is off, a button press powers it on.
  - When the Raspberry Pi is on, a button press initiates the shutdown sequence, and when it completes cuts the power off. It's not necessary an additinal swich in the usb cable. After shutdown the power is completely off.
- 5x2 socket to plug into the Raspberry Pi pins header.  It provides access to the GPIO2, GPIO3 and power pins. 

<p float="left">
<img src="Docs/hut_render.jpg" width="45%">
<img src="Docs/back1.jpeg" width="45%">
</p>

---
## The circuit 
Basic power latch circuit combined with the ***shutdown*** and ***poweroff*** GPIO's to manage the power off sequence.

## GPIO pins:

### GPIO 2: Shutdown (gpio-shutdown) 
- This overlay allows you to initiate a graceful shutdown by shorting a GPIO pin to Ground.
- Behavior: Adding dtoverlay=gpio-shutdown to your config file enables GPIO 2 as the shutdown trigger.
- Configuration: Add this line to your config file:
    
    ``dtoverlay=gpio-shutdown,gpio_pin=2,active_low=1,gpio_pull=up``

### GPIO 3: Power-Off Signaling (gpio-poweroff)
- This overlay is used to signal an external power circuit (like this HUT) to cut the main power once the Pi has safely finished its shutdown sequence. 
- Behavior: The pin will transition to its active state only after the kernel has finished shutting down, ensuring no SD card corruption occurs 
- Configuration: Add this line to your config file:
    
    ``dtoverlay=gpio-poweroff,gpiopin=3,active_low=1``

    active_low=1: The pin is HIGH (3.3V) while the Pi is running and transitions to LOW (0V) once it is safe to cut power

 
### Config file Location
Configure the behaviour of the GPIO pins in the Raspberry Pi config file:

- Raspberry Pi OS Bookworm (and newer): The file is located at /boot/firmware/config.txt.
- Older OS Versions (Bullseye and earlier): The file is located at /boot/config.txt.
- On another computer: If you insert the SD card into a PC or Mac, the file is located in the root directory of the small partition named boot or bootfs.

For example:

```bash
[all]
# Add your shutdown/poweroff overlays here
dtoverlay=gpio-shutdown,gpio_pin=2,active_low=1,gpio_pull=up
dtoverlay=gpio-poweroff,gpiopin=3,active_low=1
```

<img src="Docs/Rasp_hut2.jpeg" width="60%">





