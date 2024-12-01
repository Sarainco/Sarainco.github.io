##### 1.什么是系统调用

系统调用由操作系统提供，由应用程序调用

* 如何把一段字符串输出到屏幕-------系统调用
  user progress -----> c library -----> kernel
    write() -------->   kibc write()---->sys_write()

```c
#include <stdio.h>
#include <stdlib.h>

void main() {
    printf("hello world\n");
}

sunao@sunao-virtual-machine:~/code/linux_net$ strace ./printf 
execve("./printf", ["./printf"], [/* 67 vars */]) = 0
brk(NULL)                               = 0xbed000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=95299, ...}) = 0
mmap(NULL, 95299, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fad1a9b3000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0`\t\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1868984, ...}) = 0
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fad1a9b2000
mmap(NULL, 3971488, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fad1a3dc000
mprotect(0x7fad1a59c000, 2097152, PROT_NONE) = 0
mmap(0x7fad1a79c000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1c0000) = 0x7fad1a79c000
mmap(0x7fad1a7a2000, 14752, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fad1a7a2000
close(3)                                = 0
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fad1a9b1000
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fad1a9b0000
arch_prctl(ARCH_SET_FS, 0x7fad1a9b1700) = 0
mprotect(0x7fad1a79c000, 16384, PROT_READ) = 0
mprotect(0x600000, 4096, PROT_READ)     = 0
mprotect(0x7fad1a9cb000, 4096, PROT_READ) = 0
munmap(0x7fad1a9b3000, 95299)           = 0
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 20), ...}) = 0
brk(NULL)                               = 0xbed000
brk(0xc0e000)                           = 0xc0e000
write(1, "hello world\n", 12hello world
)           = 12
exit_group(12)                          = ?
+++ exited with 12 +++
```

* **磁盘**：
  - 磁盘是计算机的长期存储设备，负责存储操作系统、应用程序及用户数据。磁盘上的数据不会因为断电而丢失。在题目中，`hello_world` 程序存储在磁盘上。

2. **DDR（动态随机存取存储器）**：
   - DDR 是计算机的主要内存，负责临时存储正在运行的程序和数据。相比磁盘，DDR 具有更快的读取和写入速度，但断电后数据会丢失。在运行程序时，程序从磁盘加载到 DDR 中执行。

3. **CACHE（高速缓存）**：
   - CACHE 是位于 CPU 和 DDR 之间的高速存储器，用于缓存经常访问的数据和指令，以减少 CPU 直接访问内存的延迟。CACHE 的访问速度远高于 DDR，但容量较小。

4. **CPU（中央处理器）**：
   - CPU 是计算机的核心处理单元，负责执行程序中的指令并进行各种计算操作。CPU 通过指令控制其他硬件组件，完成程序运行。

5. **加载程序到内存**：
   - 当用户或操作系统请求运行 `hello_world` 程序时，磁盘控制器会从磁盘中读取该程序的二进制文件，并通过总线将其传输到 DDR（内存）中。

6. **缓存数据到 CACHE**：
   - CPU 开始执行 `hello_world` 程序时，会首先从 DDR 读取程序指令。如果程序中存在经常访问的指令或数据，CPU 会将这些内容缓存到 CACHE 中以提高访问速度。

7. **执行程序**：
   - CPU 从 CACHE（或 DDR）中读取 `hello_world` 程序的指令并执行。程序中的 `printf("hello world!");` 指令会让 CPU 通过总线控制显示器（图中的显示器）将字符串 "hello world!" 显示出来。

8. **完成执行**：
   - 当 `hello_world` 程序执行完毕后，CPU 会通知操作系统清理相关资源，如释放占用的内存（DDR），以便其他程序使用。

##### 2.Linux系统调用的方式

* C库提供的包装函数，比如：socket/listen/accept/read/write等
* long syscall(long number, ...);

##### 3.如何给linux内核添加一个自己的系统调用

###### 3.1 修改系统调用表

###### 3.2 添加系统调用函数的声明

###### 3.3 实现系统调用函数

###### 3.4 应用程序使用系统调用