# lec1: 操作系统概述

---

## **提前准备**

（请在上课前完成）

* 完成lec1的视频学习和提交对应的在线练习
* git pull ucore\_os\_lab, ucore\_os\_docs, os\_tutorial\_lab, os\_course\_exercises in github repos。这样可以在本机上完成课堂练习。
* 知道OS课程的入口网址，会使用在线视频平台，在线练习/实验平台，在线提问平台\(piazza\)
  * [http://os.cs.tsinghua.edu.cn/oscourse/OS2019spring](http://os.cs.tsinghua.edu.cn/oscourse/OS2019spring)


* 会使用linux shell命令，如ls, rm, mkdir, cat, less, more, gcc等，也会使用linux系统的基本操作。
* 在piazza上就学习中不理解问题进行提问。



# 思考题

## 填空题

* 当前常见的操作系统主要用_C、C++、ASM_编程语言编写。
* "Operating system"这个单词起源于_Operator_ 。
* 在计算机系统中，控制和管理_各种资源_ 、有效地组织_多道程序_运行的系统软件称作_操作系统_ 。
* 允许多用户将若干个作业提交给计算机系统集中处理的操作系统称为_批处理_操作系统
* 你了解的当前世界上使用最多的操作系统是_ Android_ 。
* 应用程序通过_系统调用_接口获得操作系统的服务。
* 现代操作系统的特征包括_并发_ ，_共享_ ，_虚拟_ ，_异步_。
* 操作系统内核的架构包括_单内核_ ，_微内核_ ，_外内核_。


## 问答题

- 请总结你认为操作系统应该具有的特征有什么？并对其特征进行简要阐述。

操作系统应该具有并发性、共享性、虚拟性、异步性四个特征。并发性指的是计算机中同一时间段内存在多个运行的程序需要OS管理和调度；共享性是指多个应用并发运行的时候，宏观上体现为同时运行，微观上则需要实现它们的互斥访问；虚拟性是指利用多道程序设计技术，让每个用户都觉得有一个计算机专门为他服务；异步性是指程序的执行走走停停不可预知，但是相同环境下的相同输入OS需要保证有相同的输出。

- 为什么现在的操作系统基本上用C语言来实现？为什么没有人用python，java来实现操作系统？

首先，C语言具有超强的可移植性，并且接近底层，容易嵌入汇编，这一点上java无法满足，而且C语言是编译性的语言，效率会远超过python这类解释性的语言。

---

## 可选练习题

---

- 请分析并理解[v9\-computer](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)以及模拟v9\-computer的em.c。理解：在v9\-computer中如何实现时钟中断的；v9 computer的CPU指令，关键变量描述有误或不全的情况；在v9\-computer中的跳转相关操作是如何实现的；在v9\-computer中如何设计相应指令，可有效实现函数调用与返回；OS程序被加载到内存的哪个位置,其堆栈是如何设置的；在v9\-computer中如何完成一次内存地址的读写的；在v9\-computer中如何实现分页机制。


- 请编写一个小程序，在v9-cpu下，能够输出字符


- 输入的字符并输出你输入的字符


- 请编写一个小程序，在v9-cpu下，能够产生各种异常/中断


- 请编写一个小程序，在v9-cpu下，能够统计并显示内存大小



- 请分析并理解[RISC-V CPU](http://www.riscvbook.com/chinese/)以及会使用模拟RISC\-V(简称RV)的qemu工具。理解：RV的特权指令，CSR寄存器和在RV中如何实现时钟中断和IO操作；OS程序如何被加载运行的；在RV中如何实现分页机制。
  - 请编写一个小程序，在RV下，能够输出字符
  - 输入的字符并输出你输入的字符
  - 请编写一个小程序，在RV下，能够产生各种异常/中断
  - 请编写一个小程序，在RV下，能够统计并显示内存大小

## 参考资料
 - [Intel格式和AT&T格式汇编区别](http://www.cnblogs.com/hdk1993/p/4820353.html)
 - [x86汇编指令集  ](http://hiyyp1234.blog.163.com/blog/static/67786373200981811422948/)
 - [PC Assembly Language, Paul A. Carter, November 2003.](https://pdos.csail.mit.edu/6.828/2016/readings/pcasm-book.pdf)
 - [*Intel 80386 Programmer's Reference Manual*, 1987](https://pdos.csail.mit.edu/6.828/2016/readings/i386/toc.htm)
 - [IA-32 Intel Architecture Software Developer's Manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)
 - [v9 cpu architecture](https://github.com/chyyuu/os_tutorial_lab/blob/master/v9_computer/docs/v9_computer.md)
 - [RISC-V cpu architecture](http://www.riscvbook.com/chinese/)
 - [OS相关经典论文](https://github.com/chyyuu/aos_course_info/blob/master/readinglist.md)
