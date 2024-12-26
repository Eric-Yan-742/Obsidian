# Boot Loader

- **Flash memory**: The memeory where your code lives

![[_attachments/Untitled 198.png|Untitled 198.png]]

> **Boot**: the process of starting a computer and putting it into a state of readiness for operation.

> **Boot loader**: The program runs before the application runs. It erases and rewrites the program into the flash memory

- For a brand new Arduino board, the Boot Reset Fuse is set to 0 by default. The program will begin at the boot flash start address (instead of the top of the flash memory 0x0000) after a reset. It means the Arduino has a bootloader written by the manufacturer.
    
    ![[_attachments/Untitled 1 56.png|Untitled 1 56.png]]
    
- Upload code using our own boot loader
    - What if the boot loader is deleted or the board is set to 1?
    - Do it by using the **SPI** bus
    - Use Programmer
        - Use **ICSP** Pins (In Circuit Serial Programming)
            
            ![[_attachments/Untitled 2 36.png|Untitled 2 36.png]]
            
        - Will erase the default boot loader of Arduino, so you need to burn the original boot loader to the board before you you upload the program to Arduino through USB again.
    - Use another Arduino Board
        
        - Upload the example ISP code to the “boot loader” Arduino board through USB
        - Connect GND of “boot loader” board to GND of the target board. Same for VCC/5V, SCK, MISO, MOSI
            
            ![[_attachments/Untitled 3 25.png|Untitled 3 25.png]]
            
        - Connect Pin 10 of the “boot loader” board to the RESET pin of the target board. Pin 10 is set by the ISP code.
        - It will erase the original boot loader as well.