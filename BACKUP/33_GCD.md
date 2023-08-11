# [GCD](https://github.com/yytmzys/blog/issues/33)

GCD
-----
*   ###### GCD---同步/异步 ，串行/并发

*   ###### 死锁

*   ###### GCD任务执行顺序

*   ###### dispatch_barrier_async

*   ###### dispatch_group_async

*   ###### Dispatch Semaphore

*   ###### 延时函数(dispatch_after)

*   ###### 使用dispatch_once实现单例

* * *

#### 一、GCD---队列

iOS中，有GCD、NSOperation、NSThread等几种多线程技术方案。

而GCD共有三种队列类型：
main queue：通过dispatch_get_main_queue()获得，这是一个与主线程相关的串行队列。

global queue：全局队列是并发队列，由整个进程共享。存在着高、中、低三种优先级的全局队列。调用dispath_get_global_queue并传入优先级来访问队列。

自定义队列：通过函数dispatch_queue_create创建的队列。

#### 二、 死锁

死锁就是队列引起的循环等待

###### 1、一个比较常见的死锁例子:主队列同步

```Swift
- (void)viewDidLoad {
    [super viewDidLoad];

    dispatch_sync(dispatch_get_main_queue(), ^{

        NSLog(@"deallock");
    });
    // Do any additional setup after loading the view, typically from a nib.
}

```

在主线程中运用主队列同步，也就是把任务放到了主线程的队列中。
同步对于任务是立刻执行的，那么当把任务放进主队列时，它就会立马执行,只有执行完这个任务，viewDidLoad才会继续向下执行。
而viewDidLoad和任务都是在主队列上的，由于队列的先进先出原则，任务又需等待viewDidLoad执行完毕后才能继续执行，viewDidLoad和这个任务就形成了相互循环等待，就造成了死锁。
想避免这种死锁，可以将同步改成异步dispatch_async,或者将dispatch_get_main_queue换成其他串行或并行队列，都可以解决。

###### 2、同样，下边的代码也会造成死锁：

```Swift
dispatch_queue_t serialQueue = dispatch_queue_create("test", DISPATCH_QUEUE_SERIAL);

dispatch_async(serialQueue, ^{

        dispatch_sync(serialQueue, ^{

            NSLog(@"deadlock");
        });
    });

```

外面的函数无论是同步还是异步都会造成死锁。
这是因为里面的任务和外面的任务都在同一个serialQueue队列内，又是同步，这就和上边主队列同步的例子一样造成了死锁
解决方法也和上边一样，将里面的同步改成异步dispatch_async,或者将serialQueue换成其他串行或并行队列，都可以解决

```Swift
    dispatch_queue_t serialQueue = dispatch_queue_create("test", DISPATCH_QUEUE_SERIAL);

    dispatch_queue_t serialQueue2 = dispatch_queue_create("test", DISPATCH_QUEUE_SERIAL);

    dispatch_async(serialQueue, ^{

        dispatch_sync(serialQueue2, ^{

            NSLog(@"deadlock");
        });
    });

```

这样是不会死锁的,并且serialQueue和serialQueue2是在同一个线程中的。

#### 三、GCD任务执行顺序

###### 1、串行队列先异步后同步

```Swift
    dispatch_queue_t serialQueue = dispatch_queue_create("test", DISPATCH_QUEUE_SERIAL);

    NSLog(@"1");

    dispatch_async(serialQueue, ^{

         NSLog(@"2");
    });

    NSLog(@"3");

    dispatch_sync(serialQueue, ^{

        NSLog(@"4");
    });

    NSLog(@"5");

```

打印顺序是13245
原因是:
首先先打印1
接下来将任务2其添加至串行队列上，由于任务2是异步，不会阻塞线程，继续向下执行，打印3
然后是任务4,将任务4添加至串行队列上，因为任务4和任务2在同一串行队列，根据队列先进先出原则，任务4必须等任务2执行后才能执行，又因为任务4是同步任务，会阻塞线程，只有执行完任务4才能继续向下执行打印5
所以最终顺序就是13245。
这里的任务4在主线程中执行，而任务2在子线程中执行。
如果任务4是添加到另一个串行队列或者并行队列，则任务2和任务4无序执行(可以添加多个任务看效果)

###### 2、performSelector

```Swift
dispatch_async(dispatch_get_global_queue(0, 0), ^{

        [self performSelector:@selector(test:) withObject:nil afterDelay:0];
    });

```

这里的test方法是不会去执行的，原因在于

```Swift
- (void)performSelector:(SEL)aSelector withObject:(nullable id)anArgument afterDelay:(NSTimeInterval)delay;

```

这个方法要创建提交任务到runloop上的，而gcd底层创建的线程是默认没有开启对应runloop的，所有这个方法就会失效。
而如果将dispatch_get_global_queue改成主队列，由于主队列所在的主线程是默认开启了runloop的，就会去执行(将dispatch_async改成同步，因为同步是在当前线程执行，那么如果当前线程是主线程，test方法也是会去执行的)。

#### 四、dispatch_barrier_async

###### 1、问：怎么用GCD实现多读单写？

多读单写的意思就是：可以多个读者同时读取数据，而在读的时候，不能去写入数据。并且，在写的过程中，不能有其他写者去写。即读者之间是并发的，写者与读者或其他写者是互斥的。

![image](//upload-images.jianshu.io/upload_images/1782258-0a403ecc774ccc00.png?imageMogr2/auto-orient/strip|imageView2/2/w/1198/format/webp)

这里的写处理就是通过栅栏的形式去写。
就可以用**dispatch_barrier_sync(栅栏函数)**去实现

###### 2、dispatch_barrier_sync的用法：

```Swift
dispatch_queue_t concurrentQueue = dispatch_queue_create("test", DISPATCH_QUEUE_CONCURRENT);

    for (NSInteger i = 0; i < 10; i++) {

        dispatch_sync(concurrentQueue, ^{

            NSLog(@"%zd",i);
        });
    }

    dispatch_barrier_sync(concurrentQueue, ^{

        NSLog(@"barrier");
    });

    for (NSInteger i = 10; i < 20; i++) {

        dispatch_sync(concurrentQueue, ^{

            NSLog(@"%zd",i);
        });
    }

```

这里的dispatch_barrier_sync上的队列要和需要阻塞的任务在同一队列上，否则是无效的。
从打印上看，任务0-9和任务任务10-19因为是异步并发的原因，彼此是无序的。而由于栅栏函数的存在，导致顺序必然是先执行任务0-9，再执行栅栏函数，再去执行任务10-19。

*   **dispatch_barrier_sync**: Submits a barrier block object for execution and waits until that block completes.(提交一个栅栏函数在执行中,它会等待栅栏函数执行完)
*   **dispatch_barrier_async**: Submits a barrier block for asynchronous execution and returns immediately.(提交一个栅栏函数在异步执行中,它会立马返回)
    而dispatch_barrier_sync和dispatch_barrier_async的区别也就在于会不会阻塞当前线程
    比如，上述代码如果在dispatch_barrier_async后随便加一条打印，则会先去执行该打印，再去执行任务0-9和栅栏函数；而如果是dispatch_barrier_sync，则会在任务0-9和栅栏函数后去执行这条打印。

###### 3、则可以这样设计多读单写：

```Swift
- (id)readDataForKey:(NSString *)key
{
    __block id result;

    dispatch_sync(_concurrentQueue, ^{

        result = [self valueForKey:key];
    });

    return result;
}

- (void)writeData:(id)data forKey:(NSString *)key
{
    dispatch_barrier_async(_concurrentQueue, ^{

        [self setValue:data forKey:key];
    });
}

```

#### 五、dispatch_group_async

场景：在n个耗时并发任务都完成后，再去执行接下来的任务。比如，在n个网络请求完成后去刷新UI页面。

```Swift
dispatch_queue_t concurrentQueue = dispatch_queue_create("test1", DISPATCH_QUEUE_CONCURRENT);

    dispatch_group_t group = dispatch_group_create();

    for (NSInteger i = 0; i < 10; i++) {

        dispatch_group_async(group, concurrentQueue, ^{

            sleep(1);

            NSLog(@"%zd:网络请求",i);
        });
    }

    dispatch_group_notify(group, dispatch_get_main_queue(), ^{

        NSLog(@"刷新页面");
    });

```

###### [深入理解GCD之dispatch_group](https://www.jianshu.com/p/e93fd15d93d3)

###### 六、Dispatch Semaphore

GCD 中的信号量是指 Dispatch Semaphore，是持有计数的信号。

Dispatch Semaphore 提供了三个函数

###### 1.dispatch_semaphore_create：创建一个Semaphore并初始化信号的总量

###### 2.dispatch_semaphore_signal：发送一个信号，让信号总量加1

###### 3.dispatch_semaphore_wait：可以使总信号量减1，当信号总量为0时就会一直等待（阻塞所在线程），否则就可以正常执行。

Dispatch Semaphore 在实际开发中主要用于：

*   保持线程同步，将异步执行任务转换为同步执行任务
*   保证线程安全，为线程加锁

###### 1、保持线程同步：

```Swift
dispatch_semaphore_t semaphore = dispatch_semaphore_create(0);

    __block NSInteger number = 0;

    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{

        number = 100;

        dispatch_semaphore_signal(semaphore);
    });

    dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER);

    NSLog(@"semaphore---end,number = %zd",number);

```

dispatch_semaphore_wait加锁阻塞了当前线程，dispatch_semaphore_signal解锁后当前线程继续执行

###### 2、保证线程安全，为线程加锁：

在线程安全中可以将dispatch_semaphore_wait看作加锁，而dispatch_semaphore_signal看作解锁
首先创建全局变量

```Swift
 _semaphore = dispatch_semaphore_create(1);

```

注意到这里的初始化信号量是1。

```Swift
- (void)asyncTask
{

    dispatch_semaphore_wait(_semaphore, DISPATCH_TIME_FOREVER);

    count++;

    sleep(1);

    NSLog(@"执行任务:%zd",count);

    dispatch_semaphore_signal(_semaphore);
}

```

异步并发调用asyncTask

```Swift
  for (NSInteger i = 0; i < 100; i++) {

        dispatch_async(dispatch_get_global_queue(0, 0), ^{

            [self asyncTask];
        });
    }

```

然后发现打印是从任务1顺序执行到100，没有发生两个任务同时执行的情况。

原因如下:
在子线程中并发执行asyncTask，那么第一个添加到并发队列里的，会将信号量减1，此时信号量等于0，可以执行接下来的任务。而并发队列中其他任务，由于此时信号量不等于0，必须等当前正在执行的任务执行完毕后调用dispatch_semaphore_signal将信号量加1，才可以继续执行接下来的任务，以此类推，从而达到线程加锁的目的。

#### 六、延时函数(dispatch_after)

dispatch_after能让我们添加进队列的任务延时执行，该函数并不是在指定时间后执行处理，而只是在指定时间追加处理到dispatch_queue

```Swift
//第一个参数是time，第二个参数是dispatch_queue，第三个参数是要执行的block
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{

        NSLog(@"dispatch_after");
    });

```

由于其内部使用的是dispatch_time_t管理时间，而不是NSTimer。
所以如果在子线程中调用，相比performSelector:afterDelay,不用关心runloop是否开启

#### 七、使用dispatch_once实现单例

```Swift
+ (instancetype)shareInstance {

    static dispatch_once_t onceToken;

    static id instance = nil;

    dispatch_once(&onceToken, ^{

        instance = [[self alloc] init];
    });

    return instance;
}
```