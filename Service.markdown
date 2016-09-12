（1）Thread是程序运行的最小单元，它是分配CPU的基本单位，可以用来Thread 来执行一些异步的操作。

（2） Service 是android的一种机制，当它运行的时候如果是Local Service， 那么对应的 Service 是运行在主线程main 线程上的，如：onCreate，onStart 这些函数在被系统调用的时候都是在主线程的main 线程上运行的，如果是独立的Romote Service ， 那么对应的Service 则是运行在独立的进程的main 线程上，因此不要把Service 理解为线程，它跟线程半点毛线的关系都没有。


其实这跟android的系统机制有关，我们先拿Thread 来说，thread 的运行是独立于Activity 的，也就是说当一个Activity 被finish 后， 如果你没有主动停止Thread 或者Thread 里的方法没有执行完毕的话，thread 也会一直执行，因此这里会出现一个问题：当Activity 被finish 之后，你不再持有该Thread的引用，另一方面：你没有办法在不同的Activity 中对同一的Thread 进行控制。




1). 被启动的服务的生命周期：如果一个Service被某个Activity 调用 Context.startService 方法启动，那么不管是否有Activity使用bindService绑定或unbindService解除绑定到该Service，该Service都在后台运行。如果一个Service被startService 方法多次启动，那么onCreate方法只会调用一次，onStart将会被调用多次（对应调用startService的次数），并且系统只会创建Service的一个实例（因此你应该知道只需要一次stopService调用）。该Service将会一直在后台运行，而不管对应程序的Activity是否在运行，直到被调用stopService，或自身的stopSelf方法。当然如果系统资源不足，android系统也可能结束服务。

 

2). 被绑定的服务的生命周期：如果一个Service被某个Activity 调用 Context.bindService 方法绑定启动，不管调用 bindService 调用几次，onCreate方法都只会调用一次，同时onStart方法始终不会被调用。当连接建立之后，Service将会一直运行，除非调用Context.unbindService 断开连接或者之前调用bindService 的 Context 不存在了（如Activity被finish的时候），系统将会自动停止Service，对应onDestroy将被调用。

 

3). 被启动又被绑定的服务的生命周期：如果一个Service又被启动又被绑定，则该Service将会一直在后台运行。并且不管如何调用，onCreate始终只会调用一次，对应startService调用多少次，Service的onStart便会调用多少次。调用unbindService将不会停止Service，而必须调用 stopService 或 Service的 stopSelf 来停止服务。

 

4). 当服务被停止时清除服务：当一个Service被终止（1、调用stopService；2、调用stopSelf；3、不再有绑定的连接（没有被启动））时，onDestroy方法将会被调用，在这里你应当做一些清除工作，如停止在Service中创建并运行的线程。



创建前台服务


在什么情况下使用StartService 或者 bindService 或者同时使用者两个。











