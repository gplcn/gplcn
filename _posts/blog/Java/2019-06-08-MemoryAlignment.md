---
layout: post
title: 内存对齐 memory alignment
categories: [内存, 原理]
description: 什么是内存对齐？为何要内存对齐？有什么影响？
keywords: memory, alignment,内存对齐
---
# 内存对齐

memory alignment：为了提高程序的性能，数据结构（尤其是栈）应该尽可能地在自然边界上对齐。原因在于，为了访问未对齐的内存，处理器需要作多次内存访问，然而对齐的内存访问需要的访问的次数更少。

## 内存存取粒度

通常倾向于认为内存就像一个字节数组，对内存的存取也是按照单个字节块处理。
<img src="https://github.com/gplcn/gplcn.github.io/blob/develop/images/posts/memory_alignment/m_a_01.png?raw=true"  height="300" width="270">


实际上处理器并不是按字节块来存取内存的，它一般会以双字节、4字节、8字节、16字节甚至32字节为单位来存取内存，我们将上述这些存取单位称为内存存取粒度。
<img src="https://github.com/gplcn/gplcn.github.io/blob/develop/images/posts/memory_alignment/m_a_02.png?raw=true"  height="330" width="290">

## 内存存取粒度对存取的影响
内存存取粒度是如何对该任务产生影响的.这个任务很简单:先从地址0读取4个字节到寄存器,然后从地址1读取4个字节到寄存器.

### 存取粒度为1byte的情况
<div align="left">
<img src="https://github.com/gplcn/gplcn.github.io/blob/develop/images/posts/memory_alignment/m_a_07.png?raw=true"  height="250" width="290">
<img src="https://github.com/gplcn/gplcn.github.io/blob/develop/images/posts/memory_alignment/m_a_08.png?raw=true"  height="250" width="290">
</div>

* 从地址0和地址1读取4字节数据都需要相同的4次操作。
### 存取粒度为2byte的情况
<div align="left">
<img src="https://github.com/gplcn/gplcn.github.io/blob/develop/images/posts/memory_alignment/m_a_03.png?raw=true"  height="250" width="290">
<img src="https://github.com/gplcn/gplcn.github.io/blob/develop/images/posts/memory_alignment/m_a_06.png?raw=true"  height="250" width="290">
</div>

* 从地址0读取数据，双字节存取粒度的处理器读内存的次数是单字节存取粒度处理器的一半.因为每次内存存取都会产生一个固定的开销,最小化内存存取次数将提升程序的性能。
* 但从地址1读取数据时由于地址1没有和处理器的内存存取边界对齐，处理器就会做一些额外的工作。地址1这样的地址被称作非对齐地址，由于地址1是非对齐的，双字节存取粒度的处理器必须再读一次内存才能获取想要的4个字节，这减缓了操作的速度。

### 存取粒度为4byte的情况
<div align="left">
<img src="https://github.com/gplcn/gplcn.github.io/blob/develop/images/posts/memory_alignment/m_a_04.png?raw=true"  height="250" width="290">
<img src="https://github.com/gplcn/gplcn.github.io/blob/develop/images/posts/memory_alignment/m_a_05.png?raw=true"  height="250" width="290">
</div>

* 4字节存取粒度处理器可以一次性的将4个字节全部读出;而在非对齐的内存地址上,读取次数将加倍.

