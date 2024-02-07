
Investigate the taget board for what chip are on it.

We notice that it has a STM32F103xx so lets see what the datasheet says (https://www.st.com/resource/en/datasheet/stm32f103c8.pdf). 
What we are looking for here are 2 things: 
a) the location of the SRAM and, b) the size. 

On page 34 we can see that `SRAM` is located at `0x20000000`. Next we need to see where is the `System Memory`. This sits between `0x1FFF F800` and `0x1FFF F000`. If we subtract these we get `0x800` (`2048` decimal) and this is our size. 

We will use pyocd to easily interact with the JTAG interface of the STM32 chip. 

In particular we will use `savemem ADDR LEN FILENAME`

`pyocd> savemem 0x20000000 2048 sram.bin`

`strings sram.bin`
