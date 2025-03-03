mocoLUFA fork (MIDI firmware for Arduino Uno)
======================

This is a detailed step-by-step usage of mocoLUFA by kuwatay.

Generating the firmware HEX file:

1. Clone this project locally.
2. Open a terminal and navigate to Projects/mocolufa.
3. Edit Descriptors.c file if needed: ProductString (MIDI device name), Manufacturer, ProductID.
4. Open makefile file and verify that ARDUINO_MODEL_PID is set correctly for your Arduino model (UNO or MEGA).
5. Run "make clean".
6. Run "make".

Upgrading the firmware using the one we just generated:

1. Install dfu-programmer: "sudo apt-get install dfu-programmer".
2. Connect the Arduino to Your Computer With a USB Cable.
3. Reset the 8u2 or 16u2. To do this, briefly bridge the reset pin with the ground. The pins are located near the USB connector. Connect them briefly with a piece of wire.
4. Run "sudo dfu-programmer atmega16u2 erase"
5. Run "sudo dfu-programmer atmega16u2 flash dualMoco.hex". Must be done on directory Projects/mocolufa, where we generated the new HEX file.
6. Run "sudo dfu-programmer atmega16u2 reset"
7. Unplug Your Board and Plug It Back In.
8. Optional: run "make clean" again if you want to cleanup the generated files.

Now the dual boot is done. When connecting the Arduino: No bridged pins -> MIDI device. Bridge between ground pin and the first one next to the right -> Regular Arduino edit mode. (See arduino-pins-dual-boot-modes.png. Credit: Daniel Marcial).

Useful debug terminal commands:

amidi -l // list midi devices
lsusb // list usb devices

Docs: https://docs.arduino.cc/hacking/software/DFUProgramming8U2

======================
dualMocoLUFA Project
Copyright (C) 2013,2014,2015 by morecat_lab

2015/04/11
   
http://morecatlab.akiba.coocan.jp/lab/index.php/aruino/midi-firmware-for-arduino-uno-moco/?lang=en
  
based on LUFA-100807  

This is dual mode firmware for Arduino Uno.  
There are two mode on this firmware, USB-MIDI(MocoLUFA) and Arduino-Serial.  

INSTRUCTIONS  
1. Burn 16u2 on Arduino Uno.  
   check original document below.  
2. USB-MIDI formware work as default.  
3. To enable Arduino-Serial, ass jumper to PIN 4(MOSI PB2) and PIN6 (grand) on ICSP connector for 16U2.  
   Reset is required to swicth the firmware mode.  
  
-Yoshi  
  
-------------------------------------
original readme.txt from Arduino-Serial  
  
To setup the project and upload the Arduino usbserial application firmware to an ATMEGA16U2 using the Arduino USB DFU bootloader:  
	1. unpack the source into LUFA's Projects directory  
	2. set ARDUINO_MODEL_PID in the makefile as appropriate  
	3. do "make clean; make"  
	4. put the 16U2 into USB DFU mode:  
	4.a. assert and hold the 16U2's RESET line  
	4.b. assert and hold the 16U2's HWB line  
	4.c. release the 16U2's RESET line  
	4.d. release the 16U2's HWB line  
	5. confirm that the board enumerates as either "Arduino Uno DFU" or "Arduino Mega 2560 DFU"  
	6. do "make dfu" (OS X or Linux - dfu-programmer must be installed first) or "make flip" (Windows - Flip must be installed first)  

Check that the board enumerates as either "Arduino Uno" or "Arduino Mega 2560".  Test by uploading a new Arduino sketch from the Arduino IDE.
