# [NSOperation 和 NSOperationQueue](https://github.com/yytmzys/blog/issues/34)

NSOperation 和 NSOperationQueue
-----
*   ###### NSOperationQueue的优点

*   ###### NSOperation和NSOperationQueue

*   ###### NSThread+runloop实现常驻线程

*   ###### 自旋锁与互斥锁

* * *

#### 一、NSOperationQueue的优点

NSOperation、NSOperationQueue 是苹果提供给我们的一套多线程解决方案。实际上 NSOperation、NSOperationQueue 是基于 GCD 更高一层的封装，完全面向对象。但是比 GCD 更简单易用、代码可读性也更高。

*   ###### 1、可以添加任务依赖，方便控制执行顺序

*   ###### 2、可以设定操作执行的优先级

*   ###### 3、任务执行状态控制:isReady,isExecuting,isFinished,isCancelled

如果只是重写NSOperation的main方法，由底层控制变更任务执行及完成状态，以及任务退出
如果重写了NSOperation的start方法，自行控制任务状态
系统通过KVO的方式移除isFinished==YES的NSOperation

*   ###### 4、可以设置最大并发量

* * *

#### 二、NSOperation和NSOperationQueue

*   ###### 操作（Operation）：

执行操作的意思，换句话说就是你在线程中执行的那段代码。
在 GCD 中是放在 block 中的。在 NSOperation 中，使用 NSOperation 子类 NSInvocationOperation、NSBlockOperation，或者自定义子类来封装操作。

*   ###### 操作队列（Operation Queues）：

这里的队列指操作队列，即用来存放操作的队列。不同于 GCD 中的调度队列 FIFO（先进先出）的原则。NSOperationQueue 对于添加到队列中的操作，首先进入准备就绪的状态（就绪状态取决于操作之间的依赖关系），然后进入就绪状态的操作的开始执行顺序（非结束执行顺序）由操作之间相对的优先级决定（优先级是操作对象自身的属性）。
操作队列通过设置最大并发操作数（maxConcurrentOperationCount）来控制并发、串行。
NSOperationQueue 为我们提供了两种不同类型的队列：主队列和自定义队列。主队列运行在主线程之上，而自定义队列在后台执行。

###### [iOS 多线程:『*NSOperation*、*NSOperation*Queue』详尽总结 - 简书](https://www.jianshu.com/p/4b1d77054b35)

#### 三、NSThread+runloop实现常驻线程

NSThread在实际开发中比较常用到的场景就是去实现常驻线程。

*   由于每次开辟子线程都会消耗cpu，在需要频繁使用子线程的情况下，频繁开辟子线程会消耗大量的cpu，而且创建线程都是任务执行完成之后也就释放了，不能再次利用，那么如何创建一个线程可以让它可以再次工作呢？也就是创建一个常驻线程。

首先常驻线程既然是常驻，那么我们可以用GCD实现一个单例来保存NSThread

```Swift
+ (NSThread *)shareThread {

    static NSThread *shareThread = nil;

    static dispatch_once_t oncePredicate;

    dispatch_once(&oncePredicate, ^{

        shareThread = [[NSThread alloc] initWithTarget:self selector:@selector(threadTest) object:nil];

        [shareThread setName:@"threadTest"];

        [shareThread start];
    });

    return shareThread;
}

```

这样创建的thread就不会销毁了吗？

```Swift
[self performSelector:@selector(test) onThread:[ViewController shareThread] withObject:nil waitUntilDone:NO];

- (void)test
{
    NSLog(@"test:%@", [NSThread currentThread]);
}

```

并没有打印，说明test方法没有被调用。
那么可以用runloop来让线程常驻

```Swift
+ (NSThread *)shareThread {

    static NSThread *shareThread = nil;

    static dispatch_once_t oncePredicate;

    dispatch_once(&oncePredicate, ^{

        shareThread = [[NSThread alloc] initWithTarget:self selector:@selector(threadTest2) object:nil];

        [shareThread setName:@"threadTest"];

        [shareThread start];
    });

    return shareThread;
}

+ (void)threadTest
{
    @autoreleasepool {

        NSRunLoop *runLoop = [NSRunLoop currentRunLoop];

        [runLoop addPort:[NSMachPort port] forMode:NSDefaultRunLoopMode];

        [runLoop run];
    }
}

```

这时候再去调用performSelector就有打印了。

#### 四、自旋锁与互斥锁

![image](//upload-images.jianshu.io/upload_images/1782258-8ca7c2b6e5fe5352.png?imageMogr2/auto-orient/strip|imageView2/2/w/1060/format/webp)

###### 自旋锁：

是一种用于保护多线程共享资源的锁，与一般互斥锁（mutex）不同之处在于当自旋锁尝试获取锁时以忙等待（busy waiting）的形式不断地循环检查锁是否可用。当上一个线程的任务没有执行完毕的时候（被锁住），那么下一个线程会一直等待（不会睡眠），当上一个线程的任务执行完毕，下一个线程会立即执行。
在多CPU的环境中，对持有锁较短的程序来说，使用自旋锁代替一般的互斥锁往往能够提高程序的性能。

###### 互斥锁：

当上一个线程的任务没有执行完毕的时候（被锁住），那么下一个线程会进入睡眠状态等待任务执行完毕，当上一个线程的任务执行完毕，下一个线程会自动唤醒然后执行任务。

###### 总结：

自旋锁会忙等: 所谓忙等，即在访问被锁资源时，调用者线程不会休眠，而是不停循环在那里，直到被锁资源释放锁。
　　互斥锁会休眠: 所谓休眠，即在访问被锁资源时，调用者线程会休眠，此时cpu可以调度其他线程工作。直到被锁资源释放锁。此时会唤醒休眠线程。

###### 优缺点：

自旋锁的优点在于，因为自旋锁不会引起调用者睡眠，所以不会进行线程调度，CPU时间片轮转等耗时操作。所有如果能在很短的时间内获得锁，自旋锁的效率远高于互斥锁。
　　缺点在于，自旋锁一直占用CPU，他在未获得锁的情况下，一直运行－－自旋，所以占用着CPU，如果不能在很短的时 间内获得锁，这无疑会使CPU效率降低。自旋锁不能实现递归调用。

###### 自旋锁：atomic、OSSpinLock、dispatch_semaphore_t

###### 互斥锁：pthread_mutex、@ synchronized、NSLock、NSConditionLock 、NSCondition、NSRecursiveLock

###### [深入理解 iOS 开发中的锁](https://links.jianshu.com/go?to=https%3A%2F%2Fbestswifter.com%2Fios-lock%2F%23osspinlock)

链接：https://www.jianshu.com/p/b809d503b6dd
