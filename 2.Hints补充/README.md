Lab1: Xv6 and Unix utilities
Exercise3: pingpong
	应建立两个单向通信的管道实现相互通信，而不是在一个管道上进行双向通信。

Lab3: page tables
Exercise2: A kernel page table per process
	除去hints中的工作之外，你可能还需要修改vm.c中的kvmpa函数中的内容。 

除去以上两个特例，还有大多数Lab通用的3个hints：
1.  若自己实现了某个新的函数，且需要跨文件调用，则必须将函数原型添加到kernel\defs.h中。 
2.  若完成1之后跨文件调用仍然失败，则请检查是否需要额外引用一些头文件或者extern变量（借鉴别的.c文件的写法即可）。 
3.  如需要添加新的系统调用，请参照Lab2（system calls）中Exercise1（System call tracing）中的hints完成相关流程。
这3点最早会在Lab2中遇到，并且在后续的Lab中会经常用到。
