履历表里面有写到的每个Project每个Project都先用英文先写下STAR分析powerpoint背起来或做成cheatsheet现在都是Online面试你直接在镜头前照稿念也可

STAR(T)standsfor:
- Situation-How when where and with whom? 
- Task–What task wereyou faced with o rwhat responsibilities were you given? 
- Action-What action did you take? 
- Result-What was the result or what did you achieve? 
- Additionalstep: Transfer-How did the situation help you develop and what did you learn?

Introduction: Hi I'm Eric. Now I'm a third year student studying in UC San Diego. I major in CS and math. I have course work in data structure, algorithm design, operating system, and a bunch of math classes. I did an internship at Xiaomi last year. I used Java with its web framework Springboot and MySQL. 

Internship at Xiaomi: 
- What did you do? The company has to distribute incentive bonuses to sales staff in the physicla stores according to their sales performance. The financial staff will upload many sales performance reports in excel sheets. They contain each employer's name, their sales revenue in the past month, and their positions etc. I develope an app to download and parse the excel spreasheets. Sotre the information into data base. Reorganize the data for reviewing and calculate the incentive bonuses for each employers. It helps them improve the speed of reviewing and calculating the bills. 
- Switch data source: Accessing data souce is a big part of backend program. You need to constantly add, update, delete, search different data with different constraints. Sometimes you may expect to use different data bases in the future, their data access procedure and query langauge are different. The dependency injection feature is pretty handy for that. I can create a data manipulation interface and implement it with different data souce. In the upper level of the program, I can inject the interface and use that interface to manipulate the data. The Spring framework will decide which implementation to use with a few lines of annotations. To switch the data souce, you just need to edit the annotation a bit, which is pretty easy. You don't need to change the source code of the upper level of the program, so it also decouples the software structure. 
- Front end and backend communication: Front end program will send a http request to your backend server. Backend will deal with the data and give a http respond. 
- Stream API: A stream is a like a sequence of elements that can be processed in a functional style. It can be a list or a hash map. There are many operations like map, filter, groupby, which are quite useful for parsing excel data. If you want to convert a list of one type of objects to a list of other type, it makes the code much more readable than using nested loops. 

xv6:
- Copy on write fork: Fork is used to create a child process. The child process has the exact same memory as its parent process. The most straightfoward implementation is to copy the whole physical address space of the parent process. However, that will waste a lot of the resources because in most of the cases, the child process will not ever write to more than 90% of its memory. It would be much more efficient to let the child process to borrow its parent's memory (same physical memory different virtual memroy). If the child process needs to write to certain address, we then allocate the physical memroy for it. 
	- A few tricks in the implementation: set up a reference count for each physical page, set a flag in the page table entry for that page to let the kernel know when it needs to write to it. 
- Doubly indirect node: 