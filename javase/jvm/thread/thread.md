# 线程

线程是比进程跟轻量级的调度执行单位，主流操作系统提供线程实现，

## 实现
- 内核线程实现；
    在支持多线程内核中，采用用户空间轻量级进程和内核空间线程1：1的映射关系实现；
    - 优点：实现简单，调度灵活；
    - 缺点：线程的创建、销毁及同步，需要系统调用，用户和内核空间切换，消耗性能和内核资源；
    
- 用户线程实现；
    用户线程的建立、同步、销毁和调度完全在用户态中实现，无需内核，实现方式一对多，1：N。
    - 优点：性能好；
    - 缺点：实现复杂。
    
- 用户线程加轻量级进程混合实现；
    使用内核线程与用户线程结合，用户线程创建、切换、析构等操作廉价，支持大规模用户线程并发。通过操作系统提供的轻量级进程连接用户线程与内核线程，用户线程的系统调用通过轻量级进程实现，例如，线程调度功能及处理器映射（访问），N:M。
    

### Java线程实现




## Java线程调度

