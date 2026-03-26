OS一般指操作系统。 操作系统（Operating System，缩写：OS）是一组主管并控制计算机操作、运用和运行硬件、软件资源和提供公共服务来组织用户交互的相互关联的系统软件程序。

![pic](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/systmes/os.png)

## OS的功能：

1. 管理应用程序生命周期（安装、运行、调度、卸载）
2. 提供硬件抽象与服务接口（文件I/O、多媒体处理、网络通信）
3. 资源分配（分配cpu、分配内存、管理外设）

对于OS而言，有两个层面的功能接口：

- 面向应用软件的接口——shell
- 面向操作系统内部管理硬件资源的接口——kernel

## Kernel的组成

- CPU调度器 (CPU Scheduler)
- 物理内存管理 (Physical Memory Management)
- 虚拟内存管理 (Virtual Memory Management)
- 文件系统管理 (File System Management)
- 中断处理与设备驱动 (Interrupt Handling & Device Drivers)
- 网络协议栈 (Network Stack)
- 系统调用接口 (System Call Interface)

Kernel的特征：

- 并发
- 共享
- 异步
- 虚拟
