# 集线器 hub 与 交换机 的区别
1. 集线器是半双工的，会导致大量不必要的通信
2. 交换机则是全双工的，将数据发送到指定计算机的目标端口上

## TTL
ttl有一个很重要的用途就是获取沿途各种服务器的信息，主叫方每次给ttl增加1，便可以获得ICMP超时报文，于是便可以依次获得所有的网关地址，但许多配置了防火墙的路由器并不会这么做，基于udp数据包的traceroute也可能因为主机根本就没有提供udp服务而被直接丢弃，当然这可以通过将目标端口号修改为30000以上的数字来实现，这样的话就可以获得一个“端口不可达”的报文，当然也可以直接使用tcp

## ring buffer

环形缓冲区（Ring Buffer），也称为循环缓冲区、圆形缓冲区或循环队列，是一种数据结构，用于在没有重新分配内存或覆盖数据的情况下高效地存储和管理固定大小的数据。环形缓冲区的主要特点是当它被填满后，再插入新数据时，会覆盖最早插入的数据。环形缓冲区通常用于实现数据流处理、操作系统内核、网络协议栈等场景。

环形缓冲区的工作原理如下：

1.  **初始化**：创建一个固定大小的数组和两个指针（或索引），一个用于表示读（read）位置，另一个用于表示写（write）位置。初始时，读写位置相同，表示缓冲区为空。
    
2.  **写入数据**：当需要向缓冲区写入数据时，首先检查缓冲区是否已满。如果缓冲区未满（即写指针与读指针不相邻），则将数据写入写指针当前指向的位置，并将写指针向前移动一个位置。如果写指针到达数组末尾，则将其移动到数组起始位置，形成环形结构。如果缓冲区已满，根据需求可以选择丢弃新数据、覆盖旧数据或等待缓冲区有空闲空间。
    
3.  **读取数据**：当需要从缓冲区读取数据时，首先检查缓冲区是否为空。如果缓冲区不为空（即读写指针不相等），则从读指针当前指向的位置读取数据，并将读指针向前移动一个位置。如果读指针到达数组末尾，则将其移动到数组起始位置。如果缓冲区为空，可以选择等待缓冲区有数据或返回错误。
    
4.  **缓冲区满和空的判断**：通常，我们使用以下方法判断缓冲区是否为空或已满：
    
    -   如果读写指针相等，则缓冲区为空。
    -   如果写指针紧邻读指针（写指针位于读指针前一个位置），则缓冲区已满。
    
    也可以使用一个额外的计数器来跟踪缓冲区中的元素数量，以便更直接地判断缓冲区的状态。
    

环形缓冲区的优点包括：

-   高效地利用内存空间：环形缓冲区只需要固定大小的内存，不需要动态分配或释放内存。
-   高效的数据访问：读写操作通常仅涉及指针或索引的移动，具有较低的计算开销。