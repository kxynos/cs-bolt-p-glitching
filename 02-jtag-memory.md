
# Bolt - Challenges 
## Challenge 1 - JTAG memory dump and investigation

Investigate the target board for what chips are on it.

What we are looking for in the datasheet are 2 things: 
a) the location of the SRAM and, b) the size. 
We notice that it has a `STM32F103xx` so let's see what the datasheet says (https://www.st.com/resource/en/datasheet/stm32f103c8.pdf). 

On page 34 we can see that `SRAM` is located at `0x20000000`. Next, we need to see where is the `System Memory`. This sits between `0x1FFF F800` and `0x1FFF F000`. If we subtract these, we get `0x800` (`2048` decimal) and this is our size. 

We will use pyocd to easily interact with the JTAG interface of the STM32 chip. 

Let's make a test connection to see if everything is working. Run pyocd and connect to the JTAG system. You can connect to it when the system reboots. Hit the reboot button and quickly start `pyocd commander`

If you are successful, you should get the following output: 
`$ pyocd commander 
0000597 W Generic 'cortex_m' target type is selected by default; is this intentional? You will be able to debug most devices, but not program flash. To set the target type use the '--target' argument or 'target_override' option. Use 'pyocd list --targets' to see available targets types. [board]
Connected to CoreSightTarget [Reset]: B55B5A1A00000000F73BF301
`
Now try to dump the memory. 

In particular you can use `savemem ADDR LEN FILENAME`

`pyocd> savemem 0x20000000 2048 sram.bin`

Now try to figure out how to dump the memory, check the hints provided by Tom. 
If you want to inspect the sram dump try `hexdump` or `xxd`:

`hexdump -C sram.bin`

To print out the CTF flag:

`strings sram.bin`
