- [size_t为何这么重要？ - Noble_ - 博客园 (cnblogs.com)](https://www.cnblogs.com/noble/p/4144051.html)
- windows socket: “error C4996: ‘inet_addr’: Use inet_pton() or InetPton() instead or define …”
    - add `#pragma warning(disable:4996)`

# 第一章

socket, initialize sockaddr_in, bind, listen, accept, write

socket, connect, read

- Visual Studio如何启动多个项目：右键项目-》配置启动项目-》单启动项目是不管你选中了哪一个文件都启动那个项目，多个启动项目是同时启动表中所有项目，当前选定内容是你选定了哪个文件就启动哪个项目
- 项目属性→链接器→输入→附加依赖项→ws2_32.lib
- 底层文件I/O函数与ANSI标准定义的文件I/O函数之间有何区别？
    - ANSI标准定义的输入、输出函数是与操作系统（内核）无关的以C标准写成的函数，可在任意编译、操作系统上执行；相反，底层文件I/O函数是操作系统直接提供的，只能在特定操作系统上执行
    - Linux底层I/O：read, open, write, 文件描述符为int
    - ANSI C标准：fread, fopen, fwrite, 文件描述符为FILE*

# 第二章

sockaddr_in: htons, htonl, inet_addr, inet_aton, inet_ntoa

- 大小端转换函数
    
    sin_addr.s_addr是in_addr_t类型，其实就是uint32（IPV4地址）
    
    sin_port是in_port_t类型，其实就是uint16（端口号）
    
    - 数字转数字
        
        ![[_attachments/Untitled 161.png|Untitled 161.png]]
        
    - 字符串转数字
        
        ![[_attachments/Untitled 1 31.png|Untitled 1 31.png]]
        
        传递类似 “211.214.107.99”
        
        ![[_attachments/Untitled 2 18.png|Untitled 2 18.png]]
        
    - 数字转字符串
        
        ![[_attachments/Untitled 3 12.png|Untitled 3 12.png]]
        
- Linux read，当服务端close(clnt_sock)时（不是serv_sock)，客户端的read才会返回0，相当于链接断开了
- TCP协议的数据是没有边界的：
    - write只指定字节，也不知道这些字节是分几个包发过去的（TCP根据流量控制和拥塞控制决定）。read也只指定字节，直到读到这么多字节才返回，没法控制每次都读到一条完整的用户信息。
    - 如果每条消息都是固定长度的，那每次指定相同的字节就能确认消息边界，但如果每条信息不是定长的（HTTP请求），那就得在协议上下功夫。
- socket()函数创建的，和sockaddr_in初始化的，到底哪个才是自己的套接字？
- 为什么需要sockaddr_in?
    
    In Linux socket programming, the `**socket()**` function is used to create a new socket. However, a socket alone doesn't contain all the information needed for communication; you also need to specify the address and port to bind the socket to.
    
    p50
    
- 端口（port）是干什么用的
    
    - IP地址是为了区分网络上的主机。端口号是区分同一主机下的不同的SOCKET，以确保程序进程都能准确收发数据。
    
    ![[_attachments/Untitled 4 10.png|Untitled 4 10.png]]
    

  

# 第三章

- 网络编程的大部分内容就是设计并实现应用层协议
- accept中的sockaddr*使用来保存客户端地址信息的。connect中的sockaddr*使用来告诉函数连接的服务器地址在哪。
- char *fgets(char *str, int n, FILE *stream)
    - 最多读 n-1 个字节，在最后一个字节加上 ‘\0’
    - 如果遇到了换行符，会把换行符也加进来，并最后再跟上 ‘\0’
- strlen返回的长度并不包含null terminator。
- 阻塞模式下的read
    - nbytes：能读入的**最大字节数**，一般不是真正读入的大小。
    - 原则是在不超过指定的长度下，**有多少读多少**，没有数据则一直等待
- 为什么要手动加null terminator
    
    ```C++
    write(sock, message, strlen(message));
    str_len = read(sock, message, BUF_SIZE - 1);
    message[str_len] = 0;
    ```
    
    strlen会把null terminator弄丢，read从缓冲区里有几个字节读几个字节，不负责在最后加null terminator。这样把本来message里的null terminator弄丢了。如果我们要打印message必须借助null terminator。所以这样可以确保message被正常打印。
    
- 为何需要把TCP/IP协议栈分成4层（或7层）？结合开放式系统回答
    - 隔层之间是独立的，灵活性好，结构上可以分隔开，易于实现和维护，能促进标准化工作。
- 什么时候创建连接请求等待队列？它有何作用？与accept有什么关系
    - 调用listen函数时创建了连接请求等待队列。它是存储客户端连接请求信息的空间。accept函数调用后，将从连接请求队列中取出连接请求信息，并与相应客户端建立连接。
- 客户端中为何不需要调用bind函数分配地址？如果不调用bind函数，那何时、如何向套接字分配IP地址和端口号？
    - 客户端是请求连接的程序，不是一个接收连接的程序。所以，服务器的地址信息是更重要的因素，没有必要通过bind函数明确地分配地址信息。但是，要想和服务器通信，必须将自己的地址信息分配到套接字上，因此，在connect函数调用时，（操作系统）自动把IP地址和端口号输入到套接字上。

# 第四章

- **客户端连接请求本身也是接收到的一种数据**，向要接受就需要套接字。作为listen函数的第一个参数，服务器端套接字是接收连接请求的一名门卫或一扇门。

![[_attachments/Untitled 5 9.png|Untitled 5 9.png]]

- 之前的回声服务端其实没问题，接收几个字节就发几个字节。问题在于服务端，调用一次write之后调用一次read就企图接收到发送字符串的所有字节。如果字符串过长，TCP把一个字符串分成好几份发送，read接收到的可能就只有部分字符串。所以解决方法也很简单，write后进入一个while循环，每次read之后统计收到的总字节数，直到收到所有字节再打印信息。
- scanf
    - scanf() 处理⽤⼾输⼊的原理是，⽤⼾的输⼊先放⼊缓存，等到按下**回⻋键**后，按照占位符对缓存进⾏解读。
    - scanf() 处理数值（整形，浮点）占位符时：会跳过空⽩字符，包括空格、制表符、换⾏符等。 直到读取到所有占位符，或读取到非法字符（ `abc,.;` 等）时停止。
        - 注意空白字符不算非法字符，scanf读取时遇到它们不会立刻停止。但scanf在遇到其他字符时会立刻停止（即使还有未读完的占位符）。
        - 所以中间可以用空白字符分隔数值，最后可以用空白/非法字符终止。
        - 中间的空白字符会被读取，不会留在缓存中，最后的终止字符会留在缓存中
    - scanf()里带其他字符：scanf会在这些位置预期收到这些字符并把它们读取掉（不做任何处理），比整形占位符更严格，如果到了这个位置出现了任何其他字符（包括空白字符）都会当作非法字符处理
        
        ```Plain
        scanf("%d,%d,%d", &a, &b, &c);
        
        a,b,c 正确读取
        a, b, c 正确读取
        a ,b, c 正确读取a，因为a后面不是,而是空格而提前终止，从空格往后都会留在缓存中
        ```
        
    - scanf()处理字符型占位符时： %c 不忽略空⽩字符，总是返回当前第⼀个字符，⽆论该字符是否为空格。 如果要强制跳过字符前的空⽩字符，可以写成scanf(" %c", &ch) ，即 %c 前加上⼀个空格，表⽰跳过零个或多个空⽩字符。
- printf不加\n就不输出？
    
    > 输出缓冲通常在以下情况下会被刷新（即，将缓冲区中的内容输出到终端）：
    > 
    > 1. 当输出缓冲区已满时。
    > 2. 当输出缓冲区中包含换行符 `**\n**` 时（对于行缓冲模式）。
    > 3. 当使用 `**fflush**` 函数刷新缓冲区时。
    > 4. 当程序正常终止时，缓冲区会被自动刷新。
    
- TCP每次发送多少字节
    - MSS 代表最大分段大小（Maximum Segment Size），在 TCP（传输控制协议）中，它指的是在 TCP 报文段中，除了 TCP 头部外，可以承载的最大数据量。MSS 是 TCP 在建立连接时协商确定的一个参数，用于确定在数据传输过程中每个 TCP 分段（也称为 TCP 报文段）的最大数据量。
    - op_server中如果只read1个或几个字节就只需要read一次。但如果read的几十或上百个字节，write可能会把这一条消息拆成几个数据包发送，所以需要在循环中反复read直到确认收到的长度够了为止。
        
        ```C
        while(recv_len < (opnd_cnt * OPSZ + 1))
        {
            recv_cnt = read(clnt_sock, &opinfo[recv_len], BUF_SIZE - 1);
            recv_len += recv_cnt;
        }
        ```
        
- read和write都是以字节为单位进行传输，所以参数的buffer数组最好转换成(char*)类型

# 第五章

- I/O缓冲
    
    ![[_attachments/Untitled 6 9.png|Untitled 6 9.png]]
    
    滑动窗口管理的就是这个缓冲
    
- read什么时候返回0？
    
    > 客户端调用 `close`，表明客户端没有数据需要发送了，则此时会向服务端发送 FIN 报文，进入 FIN_WAIT_1 状态；
    > 
    > 服务端接收到了 FIN 报文，TCP 协议栈会为 FIN 包插入一个文件结束符 `EOF` 到接收缓冲区中，应用程序可以通过 `read` 调用来感知这个 FIN 包。这个 `EOF` 会被**放在已排队等候的其他已接收的数据之后**，这就意味着服务端需要处理这种异常情况，因为 EOF 表示在该连接上再无额外数据到达。此时，服务端进入 CLOSE_WAIT 状态；
    
    所以在read return 0之前会读完连接中断之前发来的所有数据
    

# 第六章

- UDP套接字的recvfrom中的sockaddr*仅仅是个容器，接收到之后用来填充的。和accept差不多
    - accpet的sockaddr*建立连接之后从来没用过，但recvfrom的sockaddr*是可以为sendto指定地址的。
- recvfrom永远不会return 0，没EOF这说法
- recv_from每次读取都是一个完整的数据报文。如果对方sendto了两个数据报文，一次recv_from也只会收到第一个。所以UDP中调用I/O函数的次数应该保持一致
- TCP传输的是数据包，UDP传输的数据包又可以叫数据报。
- TCP和UDP套接字都有两套地址信息
    - 本地IP+端口号：bind绑定的那个。对于客户端，TCP的connect自动给套接字分配本机IP+端口号，UDP的sendto在第一次调用时给套接字分配本地IP+端口号。
    - 目标IP+端口号：TCP的connect根据参数向套接字注册目标IP+端口，UDP的sendto在每次调用时向套接字重新分配目标IP+端口
- 如果你已经使用 connect() 函数为UDP套接字指定了目标地址，后续调用 sendto() 函数指定的目标地址会被忽略，数据仍然会发送到 connect() 设置的默认目标地址。因此，在这种情况下，后续对 sendto() 的调用指定的目标地址不会起作用。
- TCP和UDP可以使用相同端口号。TCP和UDP不存在“同一个端口”，只存在“同一个端口号”。就像1号楼的404和2号楼的404是两个不同的房间一样。所以相同的端口号指代的其实是不同的端口。
- UDP 套接字和 TCP 套接字可以共存。若需要，可以同时在同一主机进行 TCP 和 UDP 数据传输。
- UDP数据报向对方主机的UDP套接字传递过程中，IP和UDP分别负责哪些部分？
    - IP负责节点之间数据包的传送，IP将数据包传送到节点之后，并不能识别节点上的不同应用，UDP用端口表示这些不同的应用来加以识别，增加了端口信息。而TCP还加入了更复杂的传输控制。

# 第七章

- 半连接流程
    
    ![[_attachments/Untitled 7 8.png|Untitled 7 8.png]]
    

# 第八章

- gethostbyname：通过域名获取IP
    
    ![[_attachments/Untitled 8 8.png|Untitled 8 8.png]]
    
- DNS解析流程
    
    [DNS详细解析问题 - luzhouxiaoshuai - 博客园 (cnblogs.com)](https://www.cnblogs.com/kebibuluan/p/15033442.html)
    

# 第九章

- sizeof返回类型是size_t，无符号整数类型unsigned int。
- 套接字选项并不都是4个字节
- TCP_NODELAY可选项与Nagle算法有关，可通过它禁止Nagle算法。请问何时应考虑禁用Nagle算法？结合收发数据的特性给出说明
    - 根据传输数据的特性，网络流量未受太大影响时，不使用Nagle算法要比使用它时传输速度快。例如“传输大文件数据”。将文件数据传入输出缓冲不会花太多时间。因此，即便不使用Nagle算法，也会在装满输出缓冲时传输数据包。这不仅不会增加数据包的数量，反而会在无需等待ACK的前提下连续传输，因此可以大大提高传输速度。（不存在小数据包）
- 完整的四次挥手过程可靠地实现了全双工连接的终止（确保双方数据都已经发送完毕了），但如果服务器意外崩溃需要紧急重启立刻提供服务，timewait状态会阻止程序在相同的端口创建新的套接字。

# 第十章

- 程序信号本质是硬件/软件中断用户程序，由内核跳转到对应的handler，执行结束后再回到用户原来的pc。并不是一般的函数调用。
- WIFEXITED
    
    ```C
    pid_t id = waitpid(-1, &status, WNOHANG);
    if(WIFEXITED(status))
    {
        printf("Removed proc id: %d \n", id);
        printf("Child send: %d \n", WEXITSTATUS(status));
    }
    ```
    
    如果子进程没有正常退出（即没有调用 `**exit**` 或 `**return**`），则 `**WIFEXITED(status)**` 将返回 0。  
    没有正常退出自然也就不会有WEXITSTATUS所提取的返回信息。  
    
- 解决僵尸子进程
    
    - 书中p172 remove_zombie.c
    - 我发现SIGCHLD这个signal，并不是没一个子进程return/exit都会执行一次handler。如果两个子进程在及其相近的时间内结束，handler只会被调用一次。这时如果handler内只有一个非阻塞的waitpid，其他结束的子进程就会被漏掉。
    - waitpid的返回值
        - >0：处理的子进程的pid
        - =0：还有子进程活着
        - 1：没有子进程了
    - 所以可以使用如下这种方法将同时退出的子进程一网打尽。
    
    ```C
    void read_childproc(int)
    {
        puts("handler");
        
        int status;
        pid_t id;
        // to collect return value of the child process
        while((id = waitpid(-1, &status, WNOHANG)) > 0)
        {
            if(WIFEXITED(status))
            {
                printf("Removed proc id: %d \n", id);
                printf("Child send: %d \n", WEXITSTATUS(status));
            }
        }
    
    }
    ```
    
- accept是一个Linux system call。当它被signal打断时会返回-1
    - 使用perror打印错误信息：perror("accept() error"); ⇒ accept() error: Interrupted system call
    - 但这并不是说accept出错了，如果返回-1我们继续就好
        
        ```C
        while(1) 
        {
          clnt_sock = accept(serv_sock, (struct sockaddr*)&clnt_addr, &clnt_addr_size);
          if(clnt_sock == -1) 
             continue;
          else
             puts("Client connected!");
          pid = fork();
          if(pid == 0)
          {
             while((str_len = read(clnt_sock, message, BUF_SIZE)) != 0) 
                write(clnt_sock, message, str_len);
             close(clnt_sock);
             puts("client disconnected...");
             return 10;
          }
          else
             close(clnt_sock);
        }
        ```
        
- 套接字的关闭
    - 当套接字的输出缓冲被关闭，会发送FIN报文并发送EOF。
    - 当指向套接字的全部文件描述符（多个进程）被关闭后，输入输出缓冲才会被同时关闭，但你也可以通过 shutdown(sock, SHUT_WR) 提前关闭写入缓冲并发送FIN+EOF，之后即使任意进程有套接字的描述符，也不可以继续写入了（写入会报错）。
- 如果注册了Ctrl+c的信号处理器SIGINT，那按Ctrl+c就不会立即中止程序而是由handler自行管理。如果handler没有exit，那执行完handler程序就会继续。

# 第十一章

- read pipe也跟read 套接字差不多，nbytes指定的是最多可以读取几个字节，但只要pipe内有数据read就会读并返回，实际读取的可能小于nbytes。
- read一个管道时，当所有写入端被关闭后read才会返回0.
- 父进程退出会弹出下一条命令提示符，如果子进程还有输出的话就会打印在下一条命令中
    
    ![[_attachments/Untitled 9 7.png|Untitled 9 7.png]]
    
- accept(serv_sock, (struct sockaddr*)&clnt_addr, &clnt_addr_size);
    - 存入clnt_addr_size的实际大小可能比 sizeof(clnt_addr) 小，这可能是需要单独保存一下的原因。
- 刷新缓冲区
    - 跟printf写入stdout类似，写入文件的时候也有个缓冲。fwrite的数据不会被立刻写入文件，直到缓冲区满了或fclose关闭。
    - \n似乎不会触发刷新缓冲区，如果想让数据立刻写入文件需要手动 fflush(fd)。

# 第十二章

- 为什么不能用 set[fd] = 1 这样直接注册文件描述符
    - fd_set是一个struct，不是array，所以本身不是指针。
    - fd_set是一个bit数组，数组每次位移至少是一个字节 (char[])
- fd_set内包含一个bit类型数组，长度是进程可创建最多文件描述符数。
- maxfd就是当前进程一共有几个文件描述符，它告诉select包括maxfd之后的bit都不用查了，因为那些文件描述符都还没被创建。
- select监视三种**事件**
    
    1. **可读事件**：表示对应的文件描述符有数据可读。
    2. **可写事件**：表示对应的文件描述符可以进行写操作。
    3. **异常事件**：表示对应的文件描述符发生了异常情况，比如套接字被对端关闭。
    
    - select会不断遍历fd_set，如果有文件描述符发生了对应的事件，就标记fd_set并返回发生事件的文件描述符的个数。如果没有任何描述符发生对应的事件，就继续阻塞。
    - 只要输入缓冲区里还有未读数据，就会一直触发可读事件，select会一直返回（条件触发）。
    - 如果不需要监视其中一些状态，参数给0即可。
- select基本工作原理：调用select后进程睡眠进入阻塞状态，当有监视的事件后轮询所有监听的描述符并更新结果集，最后返回。
- 每次调用select后都会替换struct timeval的成员为超市前剩余时间，所以每次调用select前，都需要重新初始化timeval结构体变量。类似的，因为select都会改变fd_set中的值，所以每次调用前都得重新复制一份。
- timeinval的秒和毫秒可以设不同的值（并不必须是1:1000的比例关系），超时时间是二者的和。
- 一般来说，当服务端套接字的`**readset**`状态变化时，可以理解为有新的连接请求到达（有新的连接请求已经到达并在等待队列中排队），并且可以通过调用`**accept()**`函数来处理这个连接请求。

# 第十三章

- TCP紧急消息OOB
    - [TCP紧急模式理解心得_tcp紧急指针作用-CSDN博客](https://blog.csdn.net/qq_41681241/article/details/86583419)
    - 当紧急消息到达缓冲区时，触发SIGURG信号或者导致select状态改变。所以如果两条urgent message同时到达缓冲区，只会触发一次。
    - 不知道为什么在Linux的信号处理handler中每次recv(…, MSG_OOB) 只能读到一个字节，但在windows select中一次可以读到多个字节。
    - Linux: 服务端在设置sigaction之前必须先用fcntl将clnt_sock事务全权交给当前进程管理（默认由操作系统管理），所以fcntl必须在accept拿到连接socket的fd后才能调用。所以客户端在connect之后sleep个一段时间，不要马上发消息，等服务端signal handler设置好后就可以正常触发SIGURG了。
- 一次read可能是一部分write，也可能是多个write。不能对这部分做任何假设。
- 能确定的是
    - write的数据最终肯定会被发出去，只是发送时间长短不定。
    - 如果确定只读几个字节，那一次read肯定是够用的，一个包至少有几个字节。
- stdin有一个缓冲（之前scanf提到过），读取函数都是从这个缓冲中读。上一个函数没读完的下一个函数会自动接着读。

# 第十四章

- 127.0.0.1是一个特殊的IP地址，通常被称为“本地主机”或“回环地址”。在计算机网络中，它用于将数据发送到同一台计算机上的网络服务。当你访问本地主机时，数据不会通过网络传输，而是直接发送到本地计算机上。这个地址通常用于测试网络服务或在本地计算机上运行的程序。
- 在 Linux 中，使用多播时，默认的 TTL 值是 1。这意味着多播数据包只能在本地子网中传播，不能跨越路由器。如果你希望多播数据包能够在更多的网络中传播，你需要显式地设置 TTL 值
- 一个多播组内除了自己程序注册的地址，还有很多很多其他的地址（别人也在用这个组）。你发出去的消息其他组员也能收到。TTL就是多播覆盖的面积，不管传播面积有多大都会有组员。而TTL不到0时不会销毁的，所以TTL设的过大会影响网络流量。对于你自己的程序来说，TTL设置一个合理的数，只要你的接收端能收到就够了。
- 路由器连接着不同的子网，是本地子网的出口。
- 假设有一个网络的 IP 地址为 192.168.0.0，子网掩码为 255.255.255.0。在这个网络中，所有以 192.168.0 开头的 IP 地址都属于同一个子网，因为它们的网络地址部分相同。例如192.168.0.1，192.168.0.2。。。这些IP地址之间通信不需要经过路由器。
- 其他类型的指针转换到 void* 不需要显示转换。

# 第十五章

- 系统I/O函数（read, write, open）是无缓冲的，每次调用都会涉及系统调用。标准I/O函数（fread, fwrite, fopen）是有缓冲的，缓冲在用户内存空间的堆中，这些缓冲区通常由 C 运行时库动态分配，并在程序运行时维护。
    - [标准IO_缓冲区_物联网心球-CSDN博客](https://blog.csdn.net/weixin_28673511/article/details/131887996)
    - 输出终端默认行缓冲（遇\n刷新），输出文件、套接字默认全缓冲（FILE*被关闭或缓冲区满才刷新）。
- 这个I/O缓冲跟内核中的缓冲是两个概念，套接字的缓冲在内核里，终端输入的行缓冲也在内核里，标准I/O的缓冲是在这个基础上再加上的一层缓冲
    
    ![[_attachments/Untitled 10 6.png|Untitled 10 6.png]]
    
- 每个文件流（FILE 结构体）都有自己的输入缓冲区和输出缓冲区。当你打开一个文件时，系统会为这个文件分配一个 FILE 结构体，这个结构体中包含了文件的相关信息，包括输入缓冲区和输出缓冲区。
- fflush只刷新输出缓冲区。
- fgets最多读n-1个字符，自动给最后一个字符赋成0。
    - 碰到0/NULL字符会自动停止读取。
- fdopen不能违反原先描述符的权限。例子：int fd是O_RDONLY，fdopen指定”w”只写模式是不行的。
    - 描述符权限是在open时就确定好的，后面不能更改

# 第十六章

- fdclose
    
    ![[_attachments/Untitled 11 6.png|Untitled 11 6.png]]
    
    ![[_attachments/Untitled 12 5.png|Untitled 12 5.png]]
    
    FILE* 本质就是文件描述符套了个壳
    

# 第十七章

- 每一个epoll event都是一个“文件”，文件描述就是epoll_create返回的epfd。
- epoll_ctl中传入的epoll_event*只是一个临时buffer，用来通知操作系统event信息的，操作系统并不会真正使用这个在用户空间的struct epoll_event。所以每次调用完后可以更改struct的内容再传给下一个epoll_ctl。
- epoll_wait的_maxevents是第二个参数（发生事件的事项集合）可以保存的最大事件数量。
- 边缘触发：每当缓冲区中有新的数据时才触发事件
- epoll 通知原理：[(23 封私信 / 81 条消息) epoll的性能为什么比select好？到底是怎么实现的？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/22863976)
    
    select在阻塞状态下等待时，不知道哪些监视的事件发生了，所以每次都会遍历/检查整个监视事件的集合。
    
    所有添加到 epoll 中的事件都会与设备（如网卡等）驱动程序建立回调关系，也就是说，相应的事件发生时会调用这里的方法。这个回调方法在内核中叫做 ep_poll_callback，它把这样的事件放到 rdllist 双向链表中。当调用 epoll_wait 检查是否有发生事件的连接时，已经知道有哪些监视事件发生了，只需要检查 eventpoll 对象中的 rdllist 双向链表中是否有 epitem 元素，如果链表为空就继续等待。如果 rdllist 链表不为空，则把这里的事件复制到用户态内存中的同时，将事件数量返回给用户。
    
- 一个文件描述符只会在epoll_wait返回的events中出现一次。如果发生了多个事件，这些事件会被组合在events[n].events中一起返回。
    
    ```C
    while (1) {
        int nfds = epoll_wait(epfd, events, MAX_EVENTS, -1);
        if (nfds == -1) {
            perror("epoll_wait");
            close(fd);
            close(epfd);
            return -1;
        }
    
        for (int n = 0; n < nfds; ++n) {
            if (events[n].events & EPOLLIN) {
                // 处理可读事件
                printf("fd %d is readable\n", events[n].data.fd);
                // 读取数据或进行其他处理
            }
        }
    }
    ```
    

# 第十八章

- 进程结束时所有进程内的线程都会终止。
- 需要手动添加 `**-lpthread**` 来确保正确链接 `**pthread**` 库
- **静态全局变量**：如果全局变量用`**static**`关键字修饰，它的作用域将被限制在定义它的文件内。换句话说，其他文件不能使用`**extern**`来引用这个变量。这有助于避免命名冲突和非预期的修改。（与不加static的全局变量仅有这点不同）。
- 静态全局/局部变量在程序开始时就分配内存，生命周期直到程序结束。静态变量与全局变量储存在一个内存区。
    - [C语言中的全局、静态、局部变量_全局变量放在哪里-CSDN博客](https://blog.csdn.net/weixin_51954217/article/details/130427468)

- p305. semaphore.c例子的工作流程
    
    ![[_attachments/Untitled 27.jpeg|Untitled 27.jpeg]]
    

- 使用pthread_detach
    
    > `**pthread_detach**`函数用于将一个线程标记为“可分离”的状态。这意味着当目标线程执行完毕时，它的资源将被自动释放，而不需要其他线程调用`**pthread_join**`来等待它的结束。
    > 
    > 如果在调用`**pthread_detach**`时目标线程尚未执行完毕，可以放心使用该函数。即使目标线程尚未执行完毕，也可以安全地将其标记为可分离状态。当目标线程最终结束时，其资源会被正确地释放，无论它是否已经被分离。
    
    但如果进程终止了，所有线程都会被强行终止