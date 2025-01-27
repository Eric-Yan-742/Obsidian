履历表里面有写到的每个Project每个Project都先用英文先写下STAR分析powerpoint背起来或做成cheatsheet现在都是Online面试你直接在镜头前照稿念也可

Introduction: Hi I'm Eric. Now I'm a third year student studying in UC San Diego. I major in CS and math. I have course work in data structure, algorithm design, operating system, and a bunch of math classes. I did an internship at Xiaomi last year. I used Java with its web framework Springboot and MySQL. 
1. Major and School
2. Previous internship. What was your primary responsibility? My main responsibility is to process some financial bills. Implement some apis to reorganize and calculate the financial data. The main tech stacks I used was Java, Springboot, and MySQL. 

## Behavioral Question

STAR(T)standsfor:
- Situation-How when where and with whom? 
- Task–What task wereyou faced with o what responsibilities were you given? 
- Action-What action did you take? 
- Result-What was the result or what did you achieve? 
- Additionalstep: Transfer-How did the situation help you develop and what did you learn?

[问BQ(behavioral question)时，他们到底在问什么|mathworks求职讨论|一亩三分地求职（非面经）版](https://www.1point3acres.com/bbs/thread-566677-1-1.html)

1. Which project are you most proud of? 
	- My favorite project is the app I developed during my internship at Xiaomi last summer. My task was to process the financial data of incentive bonuses to employees. To finish this project, I used Java and its Springboot framework to set up the backend API. I applied Java's lambada expression and Stream API to process data more effectively. I also utilized the Template Design Pattern to make the program more extensible. In the end, my project accelerated the review and distribution process of the incentive bonuses. I learned a lot from this project from Java language features to Design Pattern. **How does this project match the position** ?
2. Your biggest drawback/failure? 
	- Sometimes I pursue perfection excessively. Once I was working on an assignment in my programming class class. I needed to implement a simple image editor. The assignment was not very bad overall, but I was very concerned about a function's runtime was a little bit slow. So I spent a lot of time on optimizing that function just to make it execute a few less lines of code. It caused me to fail to finish the whole program. From that incident, I learned although pursuing perfection is not wrong, it's more important to finish the whole task first. For all my DDL later, I tried to finish the entire task first although it may not be perfect. Then if there is extra time, I would use it to refine the details. As a result, I missed fewer DDLs afterwards
3. Your biggest advantage: **Company Dependent**
4. Why do you want to work here? **Company Dependent**
5. Team mates are lazy/difficult to get along with. How do you deal with it? 
	- I met this kind of problems before. Once I was preparing a group presentation for a class. Until a few days before the DDL, there were no people speaking in the group chats. I became nervous about the progress, so I reached out to other group members. It turned out that they were confused about which part of the task they should do and what other members had done. I realized we never had a clear division of the work, so everyone was hesitant and indecisive when working on it. Afterwards, I alwasy tried have a discussion within the group and assign a responsibility to everyone as specific as possible. It also gave the whole group more motivation. At the same time, I tried to reach out to other members as early as possible if they haven't shared their progress for a while to keep us on the same page. In my later groupworks, those principles make the group much more productive. 

## Experience and Projects on the Resume

Internship at Xiaomi: 
- What did you do: The company has to distribute incentive bonuses to sales staff in the physicla stores according to their sales performance. The financial staff will upload many sales performance reports in excel sheets. They contain each employer's name, their sales revenue in the past month, and their positions etc. I develope an app to download and parse the excel spreasheets. Sotre the information into data base. Reorganize the data for reviewing and calculate the incentive bonuses for each employers. It helps them improve the speed of reviewing and calculating the bills. 
- Switch data source: Accessing data souce is a big part of backend program. You need to constantly add, update, delete, search different data with different constraints. Sometimes you may expect to use different data bases in the future, their data access procedure and query langauge are different. The dependency injection feature is pretty handy for that. I can create a data manipulation interface and implement it with different data souce. In the upper level of the program, I can inject the interface and use that interface to manipulate the data. The Spring framework will decide which implementation to use with a few lines of annotations. To switch the data souce, you just need to edit the annotation a bit, which is pretty easy. You don't need to change the source code of the upper level of the program, so it also decouples the software structure. 
- Front end and backend communication: Front end program will send a http request to your backend server. Backend will deal with the data and give a http respond. 
- Stream API: A stream is a like a sequence of elements that can be processed in a functional style. It can be a list or a hash map. There are many operations like map, filter, groupby, which are quite useful for parsing excel data. If you want to convert a list of one type of objects to a list of other type, it makes the code much more readable than using nested loops. 

xv6:
- Copy on write fork: Fork is used to create a child process. The child process has the exact same memory as its parent process. The most straightfoward implementation is to copy the whole physical address space of the parent process. However, that will waste a lot of the resources because in most of the cases, the child process will not ever write to more than 90% of its memory. It would be much more efficient to let the child process to borrow its parent's memory (same physical memory different virtual memroy). If the child process needs to write to certain address, we then allocate the physical memroy for it. 
	- A few tricks in the implementation: set up a reference count for each physical page, set a flag in the page table entry for that page to let the kernel know when it needs to write to it. 
- Doubly indirect node: Unix-like operating system associates each file with an inode/dinode. Inode keeps some meta data about the file such as file type, permission, file size, and pointers to data blocks. In the beginning, each inode contains 12 direct pointers and 1 indirect pointer. By indirect pointer I mean the pointer in the inode points to a block that only contains pointers, a total of 256 pointers. Thus, the max file size can be at most 12 + 1 * 256 blocks. Building on this structure, I improve the inode to have 11 direct pointers plus 1 indirect pointer plus 1 doubly indirect pointers. A doubly indirect pointer points to a block that only contains indirect pointers. Each indirect pointer only contains pointers to data block. With that one more layer of mapping, a file can now have 256 * 256 more blocks of data. 

## Technical Algorithm Question

When solving algorithm problem
- If no thoughts about it at all, ask for hints after 5 min. 
- If you have an idea, talk about it in general while writing down your thought process. Make sure your idea makes sense with the interviewer before actually start to write your code. 
- When you're coding, explain what the code is doing after finishing a part of the program. 

At the end, ask some futher questions. 