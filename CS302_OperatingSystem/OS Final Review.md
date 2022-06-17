# SUSTech OS Final Review

11910738 文颖潼 [TungMan0801](https://github.com/TungMan0801)

**声明：**

1. 本复习是基于2022S南方科技大学的CS302课程课件进行全面重点翻译，翻译为中文主要用于笔者更好的理解，而正式考试需要全英文作答。另外，本学期是疫情网课时期，无期中，所以期末考试将覆盖全部学习内容。
2. 本复习pdf并未包括Lec15(File System),但这也是考试的重点, 请结合自己的学习情况全面复习！
3. Review不能押题，不能保证此后教学方向和内容一致。若有错误请斧正~若您感到有帮助，请给俺留下支持！！XD

## L1 Intro

电脑是编程后可以自己做很多计算或逻辑操作的电子机器，组成参照冯诺依曼结构，有CPU， 设备控制器， 可以连通到memory的bus。电脑系统的组成则有硬件，操作系统，应用程序， 用户（自下而上）。其中，操作系统控在各种应用程序和用户之间控制和协调硬件的使用。

### 操作系统

#### 释义

一组能使计算机在一种易于使用的方式下正确且高效运行的系统软件。

管理和控制计算机系统中各种硬件和软件资源、合理地组织计算机工作流程的系统软件，是计算机之间的接口，使得用户问题更好解决，计算机系统更方便使用，计算机硬件被高效使用（硬件抽象）。

咱们平时常接触到MacOS，Windows和Linux都是操作系统。

#### 作用

它既是资源管理者（很多物理设备，处理资源分配），也可以是控制程序（防止不当的使用）。

它包括一个叫内核的软件程序，管理者所有的物理设备，也会在顶上公开某些功能如系统调用给别人来修改以调整内核操作；也包括其他的helper程序。比如Shell, GUI，Browser等

以下为五大基本功能

1. **存储管理**：存储分配与回收，存储保护，地址映射和存储扩充
2. **处理机管理**：进程调度，进程控制，进程同步，进程通讯
3. **设备管理**：缓冲管理，设备分配与回收，设备处理，设备独立性
4. **文件管理**：文件存储空间管理，目录管理，文件读写和存取控制，文件操作的一般管理
5. **用户接口**：命令接口，程序接口，图形接口

#### 三个重大用处/特征：

1. **Virtualization虚拟化**

   虚拟化CPU：在单个CPU上运行多个程序

   虚拟化内存：为每个进程提供运行在自己的内存地址空间的错觉（艹哈哈哈）

2. **Concurrency并发**

   可以运行多线程程序并确保其正确执行

3. **Persistence持久性**

   将（易失性）数据写入持久存储的地方性能。性能、防撞能力

#### 发展过程

无操作系统->批处理系统(单道，多道)->分时系统->实时系统->网络操作系统->[分布式](https://so.csdn.net/so/search?q=分布式&spm=1001.2101.3001.7020)操作系统->嵌入式操作系统

### 进程 Process (in L2)

进程是正在执行的程序，一次只有一个状态。

有这三种状态：**就绪Ready, 运行Running, 等待/阻塞Wait/Block**

**进程是程序的一次动态执行过程，是资源分配的最小单位**

进程需要资源来完成任务，进程会按顺序一次一行运行指令，直到结束。单线程的进程会有一个程序计数器来确定下一个要执行的指令，多线程的则每线程一个。

通常，系统有很多进程，一些用户，一些操作系统并发地在一或多个CPU上跑。

进程由**PCB（进程控制块），程序段，数据段**三部分组成。

PCB则包括 进程标识符，进程调度信息，处理机状态信息，进程控制信息。

后面L3还会详细介绍进程。

#### 进程管理 Process Management：

创建和删除用户和系统进程;暂停和恢复进程;提供流程同步机制;为过程通信提供机制;提供死锁处理机制。

单处理机下，进程由运行，就绪和堵塞三态。创建即就绪。并发的程序中，可以全部阻塞，同时只能最多一个运行态，同时只能最多n-1个就绪态。

#### 内存 Memory

DRAM(动态随机存取存储器)是所有台式机、笔记本电脑、服务器和移动设备使用的主要内存。

执行期间，CPU仅直接和主存储器交互。

OS管理内核和进程的主要内存

#### 内存管理 Memory Management:

内存管理决定了的内存里有什么，优化CPU利用率和电脑对用户的回应。

内存管理活动：跟踪当前正在使用内存的哪些部分以及由谁使用；决定哪些进程（或其中的一部分）和数据要移入和移出内存；根据需要释放内存空间。

#### 存储管理 Storage Management

操作系统为信息存储提供了统一的逻辑视图。抽象物理属性到逻辑存储单位上（比如文件）；每一个介质都有设备控制，如磁盘驱动器和磁带驱动器。

文件系统管理：文件被整理组织到文件夹里，大部分系统的Access Control是用来决定谁可以访问哪里。操作系统活动包括：生成和删除文件与文件夹和操作他们；

#### I/O子系统 I/O Subsystem

隐藏来自用户的硬件设备的特性，负责I/O内存管理，缓冲传输数据临时存储数据

#### 保护与安全 Protection and Security

OS可以定义控制进程或用户访问操作系统的防御机制，还有抵御系统内外部攻击的系统防御。



## L2 OS Basics

### 双模操作系统

操作系统因人需要一个能处理低级I/O的更大的库，来保护内核/硬件免受错误和恶意程序的攻击而生。

双模即内核模式和用户模式，他可以允许操作系统保护本身和其他的系统组件。

通过模式比特位判断系统要运行哪种模式，给指令权力级别，使得某些指令只可以在内核模式下运行。

要支持这种双模式系统所需要的条件：

- 决定状态的一个比特位
- 被赋予了权限等级的某些指令或者行动，只可以在内核模式下运行
- 能够从User变为Kernel，且能保存好User(小心地放在一边)
- 能够从Kernel变为User, 清掉系统模式并恢复为合适的User模式

其中最后两个是模式转换。

#### 模式转换

**模式转换**（Mode Transitions) 分为三种

1. **系统调用 System call**

   意思就是有进程请求了系统的服务。系统调用算是操作系统提供的程序服务接口，可以说是进程外的一个函数调用，但这“函数”不占用地址，而是有自己的id, 这些id作为一个表的index被接口维护。通常是C/C++写的（高级语言）。在寄存器和exec syscall可以整理系统调用的id和参数。

   ![image-20220602211449417](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220602211449417.png)

   System Call接口会调用操作系统内核中想要的System Call, 并返回系统调用的状态和所需值。

   程序员只需要遵守调用规定和知道OS将做什么就可以了，大部分细节是可以不知道的。

   几个系统调用类型：`Process Control`, `File Management`, `Devide Management`, `Information maintenance`, `communications` and `Protection`. 

   <img src="C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220602212338810.png" alt="image-20220602212338810" style="zoom: 50%;" />

2. **中断 Interrupt**

   由于**外部**异步事件（而非用户的进程）而发生了上下文切换。比如时钟、IO。

3. **Trap or Exception**

   用户**进程内部**的同步时间触发上下文切换（context switch）。比如保护冲突，除数为0..

   

### 内核结构 Kernel Structures

看起来分了好多种！！

##### Monolithic Kernel 单片内核

单片内核是一种操作系统软件拥有访问I/O设备、内存、硬件中断和CPU堆栈的所有权限的框架。单片内核包含许多组件，如内存子系统和I/O子系统，通常非常大包括文件系统、设备驱动程序等。单片内核是Linux、Unix和MS-DOS的基础。

##### Micro Kernel 微内核

##### Hybrid Kernel混合内核 （上面两个的混合）

##### Exokernel 外核

安全多路复用，内核相对小；操作系统抽象

总计OS的设计思路会随着用途、目标、硬件选择的影响有很多不同。总之要为用户和系统着想，对于用户来说要好学好用安全快速，对系统来说要可靠灵活高效且易于维护。

### 操作系统服务

![image-20220603054736123](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220603054736123.png)

出现在services的八项：用户接口，程序运行，I/O操作，文件系统操作，交流，错误检测，资源分配，Accounting， 保护与安全。

## L3-4 Processes 进程

进程定义可以看[第二章中介绍的进程](#进程 Process (in L2))。

总之就是个在跑的程序！操作系统加载这个进程的代码和静态S数据放在堆栈上。

每一个进程都有唯一的id(PID), 用getpid()可获取。此进程号可以区分不同的进程，然而进程号是有限的，所以需要释放资源。进程在这个程序跑完后生命就没啦，terminated了。

先前提到的系统调用是内核与进程的一个交互，像上文[模式转换](#模式转换)中的系统调用提到一样被安排调用数字，而参数传递中所需的信息可能比这个数字量都大。以下三个方式向OS传递参数：```寄存器```，```块```,```栈```。系统调用中，大部分OS提供标准c库来提供库API调用。比如fopen()其实是用了open()。

### 进程的创建

父进程能通过```fork()```创建子进程，子子孙孙形成一个进程树。通常我们就用pid(process identifier)来识别和管理。 其中资源共享也分三类，看孩子能和爹妈共享多少资源。孩子和爹妈是并发运行，有了wait()两者就可以以固定顺序运行，父进程会在子进程终止前保持等待。（否则两者并发运行的顺序是不确定的）

fork()产生的子进程和爹拥有一样的programe counter，code, memory, opened file，但是不一样的```Return Value of fork```,```PID```（子进程pid一般是父进程pid+1）,```Parent process```, ```Running Time```和```file locks```。

```exec()```(其实我没太看懂)能够替换用户空间信息，重写子进程以有不同功能。

```	wait()```使父子同步，可以让进程等待任意子进程。waitpid()的话就会由参数决定等哪一个孩子，可以检测孩子的不同状态变化（通过信号恢复和停止）：当wait()被执行时，当前进程会被挂起(suspend)，直到 子进程结束，子进程结束后会给父进程发送 SIGCHLD 中断，wait()收到中断后会结束阻塞并使系统回收 子进程的相关资源。

上面三个方法可以实现一个shell了

### 进程：Kernel View

#### 进程控制块Process Control Block(PCB)

上面有很多与进程相关的信息，比如状态，程序计数器，CPU寄存器，CPU调度信息，内存管理信息，会计信息，IP状态信息等。

我们以ucore里面的PCB为例子, 含有下列信息

![image-20220603225213397](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220603225213397.png)

![image-20220603225224523](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220603225224523.png)



进程控制块以多个队列的形式相连接。

##### 线程 Threads

一个进程可能会有多个线程，线程是轻量级的。PCB可以拓展去包含每个线程的信息。

进程中的不同线程可以共享PID和file descriptor.

#### 进程之间的切换

进程一旦在CPU上跑，他只会在以下情况交回CPU控制权：system call;exception;inerrupt，也就是上面的模式转换三个方式。否则就靠合作调度时OS干等和非合作调度的时钟中断。

交回控制权后，OS会结束任务，通过问CPU调度程序决定下一个运行的进程是谁。如果下一个要执行的进程和上一个不同，这样叫做**上下文切换(context switch)**

#### 上下文切换 Context Switch

在上下文切换期间，系统必须保存旧进程的状态，并为新进程加载保存的状态（存于PCB中）。上下文切换不能太频繁哈。

#### 僵尸状态

```exit()```使得进程变成僵尸状态在以下情况：进程调用exit();它异常终止;进程在main()返回

wait()和waitpid()可以获取僵尸子进程，因为僵尸会占用总数有限（在课件中kernel设定的pid_max是32768）的pid，所以需要清理他。

关于wait()的执行，这里可以排列组合出几种情况：

| 情况                                     | 结果                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| 父进程执行wait()，父进程比子进程后结束   | 子进程结束后，父进程会通过wait回收子进程的相关资源           |
| 父进程执行wait()，父进程比子进程先结束   | 有了wait（）这种情况**不会发生**                             |
| 父进程不执行wait()，父进程比子进程后结束 | 子进程虽然已经结束了，但是父进程未结束并且未回收尸体 （资源），子进程变成了僵尸（Zombie process）。 |
| 父进程不执行wait()，父进程比子进程先结束 | 父进程子进程先结束时，子进程会被过继给init进程或者注册过的祖父进程以确保每个进程都一定有父进程（reparenting)。比如init也会调用wait()。由新的父进程为他们收尸，最后也会被释放。 |

#### 终止进程/中断信号 

```kill -9```可以强制终止进程，9其实就是中断信号SIGKILL，这个信号是不能被阻塞、处理和忽略的。我们提到说子进程结束时发送SIGCHLD给爹，是允许被忽略的。

代码中可以通过 ```int kill(pit_t pid,int sig)``` 发送中断信号，其中pid代表要接收进程的进程号， sig代表要发送的中断信号的中断号。```signal()```是用来注册信号处理方式的，``signal(signum,sig_ign)``可以忽略对应的中断信号。



## L5 Address Translation

地址空间是操作系统的关键抽象概念。

地址转换需要硬件和软件的配合！迄今为止有两种方案1基础和便捷 2分段

地址空间的段代码segments in an address space: `Code segment`,`Stack segment` and `Heap` .  分别是底部指令；本地变量、参数、返回值；内存分配。

这个16KB的地址空间只是抽象的，地址空间里的0kb可不是物理内存里的0kb。这个地址空间是一个幻象~32bit CPU 支持上限为2^32字节的地址空间。

#### Memory Virtualization

内存虚拟化是对私有的大型地址空间的抽象。用于在单个物理内存上面运行多个进程。

地址转换就是**虚拟到物理**的转变。比如0KB -> 320 KB

一个字节是8位 1byte=8bits。DRAM: DynamicRAM动态随机存储器

内存虚拟化应该有三个特质 **Transparent透明的, Efficient高效的, Protection提供保护**。

进程不可见内存虚拟化，就像在一个私有内存上跑；转换快，不浪费空间；内存隔离，进程之间不能互相访问。这是构建可靠的OS的关键原则。

地址转换是CPU和OS软件之间的协调。

CPU中的**Memory management unit(MMU)** 会转换指令使用的虚拟地址为DRAM可懂的物理地址。CPU会介入每一次的内存访问。（Interposition:提高透明度的技术）。

而OS负责设置正确转换的硬件，跟踪空闲的位置和使用的位置，保持对内存使用的控制。

![image-20220612195257942](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220612195257942.png)

这个图我没理解错的话：通过MMU获取虚拟地址0x12345678的物理地址0x13579A00, 这串地址会对应到右边的mov命令那里。CPU执行这段命令的时候，`move 0x12345678(虚拟)`又相当于`move 0x13579A00（物理)`,然后就会执行将地址为0x13579A00的数据0x0000000A的移动，变到EAX处去。

### 虚拟地址如何转换为物理地址？

#### Base & Bounds: Dynamic Relocation动态重定位

会有两个硬件寄存器，一个是base,一个是bounds。  

图中的Process A就是base 320KB, bounds 64KB.				<img src="C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220612201540705.png" alt="image-20220612201540705" style="zoom: 50%;" />

##### 局限性：

1. 内部分裂Internal Fragmentation：会浪费堆栈之间的空间。
2. 不能支持更大的地址空间：空间等于分配到的
3. 很难进程间共享



每一个segment有一对base/bound。 每个seg都会指向一个物理内存中不同的区域。

片段的实现： B/B寄存器被一个表安排了，还会有他们对应的SegID。SID用来标序，Base加上虚拟地址offset可以生成物理地址。如果offset超出范围就可以检查出错误。显式使用：从高位寻址；隐式使用：用于代码提取的代码段



![image-20220612202708107](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220612202708107.png)

##### Segmentation的一些问题

- OS上下文切换必须也要存和恢复所有对儿寄存器
- 一个段可能会增长也可能不会，虚拟连续内存物理上可能不连续。
- 使用可变大小的段管理物理内存的空闲空间
- 外部碎片External fragmentation: 分配的段之间的自由间隙如果分配更多空间，分段也可能有内部碎片比需要的多

##### 一些解决方法：

- 用可变大小的段管理物理存储器的空闲空间。比如固定大小的Seg, 物理内存弯曲成固定大小的块，证书索引。
- 外部分裂External Fragmentation：固定大小seg, 每一个请求空间都能够满足

### Memory Allocation内存分配策略

OS需要管理所有的空闲物理内存区域，一个基本的方法就是维护一个空闲slot的链表。一个理想的分配算法就是又快速又可以最大限度的减少碎片。

下面是三个策略，没有最好的选择哈~

#### - - -Best Fit

搜索空闲列表，找到和请求一样大或者更大的空闲内存块，然后返回这其中最小的一个。

**优点**：满足最小外部分裂 Minimal External fragmentation

**缺点**：穷举搜索比较慢

#### - - -Worst Fit

搜索空闲列表，找到和请求一样大或者更大的空闲内存块，然后返回这其中最大的一个。

**优点**：在物理内存里面留下更大的“洞”

**缺点**：穷举搜索比较慢，严重的fragmentation

#### - - -First Fit

找第一个满足大于等于所需空间的地方然后返回

**优点**：快！

**缺点**：会用小块污染空列表的开头

![image-20220612214633692](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220612214633692.png)







## L6 Paging 分页

上面我们提到了分段的各种，分页其实就是把这以下的想法放在一起

1. 物理内存分为固定的大小(概念上)，每一个被称为**Page Frame**, 一般大小就是1~16KB，绝大部分ISA使用4KB。
2. 虚拟地址空间备份成一样大小，每一个被称为**Page**
3. Page映射向Page frame，一对一映射；多对一映射->memory sharing
4. 每个进程一个page table, 驻留在物理内存中，一个entry条目对一个虚拟->物理转换。

![image-20220612221326093](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220612221326093.png)

```
Physical Address = Physical Page Number || Offset
Virtual Address = Virtual Page Number || Offset
```

分页中的地址转换就是虚拟页面到物理页面框架的映射~

虚拟地址的长度决定地址空间的大小，偏移量offset的长度决定`page`/`page frame`的大小。假如有m位的虚拟地址和k位的偏移量，地址空间的大小：$2^m$; page的大小：$2^k$，共有$2^{m-k}-1$个page。 32-bit虚拟空间，4KB page:m=31，k=11  【感觉忘了为什么】

PTE数量=2^(m-k), k=pages总大小指数， m=总空间大小指数

Page table的大小=PTE数量 * PTE大小

#### Page Table Entry页表条目

就是页表的entry, 简称为PTE。 除了PFN，PTE还含有

1. A Valid bit。如果是没有有效映射的虚拟页，他的valid bit
2. Protection Bits. 允许是否可读可写可执行
3. A access bit, a dirty bit and a present bit. 分别代表是否可以访问、这个page是否在被放入内存前修改过，现在在物理内存还是在磁盘上。



### Multi-level page tables多级页表

这是一个二级页表的例子。首先ptr指向Page table。

![image-20220612235541084](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220612235541084.png)

一个`page table`也会占用一个`page frame`, 比如4KB的page装4Byte的PTE能装1k个，这样全是entry的话又装了1k个table了，所以页表会形成一个tree。不过虽然理论上可以装1K个二级页表，但只有一个subset是有效的，有效的页表子集会存在内存中，其它则在磁盘。



### Other page table structure

除了分层的页表Hierarchical page table，还有hashed和inverted

Hashed: 输入VPN出来的是哈希页表的inde;可以用链表解决碰撞

Inverted: 倒排页表，整个系统用一张表，每个entry对应一个物理page frame, 有pid和vpn。页表现线性查找整个表，很难共享内存。



### Translation lookaside buffer

Paging时间复杂度：在地址转换期间需要额外的内存引用，三级页表需要3个额外的内存读取

转换后备缓冲区(TLB)是一个硬件缓存，是MMU的一部分。给PTE缓存，保存可能被重用的转换（更换机制：LRU, FIFO, random）；每一个entry都持有虚拟到物理的映射

执行虚拟到物理的转换之前，TLB会用VPN来查找。hit就是找到了VPN和同一个entry用的PFN。miss就是没找到，页表walk。较大的page大小能提高hit rate

- [ ] 这里有一个TLB的图例，还没看视频

##### 为什么要TLB呢？

1. 理想状况下一个page table会遍历走完整个page。TLB比page table小哈；
2. Spatial locality空间局部性:顺序执行指令、栈上的局部变量、堆上的数组
3. Temporal locality时间局部性：同一个页面的访问时间接近



## L7 Demand Paging 请求分页

64-bit机器支持最多4EB的地址空间，应用可能会用更多物理内存中的空间。为了解决，可以隐藏地址空间中没在用的部分在下一级存储中，速度较慢但更大了。

**memory overlays**记忆覆盖：掌管在内存和硬盘之间移动数据的应用，比如调用一个函数需要保证这段代码要在内存里。

**demand paging**：操作系统设置PTE，虚拟页映射向物理内存或者硬盘上的文件。进程会看到地址空间的抽象，OS决定数据存在哪里。

 #### Swap Space交换区

一些空间是存储在硬盘上的分区或文件。OS会在它和memory之间交换pages。交换区在概念上划分为page大小的单元，OS维护他们的磁盘地址。

请求分页在需要的时候从磁盘加载page到内存中，这就是demand paging

Data会在这些page的单元中传输。

##### 两个可能的on-disk location：

1. swap space： 由OS创建用于磁盘上临时存储页面
2. Program binary files: 从二进制文件夹来的代码也只会在运行时被加载进内存，只读。

这里的物理内存可以看做是一个cache，block size就是一个page(4 kb)。采用LRU、Random或LIFO的page replacement政策。miss后去更低级别填充page.

#### Page fault页面故障

![image-20220614180247417](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220614180247417.png)

如果present bit=0就会引出一个页面故障异常，OS参与到地址转换来。handler会在物理内存找到空闲的page frame（[replacement](#Page Replacement)），从磁盘获取page存在物理内存中。接下来从页面故障异常返回，CPU重新执行访问虚拟内存的指令。TLB条目从PTE中加载。

##### Page Replacement

以替换政策决定，找到要替换的page frame，如果已经dirty就写回磁盘，更新指向PTE指向的page frame，使得这些PTE的所有TLB无效。

什么时候会触发页面替换呢？主动地page replacement一本都可以带来更好的性能，总会在系统中保留一些空闲的page frames。Swap daemon：回收page frames的后台进程，低水位线low watermark是触发swap deamon的阈值；高水位线是停止回收的阈值。

##### Fetch page from disk

从寄存器中确定故障虚拟地址，找到PTE中页面的磁盘地址(PFN应该在哪里已存储)， 向磁盘发出请求，将页面提取到内存中等等……(可能会很长时间，上下文切换！) 当I/O完成时，更新页表项，比如PFN，present bit。

---

### Page Replacement页面替换政策

#### Effective Access Time有效访问时间

EAT = Hit Rate x Hit Time + Miss Rate x Miss Penalty

<img src="C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220614184408573.png" alt="image-20220614184408573" style="zoom:50%;" />

<img src="C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220614185343075.png" alt="image-20220614185343075" style="zoom:50%;" />



##### Cache Misses:三种C

1. **Compulsory Misses强制性:** 这个页面以前没被从memory中获取过，这叫冷启动miss: cold-start miss. 可以靠预取prefetching解决
2. **Capacity Misses容量:**指的是不够内存了，需要以某种
3. 方式增加可用的内存大小。1是增加DRAM的容量，2是如果有多个进程在内存中，可以调整分配给他们的内存百分比。
4. **Conflict Misses冲突:**全关联高速缓存不会有这个问题。



#### •Optimal / MIN

替换最长时间不使用的页面，理论上导致最少的页面错误~

![image-20220614192431592](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220614192431592.png)



#### •FIFO(First in first out)

首先丢弃最旧的页面。有可能会扔掉经常使用的页面而非不常使用的页面。

• Suppose we have 3 page frames, 4 virtual pages, and following reference string:

![image-20220614192445816](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220614192445816.png)



#### •RANDOM

为每次替换选择随机页面。 相当不可预测，很难做出实时保证



#### • Least Recently Used （LRU）

替换最长时间未使用的页面，考虑的是程序的时间位置Time locality：如果一个页面已经有一段时间没有被使用，那么它不太可能在近期被使用将来的。

![image-20220614192539354](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220614192539354.png)

要实现LRU的话必须要有硬件支持，访问内存时有必要更新操作系统中的数据结构。开销就是每一次内存访问时都要增加一次内存写操作。扫描整个内存以找到LRU的那个。

带参考位Reference Bit的LRU近似，页面访问的顺序大致分为用过或没用过。

##### Clock Algorithm

在循环列表中安排物理页面，第一次访问时CPU将参考位设为1。CPU维护一个指针，检查当前页面的reference bit, 当替换发生的时候检查reference bit， 是1则把它设为0，移到下一页；如果是0那么这个页面没被访问过，很不戳，停止。

##### Clock Algorithm with Dirty Bit

dirty bit来标记它是不是最近被修改过。



#### • Least Frequently Used（LFU）

替换掉未被多次访问的页面。考虑的是程序的空间位置spatial locality: 如果一个页面已经被访问了很多次，也许它不应该被替换，因为它显然有一些价值。



##### Bélády’s Anomaly

理想：memory增加，miss rate下降。对LRU和MIN是这样的





## L8 Linux Memory Management

每个进程的虚拟地址空间在用户和内核部分之间划分。为什么内核内存被映射到进程中的地址空间呢？因为trappes到终端时不需要改变page table,没有TLB flush。每个地址空间中的内核内存是相同的。

###### Kernel logic address内核逻辑地址

大多数内核数据结构：page able, 每个进程的内核堆栈...；从0xc000000开始，指向连续物理地址。

###### Kernel Virtual address内核虚拟地址

虚拟连续内存，vmalloc

##### Large page support

x86 support 4KB, 2MB, 1GB pages

 • Hardware enforces page alignments 

• 4KB pages are 4KB aligned (lower 12 bits are 0) 

• 2MB pages are 2MB aligned (lower 21 bits are 0) 

• 1GB pages are 1GB aligned (lower 30 bits are 0)

![image-20220615045839416](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220615045839416.png)





#### Page Cache页面缓存

物理内存中的一个区域，用于保存储存在硬盘或者其他永久存储器上面的数据内存映射文件：所有二进制文件和动态库；匿名内存：交换区中堆栈。页面缓存会跟踪看entry是干净还是脏的，脏的定期写回磁盘。

用2Q replacement policy，整俩list:活跃和非活跃的(LRU Queues)， Replacement在非活跃list中发生。



## L9 CPU Scheduling

当多个进程在一个CPU上跑，就很需要调度了。

两种进程： **CPU bound**（大部分运行时间在CPU上，ut>st）和 **I/O bound**（大部分时间在io上，st>ut，比如网络程序）

CPU scheduler程序选择一个准备好执行的进程，然后分配CPU给他。当以下情况发生时，CPU调度器可能会做选择：

- Running -> Waiting
- Running -> Ready
- Waiting -> Ready
- Terminates.

调度算法仅在在1/4情况是非抢占的，其他的调度算法都是抢占式preemptive的。

**优化标准**： 给定一组进程，有到达时间Arrival Time和CPU Requirement（需要的CPU burst time）， 最小化平均周转时期和平均等待时间，减少上下文切换的次数。

周转时间：结束-到达

### Shortest-job-first(SJF)

最短工作优先，也分抢占式和非抢占式。抢占式时来新活了而且剩的少就先做。

### Round-Robin(RR)

循环调度（RR算法）是抢占式的，每一个进程都有一个quantum时间量，也就是允许执行的时间）。当这个quantum用完了的时候，这个进程就会被抢占，调度者会来选择下一个进程，它得拥有非0的quantum。如果所有进程的都用完了就大家一起remake回初始值。因此，这是一个一个一个运行的循环列表。

### Priority Scheduling

简单地说就是优先级最高的先，抢占式中，高优先级会直接夺舍；非抢占式则会进入等待队列。优先度也会分为静态和动态。

### Multiple queue priority scheduler

多个优先级，在每一个优先级类中，可部署不同的调度者。比如Linux scheduler



## L10 Threads and Concurrency线程和并发

回顾一下，线程就是程序执行的抽象。单线程程序有一个执行点，多线程有多个。每一个线程都有自己的私密的执行状态：program counter, a private set of registers, private stack for thread-local storage, CPU要从两个之间切换需要上下文切换。一个进程里的新城共享计算资源，比如地址空间、文件、信号。

#### 使用线程的好处

1. **提高并行度 Increase parallelism**
2. **避免因为缓慢IO而阻塞程序进程Avoid blocking programe progress due to slow IO**： 支持IP和其它活动同时堆叠。
3. **允许资源共享allow resource sharing**



#### 线程的实现：

- 用户级别线程：用户级别线程库来线程管理，OS不需要知道这个级别的线程
- 内核级别线程：内核来完成线程管理，OS会注意到每一个此级别线程

这两个线程有一对一，一对多（一个kernel对多个user）和多对多,各有优缺点。

1. **多对一映射**：优点:线程间的上下文切换很便宜；缺点:当一个线程阻塞I/O时，所有线程都会阻塞
2. **一对一映射**：优点:每个线程都可以独立运行或阻塞；缺点:需要进入内核模式进行调度
3. **多对多映射**：许多用户级线程在较少或相同数量的内核级线程上多路复用；优点:两全其美，更加灵活；缺点:难以实施



## L11 Synchronization同步



同一个进程的线程共享地址空间，进程可能会需要和彼此交流：比如信息共享，计算加速，模块化和隔离。

当遇到共享对象、多个进程或线程、并发地进行，很可能会出现Race Condition。

这代抱着执行的结果取决于特定的访问资源顺序。那这就很恐怖了类！

##### 解决办法1： Mutual Exclusion互斥

虽然对象是大家都共享的，但是不允许你们同时访问，只可以轮流来。

**Critical Section**：访问共享部分的代码段，它要尽可能的紧密，当然其实可以访问多个共享对象哈。实现要求：1. 互斥Mutual Exclusion；2有界等待Bounded Waiting限制进程进入临界区的次数；3进程Progress

**解决办法2： Disabling Interrupts**关掉中断

**解决办法3： Locks加锁**

- [ ] 这里省略了很多没看

### Sleep-based Lock: Semaphore信号量

这是一个结构，里面含有一个计数资源的证书还有一个等待列表。诀窍依然是section entry/exit功能的实现。

必须保证没有两个进程可以在同时使用同一个信号量的sem_wait()和sem_post()。因此，这个实现就会关键在于等待和信号量,需要在单处理机上禁用中断。

哲学家吃面~



## L12 Deadlock 死锁

#### Starvation VS Deadlock

饿死和死锁是有点不同的，starvation是指线程无限期地等待，比如低优先级的等高优先级的资源。而死锁就是循环等待资源。A拿着资源a等资源b, B拿着资源b等资源a,starvation还可以结束，但是死锁没有外力因素不能够自己了结自己。

### 死锁发生的四个条件

1. Mutual Exclusion互斥： 一次只有一个线程可以用这个资源
2. Hold and wait：拿到新的之前不放手
3. No premption:非抢占式的，资源只能由所有者自愿放弃
4. Circular wait：循环等待

那如果说就是能够侦查到死锁也是好事，下面是两个检查算法。

### Detection Algorithm

![image-20220615070205604](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220615070205604.png)

和banker的区别也就是没多少区别，这个事后吧



### Banker's Algorithm： Safety algorithm

![image-20220615070351158](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220615070351158.png)

granted的要扣掉



## L13 I/0

### I/O Management IO管理

目前已经学了一些CPU management和memory management了， I/O管理的挑战有这些：

1. 各式各样的设备，每个都不同
2. 不可靠的设备
3. 不可预测和速度缓慢的设备

### 两个让IO更加高效的方式：

**基本的I/O: Polling**

要写一字节的数据进设备，OS就等设备准备好接收状态寄存器读来的命令，OS发送这些数据到数据寄存器，OS写一个命令到民领寄存器，OS等设备完成：再次轮询到循环中看是否完成。

**1！有效的I/O: Interrupts**

这次不是反复轮询了，OS可以发出请求，把调用中的进程打晕睡觉，然后上下文切换到下一个任务。当设备完成操作，可以提出一个硬件中断，让CPU在预定的中断处理程序调到OS。

在快设备上polling更好，慢设备上interrupt 更好。不知道设备或不稳定酒混合双打。

#### 2！DMA

用来防止程序IO大量数据移动



### 两个控制设备的方法

1. Explicty I/O instructions
2. Memory-mapped I/O

### Notion of device drivers

OS封装底层细节，使得设备中立方式下构建OS其余部分变得容易

## L14 Storage仓库

### Storage Devices

- Magnetic Disks:不容易损坏的存储仓库，大容量低成本，随机访问性能差，顺序访问更好
- Flash memory:不容易损坏的存储残酷，容量中等，读性能好，写性能差

![image-20220615080249614](C:\Users\Tung\AppData\Roaming\Typora\typora-user-images\image-20220615080249614.png)

### Disk Scheduling 磁盘调度

#### FIFO

先进先出，很公平。

#### Shortest Seek Time First (SSTF)

有最短的搜寻时间会被选中，和SJF很像，容易饿。比较常用

#### Scan

又叫电梯算法，disk arm从磁盘一段开始向另一段移动，服务请求直到到达另一端。如果请求密度一致，那么磁盘另一端密度最大等待时间最长

#### C-SCAN

提供更加均匀的等待时间，头依然是磁盘一段移动到另一端一边服务，到达另一端后立刻返回磁盘的开始

#### LOOK, C-LOOK

比较像上面的变体。arm只会向最后一个请求走，然后立刻逆转方向。

**需要知道这几种方法的行走路径，要知道给定一列数字后选择某种算法会做出什么样的选择**

#### 瑞平一下以上几种

 • SSTF is common and has a natural appeal

 • SCAN and C-SCAN perform better for systems that place a heavy load on the disk with less starvation

 • Either SSTF or LOOK is a reasonable choice for the default algorithm

 • Performance depends on the number and types of requests 

• The disk-scheduling algorithm should be written as a separate module of the operating system, allowing it to be replaced with a different algorithm if necessary



## L15 File System

当时没有仔细复习，结果考了一堆。
