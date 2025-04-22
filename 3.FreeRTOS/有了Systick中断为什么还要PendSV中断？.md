1.在SysTick中断里完成任务切换会降低操作系统的实时性：

2.把systick优先级设置为最高把PendSV设置为最低的好处：

用systick的高优先级来保证操作系统时间的正确性，而用最低优先级的PendSV来保证不会在有其他中断时进行任务切换。

**因此在配置操作系统时，要把****systick****配置为优先级最高，把****pendsv****配置为优先级最低。**

 

来自 <https://blog.csdn.net/wcc243588569/article/details/117792602> 

FreeRTOS 与中断嵌套关系

高优先级任务/中断能够抢占低优先级任务/中断，这里的【抢占】本质上就是【中断嵌套】。

 

来自 <https://zhuge.blog.csdn.net/article/details/124788428?spm=1001.2014.3001.5502> 

 

 