# Project

This project is composed of: 

- a remote controlled car build with the [Makeblock Ultimate Kit](https://www.makeblock.com/steam-kits/mbot-ultimate)
- a PS3 controller, controlling the car using bluetooth
- some form of obstacle detection (tbd)
- some form of collision detection (tbd)
- FreeRTOS

## Rationale

I am building this project to learn more about embedded development, real time operating systems and to brush up my C/C++ skills. In order to do this, I will try to keep the use of external librairies as low as possible.

Using this page, I will also try to describe everything I do as much as possible.

## Content

[Development Environment](environment.md)

## links

- project repo: https://github.com/alexcetto/remote-car-mega2560
- https://github.com/felis/USB_Host_Shield_2.0/wiki/PS3-Information
- https://github.com/felis/USB_Host_Shield_2.0/wiki/Bluetooth-dongles
- http://ww1.microchip.com/downloads/en/DeviceDoc/Atmel-2549-8-bit-AVR-Microcontroller-ATmega640-1280-1281-2560-2561_datasheet.pdf
- https://github.com/pravdomil/AVR-Instruction-Set/blob/master/README.md
- https://www.microchip.com/wwwproducts/en/ATmega2560
- bluetooth adapter. https://fccid.io/2ACWW1300133B
- https://kevinfundarek.wordpress.com/2015/11/10/programming-the-atmega-2560-with-the-arduino-ide/
- https://www.obdev.at/products/crosspack/index.html
- Nice little project (unrelated): https://blog.tomarrell.com/post/rust_and_leds
- FreeRTOS project: https://feilipu.me/2015/11/24/arduino_freertos/
