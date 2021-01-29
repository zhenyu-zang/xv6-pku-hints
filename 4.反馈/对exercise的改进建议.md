（这部分内容比较主观，不一定适用于所有人，也不一定就能起到很好的效果，只是提供一个可参考的建议）
操作系统的实现必然是繁琐的，需要处理诸多细节的。但如果只是按照hints一个个将细节处理下去，那么在写完后大概率会出现一些奇奇怪怪的细节错误，然后花上比实现时间多上数倍的时间去痛苦地调试。因此，我们既要把握住底层的细节，又要把握住顶层的原理。

Lab5（xv6 lazy page allocation）的3个exercises的设计就为我们给出了一个很好的实践方法：

​	Exercise1（Eliminate allocation from sbrk()）只需要删去sys_sbrk里分配内存的两行代码即可。看似简单，但却确定了目标。

​	Exercise2（Lazy allocation）则需要我们实现Lazy alloction函数主体。这正是我们应该先去把握的整体，需要先实现的主体。

​	Exercise3（Lazytests and Usertests）则需要我们去完善细节。这又是让我们在实现主体之后，再去完善细节。

像这样，先确定目标；再把握整体，实现主体；最后深入细节，完善细节，正是一种绝佳的在实践中解决问题的方法。
尤其是在面对Lab6（Copy-on-Write Fork for xv6）和Lab10（mmap）这两个只有一个exercise的Lab时，可以借鉴这样的方法。

Lab6中，仿照这一方法可以分为三步：

1. 声明标志位PTE_C，在uvmcopy函数中删去内存分配和复制，改为取消PTE_W标记，加入PTE_C标记。

2.  实现copyonwrite函数处理Copy-on-Write下的内存分配和复制，然后在usertrap中遇到page fault和执行copyout时调用它。
3.  遵照hints完善其他细节，如声明一个数组来记录每个页面被引用的次数，在页面被引用次数为0时将其释放等。

Lab10中也可以仿照这一方法，不过Lab10中细节更多，原来的第二步实际上要做三件事情，因此可以分为以下五步：

1. 声明vma结构体，并将其加入proc结构体中。

2.  实现sys_mmap函数主体。
3.  进行page fault的处理。
4.  实现sys_munmap函数主体。
5.  遵照hints完善其他细节。
