# Development environment

## Board and microprocesseur:

I will be using one (or maybe two, depending on how many inputs I need) Atmel Mega2560. They are implemented on a controller board Elegoo. 

To program the board, you can either use the embedded USB connection on the board or you can use an ISP programmer. The programmer method has the advantage of being usable on another processor on a development kit. (like the AVR STK500)

## For the dev machine

It is possible to use the Arduino IDE and their .ino files to develop on an AVR microprocessor. However, the interface is not great and it hides a lot of steps that I would like to fully understand. For example, the compilation process, the firmware upload, etc...

**WIP** So I will be using, on an Ubuntu machine: 

- avr-gcc for the compilation
- avrdude to upload on the board

The exact commands I am using will be published later.
