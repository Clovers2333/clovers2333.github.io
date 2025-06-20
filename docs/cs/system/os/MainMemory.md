# Main Memory

> 基本转载自[咸鱼暄的代码空间](https://xuan-insr.github.io/%E6%A0%B8%E5%BF%83%E7%9F%A5%E8%AF%86/os/IV_memory_management/9_main_memory/).

## Address Binding

源程序中的地址通常是用符号表示（symbolic, 例如各种变量、函数名；汇编中的 label 等）；编译器会将其绑定到 relocatable addresses，即相对于某一个段/模块等的偏移，例如 sp - 8, ds:[0]；链接器或加载器（linker / loader）会将 relocatable addresses 绑定到 absolute addresses。当然，如果编译器在编译时就知道程序所处的内存地址，则会生成 absolute code。

<div style="text-align: center;">
    <img src="../assets/mainmemory1.png" width=200 />
</div>


## Partitioning Strategies

我们需要把多个进程同时放在内存中，并且支持其彼此之间的快速切换。最简单的内存分配方法之一，就是将内存分成许多的 partition，每个 partition 包含一个进程。其要求有：

- Protection: 保证进程之间不会互相闯入对方的存储。
- Fast execution: 不能由于 protection 降低访问内存的效率。
- Fast context switch: 每当进行 context switch 时，可以比较快地找到并访问当前进程的内存。

### Fixed-Size Partitioning

我们可以考虑固定 partition 的大小（除了 OS 使用的内存）。这种方式非常容易实现，我们只需要记录每个 partition 是否被占用即可。但显然，这种方式会带来很大的内存浪费。

<div style="text-align: center;">
    <img src="../assets/mainmemory2.png" width=200 />
</div>

如图 Process 1 使用了 Partition 1，但其所需空间小于 Partition 1 的大小，因此会导致红色部分的内存没有被使用且不能被别的进程使用（因为每个 partition 只能被一个进程使用）。由于这是 partition 内部的不可用内存，我们称之为 **Internal Fragmentation**。

### Variable-Size Partitioning

我们也可以考虑不固定 partition 的大小。在这种方案中，操作系统维护一个表，记录可用和已用的内存。最开始时整个内存就是一大块可用的内存块（将可用内存块称为 hole）；经过一段时间的运行后，内存可能会包含一系列不同大小的孔。下图是一个示例。

<div style="text-align: center;">
    <img src="../assets/mainmemory3.png" width=500 />
</div>

很可能在进行一段时间的运行后，空闲内存空间被分为大量的 hole，它们总体加起来可以满足进程要求，但它们并不连续，每一个小的 hole 都不可以被利用。我们称这种问题为 **External Fragmentation**，因为这些不可用内存是分布在 partition 之外的。

### Dynamic Storage-Allocation Problem

根据一组 hole 来分配大小为 n 的请求，称为 dynamic storage-allocation problem。这个问题最常用的解决方法包括：

- first-fit: 分配首个足够大的 hole。这种方法会使得分配集中在低地址区，并在此处产生大量的碎片，在每次尝试分配的时候都会遍历到，增大查找的开销。
- best-fit: 分配最小的足够大的 hole。除非空闲列表按大小排序，否则这种方法需要对整个列表进行遍历。这种方法同样会留下许多碎片。
- worst-fit: 分配最大的 hole。同样，除非列表有序，否则我们需要遍历整个列表。这种方法的好处是每次分配后通常不会使剩下的空闲块太小，这在中小进程较多的情况下性能较好，并且产生碎片的几率更小。

### Protection

我们需要保证一个进程能且仅能访问自己空间中的地址。我们可以通过一套 base 和 limit 寄存器来确定一个程序的空间：

- base 寄存器：存储进程的基地址。
- limit 寄存器：存储进程的地址范围。

每当 context switch 到一个新的进程时，CPU 会 load 这两个寄存器的值。每当 user mode 想要进行一次内存访问时，CPU 都要检查其是否试图访问非法地址；如果是，则会引发一个 trap 并被当做致命错误处理（通常会 terminate 掉进程）。

## Segmentation

虽然我们程序中的主函数、数组、符号表、子函数等等内部需要有一定的顺序，但是这些模块之间的先后顺序是无关紧要的。

一个程序是由一组 segment （段）构成的，每个 segment 都有其名称和长度。我们只要知道 segment 在物理内存中的基地址 (base) 和段内偏移地址 (offset) 就可以对应到物理地址中了。对于每一个 segment，我们给其一个编号。即，我们通过二元有序组 (segment-number, offset) 表示了一个地址。这种表示称为 logical address（逻辑地址）或 virtual address（虚拟地址）。

### Logical Address & MMU

要将逻辑地址映射到物理地址，首先我们需要找到段的基地址。我们有一个 segment table，其中每个条目以 segment-number 索引，存储其 段基地址 segment-base 和 段界限 segment-limit（可能还包含权限位）。因此逻辑地址的映射方式如下图：

<div style="text-align: center;">
    <img src="../assets/mainmemory4.png" width=500 />
</div>


这一过程是由硬件设备 MMU (Memory-Management Unit, 内存管理单元) 完成的。CPU 使用的是逻辑地址，而内存寻址使用的是物理地址，MMU 完成的是翻译（映射）和保护工作：

<div style="text-align: center;">
    <img src="../assets/mainmemory5.png" width=500 />
</div>

这里的 relocation register 即为 base register。

Segmentation 将程序分为了几块，相比于简单的 partition，分段有助于减小 external fragmentation。

## Paging

### Introduction

我们将物理内存和逻辑内存划分为等大小的块，并建立映射关系：

- **物理内存（Physical Memory）**：
    - 被切分成等大小的块，称为 frames（帧）
    - 大小为 2 的幂，通常为 4KB = 2^12B
    - 每个 frame 有唯一的 frame number 或 base address

- **逻辑内存（Logical Memory）**：
    - 被切分成与 frame 同样大小的块，称为 pages（页）
    - 每个 page 有唯一的 page number
    - 逻辑地址由 page number 和 page offset 组成

- **页表（Page Table）**：
    - 以 page number 为索引的表
    - 第 i 项存储 page number 为 i 的页所对应的物理 frame 的 base address
    - 建立 page 到 frame 的映射关系

- **地址转换过程**：
    - 进程执行时，其内容被加载到可用的 frames 中
    - CPU 生成逻辑地址（page number + page offset）
    - 通过页表将 page number 转换为对应的 frame 的 base address
    - 最终物理地址 = frame base address + page offset

<div style="text-align: center;">
    <img src="../assets/mainmemory6.png" width=500 />
</div>

Paging 将程序分成了好几块，用 Logical Address 的连续替代了之前 Physical Address 的连续.

Paging 只在程序的最后一块可能存在 Internal Fragmentation，不会有 External Fragmentation。

### Hardware Support


