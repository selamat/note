##并发的挑战

### 1.上线文档切换的代价

* 上下文切换

   CPU通过时间片分配算法来循环执行任务，以便当前执行一个时间片后会切换到下一个任务。但是，在切换前会保存上一个任务的状态，以便下次切换回这个任务时，可以再加载这个任务的状态。所以任务从保存到加载的过程就是一次上下文切换。
    这就像我们同时读两本书，当我们在读一本英文的技术书时，发现某个单词不认识，于是便打开中英文字典，但是在放下英文技术书之前，大脑必须先记住这本书读到了多少页的第多少行，等查完单词之后，能够继续读这本书。这样的切换是影响读书效率的，同样上下文切换也会影响多线程的执行速度。

    总结：所谓上下文切换是指保存当前时间片执行的状态和加载下一个时间片执行的状态。

### 如何减少上下文切换

    减少上下文切换的方法有无锁并发编程、CAS算法、使用最少线程和使用协程。

    * 无锁并发编程。多线程竞争锁时，会引起上下文切换，所以多线程处理数据时，可以用一些办法来避免使用锁，如将数据的ID按照Hash算法取模分段，不同的线程处理不同段的数据。
    * CAS算法。Java的Atomic包使用CAS算法来更新数据，而不需要加锁。
    * 使用最少线程。避免创建不需要的线程，比如任务很少，但是创建了很多线程来处理，这样会造成大量线程都处于等待状态。
    * 协程。在单线程里实现多任务的调度，并在单线程里维持多个任务的切换。