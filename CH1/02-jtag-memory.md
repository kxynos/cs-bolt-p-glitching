# Bolt Glitching Multitool - Challenges

## Challenge 1 - JTAG memory dump and investigation (setup and getting you to the solution)

Investigate the target board for what chips are on it.

What we are looking for in the datasheet are 2 things: 
a) the location of the SRAM and, b) the size. 
We notice that it has a `STM32F103xx` so let's see what the datasheet says (https://www.st.com/resource/en/datasheet/stm32f103c8.pdf). 

On page 34 we can see that `SRAM` is located at `0x20000000`. Next, we need to see where is the `System Memory`. This sits between `0x1FFF F800` and `0x1FFF F000`. If we subtract these, we get `0x800` (`2048` decimal) and this is our size. 

We will use pyocd to easily interact with the JTAG interface of the STM32 chip. 

Let's make a test connection to see if everything is working. Run pyocd and connect to the JTAG system. You can connect to it when the system reboots. Hit the reboot button and quickly start `pyocd commander`

Command:
```
pyocd commander 
```
If you successfully connect, you should get the following output (and the device will have frozen, no LED activity): 

```
$ pyocd commander 
0000597 W Generic 'cortex_m' target type is selected by default; is this intentional? You will be able to debug most devices, but not program flash. To set the target type use the '--target' argument or 'target_override' option. Use 'pyocd list --targets' to see available targets types. [board]
Connected to CoreSightTarget [Reset]: B55B5A1A00000000F73BF301
```

Now try to dump the memory. 

In particular you can use `savemem ADDR LEN FILENAME`, check the pyocd manual or `help` in **pyocd commander**.

Command (in pyocd): 
```
savemem 0x20000000 2048 sram.bin
```

Output (example):
```
pyocd> savemem 0x20000000 2048 sram.bin
Saved 2048 bytes to sram.bin
```

If you want to inspect the sram dump try `hexdump` or `xxd`. 

Command:
```
hexdump -C sram.bin
```

If you have dumped the initial script you should get something like this (this is correct): 
```
00000120  00 00 00 00 00 20 20 00  00 00 00 00 48 6f 6c 64  |.....  .....Hold|
00000130  20 6f 6e 65 20 6f 66 20  74 68 65 20 34 20 63 68  | one of the 4 ch|
00000140  61 6c 6c 65 6e 67 65 20  62 75 74 74 6f 6e 73 20  |allenge buttons |
00000150  74 6f 20 73 74 61 72 74  20 74 68 65 6d 0d 0a 00  |to start them...|
```

Now try to figure out how to dump the memory of challenge 1, check the hints provided by Tom. 

To print out the CTF flag:

```
strings sram.bin | grep ctf
```
