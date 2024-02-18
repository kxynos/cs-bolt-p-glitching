# Bolt Glitching Multitool - Challenges
## Challenge 2 - pin mappings and software setup

**Follow these instructions/hints etc at your own risk !! I can't be held responsible for any damage you might cause to any of your devices!**

**There are limited spoilers in this tutorial (the solution is not provided!)**

Devices used:
* Shikra to investigate UART ports (you can use RaspberryPi, Tigard, Buspirate etc)

Software used:
* screen (see CH1 for installing screen)
* Bolt scope library 

Aim of this tutorial:
* Reset the target board using the BOLT. The BOLT will cause a voltage glitch via Python3 that we control.

### 1. Setup pins

Like me, you might be wondering what to wire and how. Let's start with the BOLT. As per the BOLT's pin layout (https://bolt.curious.supplies/docs/getting-started/pinout/) what we are interested in using are the `GND` and `SIG` pins. These will be mapped to `GND` and `VMCU` repectively. So first thing is to map BOLT `GND` to Target `GND` and BOLT `SIG` to Target `VMCU`.

| BOLT | Target| 
| ----- | ---- |
| GND | GND |
| SIG | VMCU |

Table 1. pin mapping

`Tom` tells me that the BOLT uses SIG to short with GND briefly and this causes the glitch to occur. 

Note. I am using my Shikra to still be able to interact with the UART interface. (See CH1 for how I set that up)

### 2. Setup software 

We need to install pyserial dependancy.

Install pyserial:
```
pip3 install pyserial
```

Setup serial comms permissions(add current user to the `dailout` group, exit and loging again for it to apply): 
```
sudo usermod -a -G dialout $USER
logout
```

Check that `dailout` group is present: 
```
groups
```

Create a working folder (I assume you are using Documents and a folder called bolt, change this as required):
```
mkdir ~/Documents/bolt
cd ~/Documents/bolt
```

Install Curious Supplies Bolt library called scope.py found in lib (I will assume you are using a Linux system):

```
git clone https://github.com/tjclement/bolt
cd ~/Documents/bolt/bolt/lib
```

### 3. Test if everything is working

You should be in the folder (~/Documents/bolt/bolt/lib).


Start Python3 and test a voltage glitch via `s.glitch.repeat` :
```
from scope import Scope
s = Scope()
s.glitch.repeat = 1000000
s.trigger()
```

We need to setup `s.glitch.repeat` to the correct value. For now we will set it to a large number so we can see it resetting the board via UART/screen. By setting `s.glitch.repeat = 1000000` we will get `8.3ms` total glitch duration. (Each clock cycle is 8.3ns, so this would be 8.3ms total glitch duration) - Thank `Tom` for the information!


Expected output from console on PC:

```
$ python3
Python 3... (..) [...] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from scope import Scope
>>> s = Scope()
Connected to version: 0.0.1
>>> s.glitch.repeat = 1000000
>>> s.trigger()

```

Expected output from BOLT via screen (I have selected Challenge 2 and as it starts to count I issue a `s.trigger()` command in Python3 and the target resets to the default menu:

```
Hold one of the 4 challenge buttons to start them
Hold one of the 4 challenge buttons to start them
Starting challenge 2
1000 1000 1000000
Hold one of the 4 challenge buttons to start them
Hold one of the 4 challenge buttons to start them
```

This is only the first step in solving this challenge. You know need to find the correct timing and set it in `s.glitch.repeat` and trigger a glitch.

Good luck with your glitching and finding the CTF flag!

