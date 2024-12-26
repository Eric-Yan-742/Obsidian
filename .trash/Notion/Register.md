# Register

- **RAM memory**: All the data, variables, and registers live
- All the regions above “Internal SRAM” are used for registers. Those memory regions are occupied before any program data (eg: variables) are stored into the memory.

![[Untitled 120.png]]

- Because ATmega328P is 8 bit microcontroller, each register is 8 bit. 1 digital pin on Arduino is 1 bit (0 or 1). Thus, each register controls 8 digital pin.
    
    ![[Untitled 121.png]]
    
- When you set a register, you set 8 pins all together. You never set each digital pin independently.
    
    ```C
    //Binary form of 32: 0010 0000
    PORTB = 32; //Set PORTB5 to be HIGH and all other pins controlled by PORTB to be LOW
    ```
    
    ![[Untitled 122.png]]
    
    PB5 corresponds to Digital Pin 13