# lec2：lab0 SPOC思考题

## **提前准备**
（请在上课前完成，option）

- 完成lec2的视频学习
- git pull ucore_os_lab, os_tutorial_lab, os_course_exercises  in github repos。这样可以在本机上完成课堂练习。
- 了解代码段，数据段，执行文件，执行文件格式，堆，栈，控制流，函数调用,函数参数传递，用户态（用户模式），内核态（内核模式）等基本概念。思考一下这些基本概念在不同操作系统（如linux, ucore,etc.)与不同硬件（如 x86, riscv, v9-cpu,etc.)中是如何相互配合来体现的。
- 安装好ucore实验环境，能够编译运行ucore labs中的源码。
- 会使用linux中的shell命令:objdump，nm，file, strace，gdb等，了解这些命令的用途。
- 会编译，运行，使用v9-cpu的dis,xc, xem命令（包括启动参数），阅读v9-cpu中的v9\-computer.md文档，了解汇编指令的类型和含义等，了解v9-cpu的细节。
- 了解基于v9-cpu的执行文件的格式和内容，以及它是如何加载到v9-cpu的内存中的。
- 在piazza上就学习中不理解问题进行提问。

---

## 思考题

- 你理解的对于类似ucore这样需要进程/虚存/文件系统的操作系统，在硬件设计上至少需要有哪些直接的支持？至少应该提供哪些功能的特权指令？

需要能够有直接控制cpu、内存和外部存储的支持。至少应提供中断异常服务的管理指令、TLB管理指令、分段分页管理指令等特权指令。

- 你理解的x86的实模式和保护模式有什么区别？你认为从实模式切换到保护模式需要注意那些方面？

可访问的物理内存空间大小不同，实模式较小而保护模式很大；实模式下系统程序和用户程序的内存区域没有区别对待，用户程序可能会修改系统程序的内存，而保护模式下程序不能直接访问物理内存地址，而经由操作系统管理，增加了安全性。从实模式切换到保护模式时，需要注意清空相应寄存器，清空已经进入流水线的指令，建立全局的描述符表，设置好相应的堆栈段。

- 物理地址、线性地址、逻辑地址的含义分别是什么？它们之间有什么联系？

物理地址是指处理器提交到总线上用于访问计算机系统中的内存和外设的最终地址；线性地址是指逻辑地址到物理地址变换之间的中间层，处理器通过段机制控制下的形成的地址空间；逻辑地址则是指在有地址变换功能的计算机中,访问指令给出的地址。在段机制启动、页机制没有启动的情况下，逻辑地址->段机制处理->线性地址=物理地址，在段页机制都启动的情况，逻辑地址->段机制处理->线性地址->页机制处理->物理地址。

- 你理解的risc-v的特权模式有什么区别？不同 模式在地址访问方面有何特征？

运行于机器模式下的代码可以在低层次访问机器的实现。用户模式用于传统应用程序，监管者模式则是两者之间的新型特权模式，用于现代操作系统。机器模式可以指定用户模式可用的地址段。

- 理解ucore中list_entry双向链表数据结构及其4个基本操作函数和ucore中一些基于它的代码实现（此题不用填写内容）

- 对于如下的代码段，请说明":"后面的数字是什么含义
```
 /* Gate descriptors for interrupts and traps */
 struct gatedesc {
    unsigned gd_off_15_0 : 16;        // low 16 bits of offset in segment
    unsigned gd_ss : 16;            // segment selector
    unsigned gd_args : 5;            // # args, 0 for interrupt/trap gates
    unsigned gd_rsv1 : 3;            // reserved(should be zero I guess)
    unsigned gd_type : 4;            // type(STS_{TG,IG32,TG32})
    unsigned gd_s : 1;                // must be 0 (system)
    unsigned gd_dpl : 2;            // descriptor(meaning new) privilege level
    unsigned gd_p : 1;                // Present
    unsigned gd_off_31_16 : 16;        // high bits of offset in segment
 };
```

数字表示每个成员变量的位宽

- 对于如下的代码段，

```
#define SETGATE(gate, istrap, sel, off, dpl) {            \
    (gate).gd_off_15_0 = (uint32_t)(off) & 0xffff;        \
    (gate).gd_ss = (sel);                                \
    (gate).gd_args = 0;                                    \
    (gate).gd_rsv1 = 0;                                    \
    (gate).gd_type = (istrap) ? STS_TG32 : STS_IG32;    \
    (gate).gd_s = 0;                                    \
    (gate).gd_dpl = (dpl);                                \
    (gate).gd_p = 1;                                    \
    (gate).gd_off_31_16 = (uint32_t)(off) >> 16;        \
}
```
如果在其他代码段中有如下语句，
```
unsigned intr;
intr=8;
SETGATE(intr, 1,2,3,0);
```
请问执行上述指令后， intr的值是多少？

执行上述指令后，intr = 0x20003

### 课堂实践练习

#### 练习一

1. 请在ucore中找一段你认为难度适当的AT&T格式X86汇编代码，尝试解释其含义。
   
```asm
seta20.1:
    inb $0x64, %al                                  
    testb $0x2, %al
    jnz seta20.1
    movb $0xd1, %al                                 
    outb %al, $0x64                             
```
以上代码段的含义为，读取端口0x64的数据存入寄存器al中，将该数据与0x2比较，如果不同，则返回此代码段的开头，否则将0xd1输出到0x64端口。

2. (option)请在rcore中找一段你认为难度适当的RV汇编代码，尝试解释其含义。

#### 练习二

宏定义和引用在内核代码中很常用。请枚举ucore或rcore中宏定义的用途，并举例描述其含义。

宏定义的用途有：
1.定义常量，比如
```C
#define STA_X       0x8     // Executable segment
#define STA_E       0x4     // Expand down (non-executable segments)
#define STA_C       0x4     // Conforming code segment (executable only)
```
2.定义表达式，封装类似函数的操作，但效率高于函数，比如
```C
#define SEG_ASM(type,base,lim)                                  \
    .word (((lim) >> 12) & 0xffff), ((base) & 0xffff);          \
    .byte (((base) >> 16) & 0xff), (0x90 | (type)),             \
        (0xC0 | (((lim) >> 28) & 0xf)), (((base) >> 24) & 0xff)
```
3.防止头文件被重复包含，比如
```C
#ifndef __BOOT_ASM_H__
#define __BOOT_ASM_H__
```

## 问答题

#### 在配置实验环境时，你遇到了那些问题，是如何解决的。

由于ubuntu的镜像源的问题，qemu的安装总是失败，更换了镜像源之后又出了一系列问题，最后用新的ubuntu16.04镜像重新装了一个虚拟机，然后依次安装必须的环境最后配置成功。

## 参考资料
 - [Intel格式和AT&T格式汇编区别](http://www.cnblogs.com/hdk1993/p/4820353.html)
 - [x86汇编指令集  ](http://hiyyp1234.blog.163.com/blog/static/67786373200981811422948/)
 - [PC Assembly Language, Paul A. Carter, November 2003.](https://pdos.csail.mit.edu/6.828/2016/readings/pcasm-book.pdf)
 - [*Intel 80386 Programmer's Reference Manual*, 1987](https://pdos.csail.mit.edu/6.828/2016/readings/i386/toc.htm)
 - [IA-32 Intel Architecture Software Developer's Manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)
 - [v9 cpu architecture](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)
 - [RISC-V cpu architecture](http://www.riscvbook.com/chinese/)
 - [OS相关经典论文](https://github.com/chyyuu/aos_course_info/blob/master/readinglist.md)
