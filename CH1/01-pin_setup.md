# Bolt Glitching Multitool - Challenges
## Challenge 1 - setup and pin mappings

**Follow these instructions/hints etc at your own risk !! I can't be held respnsible for any damage you might cause to any of your devices!**

Devices used:
* Shikra to investigate UART ports (you can use RaspberryPi, Tigard, Buspirate etc)
* ST-Link V2 (provided in the kit) (I had one and it had an older version that isn't supported by pyocd) 

Software used:
* screen
* pyocd

### 1. Setup 
There are many ways to install pyocd. Feel free to pick a method that works for you. 

Install pyocd:
```
pip3 install pyocd 
```

I also prefer screen for connecting to my Shikra (UART). Feel free to use any other alternative.

Install screen:
```
sudo apt install screen
```

### 1. Shikra or any device that supports UART
UART is s very simple protocol and here we have to connect the `TX` port on the Shikra to the `RX` on the target and then the `RX` port on the Shikra to the `TX` on the target and finally connect the `GND` from the Shikra to the target. (If you make a mistake just switch them over.)

### 2. ST-Link V2

Wire up the provided ST-Link v2 and the target board. You will be connecting to the JTAG port which are the pins `SWDIO`, `SWCLK` and `GND` from the ST-Link V2 to those found on the target board. I won't provide the pin numbers as the two ST-Links I have different pin mappings, so check yours first. It doesn't hurt to double and triple check connections)

### 4. Connect using UART
I used 3 bash windows. One for UART, a second for pyocd and a third for checking the output binary. 

Here my Shikra is presented as device `/dev/ttyUSB0` (yours might be a bit different) 

Command (screen):
```
screen /dev/ttyUSB0 115200 
```

### 5. Power up target

Now you can power up the target board via the USB-C port. You should get the following output via the UART in screen.

Output:
```
Hold one of the 4 challenge buttons to start them
Hold one of the 4 challenge buttons to start them
```

Move on to the next part, JTAG memory dumping using pyocd.
