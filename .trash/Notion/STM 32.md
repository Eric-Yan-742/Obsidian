[[Fastbit MCU1]]

# How to set up a project using STM32Cube IDE

> [!info] Getting Started with STM32 and Nucleo Part 1: Introduction to STM32CubeIDE and Blinky – Digi-Key  
> We’re kicking off a new video series!  
> [https://www.youtube.com/watch?v=hyZS2p1tW-g](https://www.youtube.com/watch?v=hyZS2p1tW-g)  

# Document

> [!info] NUCLEO-L476RG - STMicroelectronics  
> NUCLEO-L476RG - STM32 Nucleo-64 development board with STM32L476RG MCU, supports Arduino and ST morpho connectivity, NUCLEO-L476RG, STMicroelectronics  
> [https://www.st.com/en/evaluation-tools/nucleo-l476rg.html#](https://www.st.com/en/evaluation-tools/nucleo-l476rg.html#)  

> [!info] STM32L476RG - STMicroelectronics  
> STM32L476RG - Ultra-low-power with FPU Arm Cortex-M4 MCU 80 MHz with 1 Mbyte of Flash memory, LCD, USB OTG, DFSDM, STM32L476RGT6TR, STM32L476RGT6, STM32L476RGT3, STMicroelectronics  
> [https://www.st.com/en/microcontrollers-microprocessors/stm32l476rg.html](https://www.st.com/en/microcontrollers-microprocessors/stm32l476rg.html)  

![[arm_cortexm4_processor_trm_100166_0001_04_en.pdf]]

![[stm32l476rg.pdf]]

![[Nucleo-l476rg_Reference_Manual.pdf]]

![[um1724-stm32-nucleo64-boards-mb1136-stmicroelectronics.pdf]]

![[nucleo-l476rg_(2).pdf]]

![[en.MB1136-DEFAULT-C05_Schematic.pdf]]

  

  

# Source Code of Examples on Mastering STM32

> [!info] GitHub - cnoviello/mastering-stm32: Repository of all examples presented in the "Mastering STM32" book  
> Repository of all examples presented in the "Mastering STM32" book - GitHub - cnoviello/mastering-stm32: Repository of all examples presented in the "Mastering STM32" book  
> [https://github.com/cnoviello/mastering-stm32](https://github.com/cnoviello/mastering-stm32)  

# Enable OpenOCD semihosting for a project

> [!info] Lecture 8: Testing printf using semihosting and OpenOCD  
> Course Name: Microcontroller Embedded C Programming for absolute beginners  
> [https://www.youtube.com/watch?v=FDCI4yfovfw&t=321s](https://www.youtube.com/watch?v=FDCI4yfovfw&t=321s)  

# Set up SWV

> [!info] Getting Started with Instrumentation Trace Macrocell in STM32CubeIDE  
> For ARM Cortex M3 and M4 based MCUs  
> [https://medium.com/@g.bharathraj19/getting-started-with-instrumentation-trace-macrocell-in-stm32cubeide-4af179eb0034](https://medium.com/@g.bharathraj19/getting-started-with-instrumentation-trace-macrocell-in-stm32cubeide-4af179eb0034)  

# How to change all text to English

> [!info] How to change language for STM32cubeIDE?  
>  
> [https://community.st.com/s/question/0D53W00000EnekQSAR/how-to-change-language-for-stm32cubeide](https://community.st.com/s/question/0D53W00000EnekQSAR/how-to-change-language-for-stm32cubeide)  

# Key Point

- A typical **microcontroller** includes a **processor**, memory and input/output (I/O) peripherals on a single chip.
    - stm32l476rg is a MCU. ARM Cortex M4 is a processor
- LD1
    - The tricolor LED (green, orange, red) LD1 (COM) provides information about ST-LINK communication status. LD1 default color is red. LD1 turns to green to indicate that communication is in progress between the PC and the ST-LINK/V2-1
- STML4 Series: SRAM2 is within Code memory space (base address: 0x1000 0000)
    - The SRAM total size is 128 Kbytes. It is split into 2 parts: SRAM1 is 96 Kbytes starting from address 0x20000000 and SRAM2 is 32 Kbytes starting from address 0x10000000. SRAM1 is located in the usual ARM memory space for RAM while SRAM2 can be directly accessed through Data code and Instruction code buses.
- FCLK and HCLK
    - Processor clock or Core clock (FCLK) always runs with the same speed as HCLK (H-Clock, clock speed of AHB bus). If system clock speed is greater than max core clock speed, you need to use AHB prescalar to divide system clock speed to be under the max core clock speed.
- **Block Diagram** can be found at the datasheet of MCU
- Reset value is the default value of register
- How to manipulate bits of a register
    1. Write down the position of this bit and the address offset of this register
    2. Go to memory overview (rm), find the base address of RCC and the peripheral.
    3. With block diagram, check which bus is this peripheral connected to.
    4. Go to RCC registers section (rm), find the clock enable register for that bus.
    5. Find the offset address of the enable register and the bit position of that peripheral.
    6. Use macro to define the address of the specific register
        
        - Add UL at the end of the address, UL means unsigned long
        
        $$\text{Register address = Peripheral Base address + Register Offset address}$$
        
    7. Use `uint32_t*` to store the register address.
    8. Turn clock enable bit to 1. Use the mask below
        
        $$\text{*RCC-reg}=(1<<\text{bit position})$$
        
    9. Use the same approach, change the bit of the register of the peripheral.
- How to clear specific bits
    
    ```Plain
    *ptr &= ~(0x3 << 16); //Turn 16th and 17th bits to 0, all other bits remain unchanged
    ```
    
- You can consider Exception Handler as a ‘C’ function
    
    > A pointer to a function can hold the address marking the start of the function code.
    
- BEQ (Arm Assembly)
    
    ![[_attachments/Untitled 31.png|Untitled 31.png]]
    
    cmp + beq.n form a while loop
    
    > **BEQ** (short for "**B**ranch if **EQ**ual") is the [mnemonic](https://www.c64-wiki.com/wiki/Mnemonic) for a [machine language](https://www.c64-wiki.com/wiki/Machine_language) instruction which branches, or "jumps", to the address specified if, and _only_ if the [zero flag](https://www.c64-wiki.com/wiki/Zero_flag) is set. If the zero flag is clear when the CPU encounters a BEQ instruction, the CPU will continue at the instruction following the BEQ rather than taking the jump.
    
- Volatile
    - Whenever when you are reading or writing to an address, you should always use volatile keyword for the pointer.

# GPIO