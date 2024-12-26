# Volatile

- The **volatile** keyword can prevent the compiler from ignoring “useless” assignment.
    
    ![[_attachments/Untitled 164.png|Untitled 164.png]]
    
    If we remove volatile, compiler will ignore all assignments to mitch because we don’t use mitch anywhere in the program except repeatedly assigning values to it.
    
- It’s useful to use volatile for hardware address
- Set address 0x25 with a value of 32
    
    ```C
    *( (volatile byte*) 0x25) = 32;
    ```