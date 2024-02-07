# Bolt Glitching Multitool - Challenges
## Challenge 1

** Follow these instructions/hints etc at your own risk !! I can't be held respnsible for any damage you might cause to any of your devices! **

Devices used:
* Shikra to investigate UART ports (you can use RaspberryPi, Buspirate, 
* ST-Link V2 (provided in the kit) (I had one and it had an older version that isn't supported by pyocd) 

## 1. Shikra or any device that supports UART
UART is very simple and here we have to connect the `TX` port on the Shikra to the `RX` on the target and then the `RX` port on the Shikra to the `TX` on the target and finally connect the `GND` from the Shikra to the target. (If you make a mistake 

## 2. ST-Link V2

Wire up the provided ST-Link v2 and the target board. You will be connecting to the JTAG port which are the pins `SWDIO`, `SWCLK` and `GND`. 
Now you can power up the ..
