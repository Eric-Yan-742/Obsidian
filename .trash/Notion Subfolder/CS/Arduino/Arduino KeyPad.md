- [x] Debounce?

- [x] Pull up and pull down resistor

- [x] Manually Create a pull down resistor for the project

- [x] volatile

- [x] What is enum
- [x] Test Serial Monitor
- [x] Why no conflicts between timeOut and clearLock
- [x] Test Max Failed Number
- [x] Record a demo video

# Debounce

> [!info] Arduino Button Debounce Tutorial  
> Arduino Button Tutorial: How to debounce a button on an Arduino Uno or Mega.  
> [https://www.youtube.com/watch?v=jYOYgU2vlSE](https://www.youtube.com/watch?v=jYOYgU2vlSE)  

# Pullup and Pulldown resistor

> [!info] Short and sweet: pull up/pull down resistors explained  
> Short and sweet: pull up/pull down resistors explained  
> [https://www.youtube.com/watch?v=5vnW4U5Vj0k](https://www.youtube.com/watch?v=5vnW4U5Vj0k)  

- For a mechanical button, the pins that are far apart are ganged together. The pins that are close to each other are ganged together.
    
    ![[_attachments/Untitled 182.png|Untitled 182.png]]
    
- A digital pin has three states: High, Low, and Floating

> [!info] Pull-up and Pull-down Resistors | Resistor Applications | Resistor Guide  
> What Are Pull-up Resistors?  
> [https://eepower.com/resistor-guide/resistor-applications/pull-up-resistor-pull-down-resistor/#](https://eepower.com/resistor-guide/resistor-applications/pull-up-resistor-pull-down-resistor/#)  

# Breadboard

- For the two pairs of power column (red + and blue -), the column only has voltage when it is connected to the 5V or GND of the board.

# Volatile

- The volatile qualifier tells the compiler that a variable can have its value altered by agencies other than the program. It is typically used for hardware addresses and for data shared with other programs or threads running simultaneously. For example, an address might hold the current clock time. The value at that address changes as time changes, regardless of what your program is doing. Or an address could be used to receive information transmitted from, say, another computer.
- Syntax:
    
    ```C
    volatile int loc1; /* loc1 is a volatile location */
    volatile int * ploc; /* ploc points to a volatile location */
    ```
    
- Volatile facilitates **optimization**
    
    ```C
    val1 = x;
    /* some code not using x */
    val2 = x;
    ```
    
    A smart (optimizing) compiler might notice that you use x twice without changing its value. It would temporarily store the x value in a register. Then, when x is needed for val2, it can save time by reading the value from a register instead of from the original memory location. This procedure is called **caching**.
    
    ```C
    <reg> = x
    val1 = <reg>
    val2 = <reg>
    ```
    
    Ordinarily, caching is a good optimization, but not if x is changed (in memory) between the two statements by some other agency. If there were no volatile keyword, a compiler would have no way of knowing whether this might happen. Therefore, to be safe, the compiler couldn’t cache. That was the pre-ANSI situation. Now, however, if the volatile keyword is not used in the declaration, the compiler can assume that a value hasn’t changed between uses, and it can then attempt to optimize the code.
    

# Interrupt

- Lines of code are broken down into instructions and must be run in order. This means that when trying to take inputs, that particular instruction must be currently running. However, that makes it easy to miss inputs from sensors.
- Interrupts are used to instantly respond to inputs from a sensor. It runs in the background and can be instantly activated as soon as inputs are detected. It _interrupts_  
    the main program and runs the Interrupt Service Routine (ISR) assigned to it. When it is completed, the program will continue from where it left off.  
    
- ISRs are designed to be short so that the program can quickly resume and be able to detect new inputs immediately.

![[_attachments/Untitled 1 42.png|Untitled 1 42.png]]

```C
attachInterrupt(digitalPinToInterrupt(interruptButton), interruptdetected, RISING);
// Sets up the interrupt function to run whenever the pin state changes from HIGH to LOW
```

# Addressable LED

> You can use a single digital pin to transfer a signal along the entire LED strip and manipulate each LED separately instead of using a digital pin for each LED.

# Enumerated Types

- You can use the enumerated type to declare symbolic names to represent **integer constants**. By using the enum keyword, you can create a new “type” and specify the values it may have. (Actually, enum constants are type int; therefore, they can be used wherever you would use an int.) The purpose of enumerated types is to **enhance the readability of a program**.

```C
enum spectrum {red, orange, yellow, green, blue, violet}; //establish "spectrum" as a tag
enum spectrum color; //"color" is a variable of "spectrum" type.
//red, orange, yellow ... are possible values of color. 
```

- Symbolic constants like red, orange etc are termed **enumerator**
- Although enumerators such as red and blue are type int, enumerated variables are more loosely constrained to be an integral type as long as the type can hold the enumerated constants. **(compiler chooses the most space efficient type for the integer**). For example, the enumerated constants for spectrum have the range 0–5, so a compiler could choose to use unsigned char to represent the color variable.
- Default value
    
    ```C
    enum kids {nippy, slats, skippy, nina, liz};
    //nippy = 0, slats = 1 ... liz = 4
    ```
    
- Assigned value
    
    ```C
    enum feline {cat, lynx = 10, puma, tiger};
    //cat is 0, by default, and lynx, puma, and tiger are 10, 11, and 12
    ```
    

# LOW and GND

- If you set a digital pin as OUTPUT and LOW, it acts approximately the same as the ground, which is 0V.

# Buzzer

> [!info] Arduino: How to Use a Piezo Buzzer  
> Parts:  
> [https://www.youtube.com/watch?v=K8AnlUT0ng0](https://www.youtube.com/watch?v=K8AnlUT0ng0)  

# Servo

> [!info] Arduino Tutorial 30: Understanding and Using Servos in Projects  
> You guys can help me out over at Patreon, and that will help me keep my gear updated, and help me keep this quality content coming:  
> [https://www.youtube.com/watch?v=aFHu65LiFok](https://www.youtube.com/watch?v=aFHu65LiFok)  

# Map Function

> [!info] 18 Demo of Arduino Map function  
> Demo of Arduino Map function  
> [https://www.youtube.com/watch?v=SpabOCIOTO4](https://www.youtube.com/watch?v=SpabOCIOTO4)  

  

- Use `return;` in `loop()` is approximately the same as using `continue` in a `while` loop.
    
    > loop() is just a function called from main() and as such it is legitimate to `return;`  
    > from it and have it called again from the start  
    

# Multiple .ino files in 1 folder

> Before compilation starts, the Arduino IDE concatenates all .ino files in your sketch into a single file, starting with the .ino file that matches the sketch folder name, followed by the rest of the .ino files in **alphabetical** order.