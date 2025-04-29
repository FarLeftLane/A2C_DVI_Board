# A2C_DVI_Board

This is an adapter to let an Apple IIc use an A2DVI card. 4/29/25

**USE AT YOUR OWN RISK.   MIT LICENSE**

This has only been tested with an A2DVI v2 card and a 8BitDevices A2DVI V3 DVI/HDMI card ( https://www.8bitdevices.com/product/a2dvi/ ).  I don't think the firmware will work on a v4 card, I have one to try.

**BACKGROUND**

I have been working on a project using an ESP32-C6 to take the digital video signals from the Apple IIc DB15 video connector and allow remote viewing of the screen on a PC (watch this account for the ViewIIc plans).   The circuit design was pretty simple, a DB15 connector, a 74VLC245 for level conversion (5V to 3.3V) and a ESP32-C6 module.   It occurred to me that I could take those same Apple IIc signals and using the picoDVI libraries to create an Apple IIc to HDMI solution using an RP2040 or RP2350 (Raspberry Pi Pico).   This is the same HW that is on an A2DVI card for a slotted Apple II (RP2040 and 245's).  I built a Protype ( [https://github.com/FarLeftLane/A2C_DVI_Prototype](url) ) that takes the A2DVI card out of a slotted Apple II and instead of connecting to Apple II bus signals (address, data, etc.), we feed the Apple IIc digital video signals into the cards pins and use custom firmware to recreate the display out to HDMI.  This project is a design and PCB for a "production" version of the prototype.

**SCHEMATIC**

[https://github.com/FarLeftLane/A2C_DVI_Board/blob/main/Schematic.pdf](url)

**ASSEMBLY**

Everything is through-hole, so it is very approachable.  A male 90-deg DB15 connector is used to connect to the Apple IIc.  This connector has to be slightly modified by replacing the male nut screws with two 4/40 3/8" flat head screws you can get at ACE Hardware.  The board has a 50-pin slot for the A2DVI card to plug into, you need to trim the four plastic "feet" off the corners so that it sits flush.  When you solder it in, you don't need to do all 50 pins.  There are white lines near the pins that are required and I would do a few more for structural stability.  The board also has a button/resistors to access the config menu and a 7805 regulator/capacitors to convert +12V from the DB15 to +5V for the card.  You can power the card from a USB connection, but you need a +5V supply to make the config button work.  The +5V pin on the A2DVI card is protected by a diode, so it doesn't produce +5V when you are plugged into USB.  Follow the schematic on how to connect the 7805 and the 2 capacitors to the +12V line.  Solder in the button and the two resistors.  There is a 6 pin header (2x3) that is optional for some additional signals as well as the botton signals for a remore button.  8 items, pretty easy.

C1 = 0.33uF
C2 = 0.1uF
R1 = 10K
R2 = 1K

**SIGNALS**

These are the signals that are routed from the IIc DB15 to the A2DVI card (A2E).

```
PIN_SEROUT  //  A2C Pin 11 - A2E Pin 49
PIN_14M     //  A2C Pin 2  - A2E Pin 48
PIN_WNDW    //  A2C Pin 7  - A2E Pin 47
PIN_TEXT    //  A2C Pin 1  - A2E Pin 46
PIN_GR      //  A2C Pin 10 - A2E Pin 45
PIN_GND     //  A2C Pin 13
PIN_+12V    //  A2C Pin 8

```

**FIRMWARE**

To load the firmware use a USB cable to your PC/Mac with the A2DVI card not slotted into anything.   There is a BOOTSEL button on the pico module.  Hold that button down, and while holding it, plug the USB cable into your PC.   You should see a volume appear on the PC/Mac.  Copy the "A2C_DVI_v1.7.X.uf2" file to this volume and the card should update and reboot.   If you have a HDMI monitor attached to the card, you should see the A2C_DVI splash screen.  Unplug the card from USB.  Never hot-plug the card in the slot or the DB15 connector to the Apple IIc.

[https://github.com/FarLeftLane/A2C_DVI_Prototype](url)

**BOM**

You can certainly source these components from places like DigiKey or Mouser for a lot less, but I'm just listing the Amazon links because you can typically get them overnight.

DB15: [https://www.amazon.com/dp/B01DLKNHBO](url)

Button: [https://www.amazon.com/dp/B07QH9WV12](url)

7805 Regulator and capacitors: [https://www.amazon.com/dp/B0BRV3HTXY](url)

Resistors: [https://www.amazon.com/dp/B0CDWW5BFH](url)

50-pin slot: [https://www.amazon.com/dp/B0BPP5794Z](url)

My boards were fabricated by JLCPCB, $12 for 30 boards and $23 for shipping.  Production files for JLCPCB and the KiCAD designs are included.

**USAGE**

Put your A2DVI card in the slot with the components facing away from the IIc keyboard.  Connect the DB15 directly to the back of the IIc.  Alternately, you can use a DB15 shielded extension cable ( [https://www.amazon.com/dp/B09P1LFRB4](url) ) if you need to use the Modem serial port.

With the 7805 working, the card should power on when you turn on you IIc.  A short press on the config button will cycle through video modes.  To access the config menu, you make a long press on the button.   Short presses to navigate the menus and long presses to select an option.  Long press on Save to store the settings for subsequent boots.

**ISSUES**

Still working on the color model for the firmware.
Firmware source code will be posted soon.

**COMING SOON?**

I have heard from a couple people that they are designing purpose-built cards based on this design.   Stay tuned...


**SPECIAL THANKS**

- Chris Auger ( https://www.8bitdevices.com/product/a2dvi/ )
- Jonathan Adar
- Jawaid Bazyar
- Ralle Palaveev ( https://github.com/rallepalaveev/A2DVI )
- Thorsten Brehm ( https://github.com/ThorstenBr/A2DVI-Firmware )


**Pictures**

Config screen:
![DEFAULT](https://github.com/user-attachments/assets/00f029e0-050e-4da7-86ac-1dbc8562bdf5)

DP15 Pinouts
![IMG_8899](https://github.com/user-attachments/assets/4d250206-9c86-4687-9743-096d6ff55ef8)

7805 Circuit:
![image](https://github.com/user-attachments/assets/ae684c7b-1a18-4d3e-9874-b82fae98b99a)

A2C_DVI_Board:
![image](https://github.com/user-attachments/assets/325f45c0-930e-4e58-8a78-005b97d244a0)


