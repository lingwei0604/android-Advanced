SharedPreference在使用过程中的注意点。


commit（）和apply（）的区别？<br>
返回值：
apply（）没有返回值，而commit（）返回boolean表明修改是否提交成功。

操作效率：

apply（）是将修改数据原子提交到内存，而后异步提交到硬件磁盘。从而降低了效率。

而commit（）是同步的提交到硬件磁盘

因此，在多并发commit（）的时候，会等待正在处理的commit保存到磁盘后在操作，从而降低了效率。

而apply只是原子提交到内容，后面有调用apply的函数将会直接覆盖前面的内存数据，从一定程度上提高了效率。

在多进程中，由于进程间是不能内存共享的，每个进程操作的SharePreference 都是一个单独的实例，而且不能通过加锁来解决，这导致了多进程通过SharePreference 来共享数据时不安全的，这个问题只能通过其他的通信方式或者确保不会同时操作SharPreferences 数据的前提下使用SharePreferences 来解决。