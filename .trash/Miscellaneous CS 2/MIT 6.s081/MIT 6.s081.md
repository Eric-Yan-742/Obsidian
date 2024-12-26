- 关于多线程
    - 多个线程可以同时读取公有资源，但是此时不能有线程进行写操作
    - 只能有一个线程进行写操作且此时其他线程不能进行读操作，因为此时可能会读取到修改前的数据
- lab 7
    - 当过一个interrupt handler在执行中，外设不会产生其他interrupt。所以对于interrupt handler，并不存在多线程同时执行的情况，不需要锁。
    - [8254x_GBe_SDM.pdf (mit.edu)](https://pdos.csail.mit.edu/6.S081/2021/readings/8254x_GBe_SDM.pdf) 重点全在3.3
    - 每一个descriptor只有一个mbuf，并不存在对应一个chain的多个mbuf的情况，mbuf中的 `next` 至少在本lab中没有用到
    - `e1000_recv` 会调用多次 `e1000_transmit` 。所以二者都是 interrupt handler 的一部分。
    - 如果 E1000_TXD_STAT_DD 为1，说明网卡已经发送完成这个包了
    - 如果`e1000_transmit` 中 E1000_TXD_STAT_DD 为0，说明网卡还没发完前面那个包，也就是整个ring都没有空位子，包移除了，这时我们报错。
    - 如果 E1000_RXD_STAT_DD 为0，说明驱动已经读取过这个包了。这就是为什么每次读取之后都要把status bit清空为0
    - 对于 `e1000_recv` 包来的会比网卡产生中断的速度快，这样当 interrupt handler执行时，rx_ring中可能已经有多个待处理的包了。我们必须持续读取直到descriptor的DD为0时（已经处理过了）才结束。所以需要一个 `while(1)`
    - 对于 `e1000_transmit` 每次执行都只发一个包，所以不需要循环。
- lab 8: lock
    - `holding(spinlock)` is very useful when you are not sure if you need to `acquire` or `release` a lock.
    - Memory Allocator
        - In the stealing process, if there are no pages to steal, we just don’t steal. `struct run *r` will be 0 (null). The kernel allows this.
    - Buffer Cache
        - The `**ticks**` variable in `**trap.c**` is used to keep track of the number of timer interrupts that have occurred since the system started running.
        - Split the buffer list into 13 separate buckets, each with its own lock. Therefore, buckets don’t interrupt each other. Each bucket with 8 buffers. Use `blockno % 13` to ensure each block will only be hashed into one section, which avoids one block are cahced into several buckets
- lab 9: File System
    - [【MIT6.S081】 lab9 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/465636130?utm_id=0)
    - Big File
        
        ![[_attachments/Untitled 25.jpeg|Untitled 25.jpeg]]
        
        A total of 256*256+256+11 data block
        
    - Symbolic Link
        - `create` in sysfile.c already hold the lock of the new inode.
        - Whenever you finish using an inode in kernel transient code, use `iunlockput(ip)` . `itable` uses `ip->ref` to determine whether it should keep this inode in memory. if you don’t `iput(ip)` to decrement `ref` after using the inode, `itable` will never remove it from memory. Thus, `itable` will fill up quickly. You will soon run out of in-memory inodes to cache.