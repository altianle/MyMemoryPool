# 高并发内存池

该内存池结构上分为三层。

* 第一层是Thread Cache，线程缓存是每个线程独有的，在这里设计的是用于小于64k的内存分配，线程在这里申请不需要加锁，每一个线程都有自己独立的cache。
* 第二层是Central Cache，在这里是所有线程共享的，它起着承上启下的作用，Thread Cache按需要从Central Cache中获取对象，起着平衡多个线程按需调度的作用，既可以将内存对象分配给Thread Cache来的每个线程，又可以将线程归还回来的内存进行管理。Central Cache是存在竞争的，在这里取内存对象的时候需要加锁。
* 第三层是Page Cache，存储的是以页为单位存储及分配，Central Cache没有内存对象(Span)时，从Page cache分配出一定数量的Page，并切割成定长大小的小块内存，分配给Central Cache。Page Cache会回收Central Cache满足条件的Span(使用计数为0)对象，并且合并相邻的页，组成更大的页，缓解内存碎片的问题。

