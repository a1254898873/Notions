## NIO

### 定义

java.nio全称java non-blocking IO，是指JDK1.4 及以上版本里提供的新api（New IO） ，为所有的原始类型（boolean类型除外）提供缓存支持的数据容器，使用它可以提供非阻塞式的高伸缩性网络

JDK1.4之前使用的是BIO，也就是我们常用的IO流。

BIO的问题其实不用多说了，因为在使用BIO时，主线程会进入阻塞状态，这就非常影响程序的性能，不能充分利用机器资源。但是这样就会有人提出疑问了，那我使用多线程不就可以了吗？

但是在高并发的情况下，会创建很多线程，线程会占用内存，线程之间的切换也会浪费资源开销。

而NIO只有在连接/通道真正有读写事件发生时(事件驱动)，才会进行读写，就大大地减少了系统的开销。不必为每一个连接都创建一个线程，也不必去维护多个线程。

避免了多个线程之间的上下文切换，导致资源的浪费。



### 三大核心

| NIO的核心 | 对应的类或接口 | 应用          | 作用     |
| :-------- | :------------- | :------------ | :------- |
| 缓冲区    | Buffer         | 文件IO/网络IO | 存储数据 |
| 通道      | Channel        | 文件IO/网络IO | 运输     |
| 选择器    | Selector       | 网络IO        | 控制器   |



#### 1、缓冲区

![图片](https://raw.githubusercontent.com/a1254898873/images/master/202206161547206.png)

Buffer是一个内存块。在NIO中，所有的数据都是用Buffer处理，有读写两种模式。所以NIO和传统的IO的区别就体现在这里。传统IO是面向Stream流，NIO而是面向缓冲区(Buffer)。



主要分成两种：JVM堆内内存块Buffer、堆外内存块Buffer。

创建堆内内存块(非直接缓冲区)的方法是：

```java
//创建堆内内存块HeapByteBuffer
ByteBuffer byteBuffer1 = ByteBuffer.allocate(1024);

String msg = "java技术爱好者";
//包装一个byte[]数组获得一个Buffer，实际类型是HeapByteBuffer
ByteBuffer byteBuffer2 = ByteBuffer.wrap(msg.getBytes());
```

创建堆外内存块(直接缓冲区)的方法：

```java
//创建堆外内存块DirectByteBuffer
ByteBuffer byteBuffer3 = ByteBuffer.allocateDirect(1024);
```



`DirectByteBuffer`的使用场景：

1. java程序与本地磁盘、socket传输数据
2. 大文件对象，可以使用。不会受到堆内存大小的限制。
3. 不需要频繁创建，生命周期较长的情况，能重复使用的情况。

`HeapByteBuffer`的使用场景：

除了以上的场景外，其他情况还是建议使用`HeapByteBuffer`，没有达到一定的量级，实际上使用`DirectByteBuffer`是体现不出优势的。





#### 2、管道

![图片](https://raw.githubusercontent.com/a1254898873/images/master/202206161552113.png)

常用的Channel有这四种：

> FileChannel，读写文件中的数据。
> SocketChannel，通过TCP读写网络中的数据。
> ServerSockectChannel，监听新进来的TCP连接，像Web服务器那样。对每一个新进来的连接都会创建一个SocketChannel。
> DatagramChannel，通过UDP读写网络中的数据。

**Channel本身并不存储数据，只是负责数据的运输**。必须要和`Buffer`一起使用。





#### 3、选择器

`Selector`翻译成**选择器**，有些人也会翻译成**多路复用器**，实际上指的是同一样东西。

只有网络IO才会使用选择器，文件IO是不需要使用的。

选择器可以说是NIO的核心组件，它可以监听通道的状态，来实现异步非阻塞的IO。换句话说，也就是事件驱动。以此实现**单线程管理多个Channel**的目的。

![图片](https://raw.githubusercontent.com/a1254898873/images/master/202206161555704.png)

| API方法名       | 作用                                            |
| :-------------- | :---------------------------------------------- |
| Selector.open() | 打开一个选择器。                                |
| select()        | 选择一组键，其相应的通道已为 I/O 操作准备就绪。 |
| selectedKeys()  | 返回此选择器的已选择键集。                      |



(1) Selector的创建

通过调用Selector.open()方法创建一个Selector对象，如下：

```java
Selector selector = Selector.open();
```

(2) 注册Channel到Selector

```java
channel.configureBlocking(false);
SelectionKey key = channel.register(selector, Selectionkey.OP_READ);
```

**Channel必须是非阻塞的**。 
所以FileChannel不适用Selector，因为FileChannel不能切换为非阻塞模式，更准确的来说是因为FileChannel没有继承SelectableChannel。Socket channel可以正常使用。

(3) 从Selector中选择channel

> **Selector维护的三种类型SelectionKey集合：**

- **已注册的键的集合(Registered key set)**

  所有与选择器关联的通道所生成的键的集合称为已经注册的键的集合。并不是所有注册过的键都仍然有效。这个集合通过 **keys()** 方法返回，并且可能是空的。这个已注册的键的集合不是可以直接修改的；试图这么做的话将引发java.lang.UnsupportedOperationException。

- **已选择的键的集合(Selected key set)**

  所有与选择器关联的通道所生成的键的集合称为已经注册的键的集合。并不是所有注册过的键都仍然有效。这个集合通过 **keys()** 方法返回，并且可能是空的。这个已注册的键的集合不是可以直接修改的；试图这么做的话将引发java.lang.UnsupportedOperationException。

- **已取消的键的集合(Cancelled key set)**

  已注册的键的集合的子集，这个集合包含了 **cancel()** 方法被调用过的键(这个键已经被无效化)，但它们还没有被注销。这个集合是选择器对象的私有成员，因而无法直接访问。

> **select()方法介绍：**

在刚初始化的Selector对象中，这三个集合都是空的。 **通过Selector的select（）方法可以选择已经准备就绪的通道** （这些通道包含你感兴趣的的事件）。比如你对读就绪的通道感兴趣，那么select（）方法就会返回读事件已经就绪的那些通道。下面是Selector几个重载的select()方法：

- int select()：阻塞到至少有一个通道在你注册的事件上就绪了。
- int select(long timeout)：和select()一样，但最长阻塞时间为timeout毫秒。
- int selectNow()：非阻塞，只要有通道就绪就立刻返回。

**select()方法返回的int值表示有多少通道已经就绪,是自上次调用select()方法后有多少通道变成就绪状态。之前在select（）调用时进入就绪的通道不会在本次调用中被记入，而在前一次select（）调用进入就绪但现在已经不在处于就绪的通道也不会被记入**。例如：首次调用select()方法，如果有一个通道变成就绪状态，返回了1，若再次调用select()方法，如果另一个通道就绪了，它会再次返回1。如果对第一个就绪的channel没有做任何操作，现在就有两个就绪的通道，但在每次select()方法调用之间，只有一个通道就绪了。

**一旦调用select()方法，并且返回值不为0时，则 可以通过调用Selector的selectedKeys()方法来访问已选择键集合** 。如下： 
Set selectedKeys=selector.selectedKeys(); 
进而可以放到和某SelectionKey关联的Selector和Channel。

```java
Set selectedKeys = selector.selectedKeys();
Iterator keyIterator = selectedKeys.iterator();
while(keyIterator.hasNext()) {
    SelectionKey key = keyIterator.next();
    if(key.isAcceptable()) {
        // a connection was accepted by a ServerSocketChannel.
    } else if (key.isConnectable()) {
        // a connection was established with a remote server.
    } else if (key.isReadable()) {
        // a channel is ready for reading
    } else if (key.isWritable()) {
        // a channel is ready for writing
    }
    keyIterator.remove();
}
```





### 模板

```java
ServerSocketChannel ssc = ServerSocketChannel.open();
ssc.socket().bind(new InetSocketAddress("localhost", 8080));
ssc.configureBlocking(false);

Selector selector = Selector.open();
ssc.register(selector, SelectionKey.OP_ACCEPT);

while(true) {
    int readyNum = selector.select();
    if (readyNum == 0) {
        continue;
    }

    Set<SelectionKey> selectedKeys = selector.selectedKeys();
    Iterator<SelectionKey> it = selectedKeys.iterator();

    while(it.hasNext()) {
        SelectionKey key = it.next();

        if(key.isAcceptable()) {
            // 接受连接
        } else if (key.isReadable()) {
            // 通道可读
        } else if (key.isWritable()) {
            // 通道可写
        }

        it.remove();
    }
}
```









## Netty

### 结构体系

![image.png](https://raw.githubusercontent.com/a1254898873/images/master/202206161844839.png)

它涉及到的内容主要分为三个部分 : bootstrap, channel, eventLoop.

 `bootstrap` 主要负责服务建立与发布

 `channel` 主要负责协议建立与协议事件处理

 `eventloop` 主要负责任务执行与事件监听

在基于 TCP 的 socket 程序里面， 我们的协议主要是指一个 socket 的`建立`, `listen`, `accept`, `connect`, `read`, `write` 等。









### 线程模型

Netty的线程模型是比较重要的，理解了Netty的线程模型才能很好地使用Netty，Netty常见的线程模型有三种：

1.单线程模型

单线程模型，是指所有的 I/O 操作都在同一个 NIO 线程上面完成的，此时NIO线程职责包括：接收新建连接请求、读写操作等，在游戏开发中不会使用，也不合理，不展开。

2.Reactor多线程模型

第一种不合理，升级一下，一个接受连接的线程， 所有的 I/O 操作都在同一个 NIO 线程池上面完成，这种线程模型可以满足大部分情况，但是如果在连接的时候需要做一些验证，就会阻塞线程。性能会出问题，服务器

3.Reactor主从多线程模型

服务端用于接收客户端连接的不再是一个单独的 NIO 线程，而是一个独立的 NIO 线程池。Acceptor 接收到客户端 TCP连接请求并处理完成后（可能包含接入认证等），将新创建的 SocketChannel注 册 到 I/O 线 程 池（sub reactor 线 程 池）的某个I/O线程上， 由它负责SocketChannel 的读写和编解码工作。Acceptor 线程池仅仅用于客户端的登录、握手和安全认证，一旦链路建立成功，就将链路注册到后端 subReactor 线程池的 I/O 线程上，由 I/O 线程负责后续的 I/O 操作。

这也是在游戏开发中最常用的线程模型，需要掌握，下面这张图将核心技术都做了展示


<img src="https://raw.githubusercontent.com/a1254898873/images/master/202207081054402.png" alt="图片" style="zoom: 67%;" />





### 核心组件

#### 1、Bytebuf

网络通信最终都是通过字节流进行传输的。 ByteBuf 就是 Netty 提供的一个字节容器，其内部是一个字节数组。 当我们通过 Netty 传输数据的时候，就是通过 ByteBuf 进行的。

我们可以将 ByteBuf 看作是 Netty 对 Java NIO 提供了 ByteBuffer 字节容器的封装和抽象。

有很多小伙伴可能就要问了 ： 为什么不直接使用 Java NIO 提供的 ByteBuffer 呢？

因为 ByteBuffer 这个类使用起来过于复杂和繁琐。



#### 2、Bootstrap 和 ServerBootstrap

**`Bootstrap` 是客户端的启动引导类/辅助类**，具体使用方法如下：

```java
        EventLoopGroup group = new NioEventLoopGroup();
        try {
            //创建客户端启动引导/辅助类：Bootstrap
            Bootstrap b = new Bootstrap();
            //指定线程模型
            b.group(group).
                    ......
            // 尝试建立连接
            ChannelFuture f = b.connect(host, port).sync();
            f.channel().closeFuture().sync();
        } finally {
            // 优雅关闭相关线程组资源
            group.shutdownGracefully();
        }
```

**`ServerBootstrap` 客户端的启动引导类/辅助类**，具体使用方法如下：

```java
        // 1.bossGroup 用于接收连接，workerGroup 用于具体的处理
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        EventLoopGroup workerGroup = new NioEventLoopGroup();
        try {
            //2.创建服务端启动引导/辅助类：ServerBootstrap
            ServerBootstrap b = new ServerBootstrap();
            //3.给引导类配置两大线程组,确定了线程模型
            b.group(bossGroup, workerGroup).
                   ......
            // 6.绑定端口
            ChannelFuture f = b.bind(port).sync();
            // 等待连接关闭
            f.channel().closeFuture().sync();
        } finally {
            //7.优雅关闭相关线程组资源
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }
```

从上面的示例中，我们可以看出：

1. `Bootstrap` 通常使用 `connet()` 方法连接到远程的主机和端口，作为一个 Netty TCP 协议通信中的客户端。另外，`Bootstrap` 也可以通过 `bind()` 方法绑定本地的一个端口，作为 UDP 协议通信中的一端。
2. `ServerBootstrap`通常使用 `bind()` 方法绑定本地的端口上，然后等待客户端的连接。
3. `Bootstrap` 只需要配置一个线程组— `EventLoopGroup` ,而 `ServerBootstrap`需要配置两个线程组— `EventLoopGroup` ，一个用于接收连接，一个用于具体的 IO 处理。





#### 3、Channel

`Channel` 接口是 Netty 对网络操作抽象类。通过 `Channel` 我们可以进行 I/O 操作。

一旦客户端成功连接服务端，就会新建一个 `Channel` 同该用户端进行绑定，示例代码如下：

```java
   //  通过 Bootstrap 的 connect 方法连接到服务端
   public Channel doConnect(InetSocketAddress inetSocketAddress) {
        CompletableFuture<Channel> completableFuture = new CompletableFuture<>();
        bootstrap.connect(inetSocketAddress).addListener((ChannelFutureListener) future -> {
            if (future.isSuccess()) {
                completableFuture.complete(future.channel());
            } else {
                throw new IllegalStateException();
            }
        });
        return completableFuture.get();
    }
```

比较常用的`Channel`接口实现类是 ：

- `NioServerSocketChannel`（服务端）
- `NioSocketChannel`（客户端）

这两个 `Channel` 可以和 BIO 编程模型中的`ServerSocket`以及`Socket`两个概念对应上。





#### 4、EventLoop

EventLoop 的主要作用实际就是责监听网络事件并调用事件处理器进行相关 I/O 操作（读写）的处理。

Channel 为 Netty 网络操作(读写等操作)抽象类，EventLoop 负责处理注册到其上的Channel 的 I/O 操作，两者配合进行 I/O 操作。

EventLoopGroup 包含多个 EventLoop（每一个 EventLoop 通常内部包含一个线程），它管理着所有的 EventLoop 的生命周期。

并且，EventLoop 处理的 I/O 事件都将在它专有的 Thread 上被处理，即 Thread 和 EventLoop 属于 1 : 1 的关系，从而保证线程安全。

下图是 Netty NIO 模型对应的 EventLoop 模型。通过这个图应该可以将EventloopGroup、EventLoop、 Channel三者联系起来。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202206171249625.png)



#### 5、ChannelHandler 和 ChannelPipeline

```java
        b.group(eventLoopGroup)
                .handler(new ChannelInitializer<SocketChannel>() {
                    @Override
                    protected void initChannel(SocketChannel ch) {
                        ch.pipeline().addLast(new NettyKryoDecoder(kryoSerializer, RpcResponse.class));
                        ch.pipeline().addLast(new NettyKryoEncoder(kryoSerializer, RpcRequest.class));
                        ch.pipeline().addLast(new KryoClientHandler());
                    }
                });
```

ChannelHandler 是消息的具体处理器，主要负责处理客户端/服务端接收和发送的数据。

当 Channel 被创建时，它会被自动地分配到它专属的 ChannelPipeline。 一个Channel包含一个 ChannelPipeline。 ChannelPipeline 为 ChannelHandler 的链，一个 pipeline 上可以有多个 ChannelHandler。

我们可以在 ChannelPipeline 上通过 addLast() 方法添加一个或者多个ChannelHandler （一个数据或者事件可能会被多个 Handler 处理） 。当一个 ChannelHandler 处理完之后就将数据交给下一个 ChannelHandler 。

当 ChannelHandler 被添加到的 ChannelPipeline 它得到一个 ChannelHandlerContext，它代表一个 ChannelHandler 和 ChannelPipeline 之间的“绑定”。 ChannelPipeline 通过 ChannelHandlerContext来间接管理 ChannelHandler 。

![img](https://raw.githubusercontent.com/a1254898873/images/master/202206171306544.png)



#### 6、ChannelFuture

```java
public interface ChannelFuture extends Future<Void> {
    Channel channel();

    ChannelFuture addListener(GenericFutureListener<? extends Future<? super Void>> var1);
     ......

    ChannelFuture sync() throws InterruptedException;
}
```

Netty 是异步非阻塞的，所有的 I/O 操作都为异步的。

因此，我们不能立刻得到操作是否执行成功，但是，你可以通过 ChannelFuture 接口的 addListener() 方法注册一个 ChannelFutureListener，当操作执行成功或者失败时，监听就会自动触发返回结果。

```java
ChannelFuture f = b.connect(host, port).addListener(future -> {
  if (future.isSuccess()) {
    System.out.println("连接成功!");
  } else {
    System.err.println("连接失败!");
  }
}).sync();
```

并且，你还可以通过`ChannelFuture` 的 `channel()` 方法获取连接相关联的`Channel` 。

```java
Channel channel = f.channel();
```

另外，我们还可以通过 `ChannelFuture` 接口的 `sync()`方法让异步的操作编程同步的。

```java
//bind()是异步的，但是，你可以通过 `sync()`方法将其变为同步。
ChannelFuture f = b.bind(port).sync();
```









### 开发流程

![image-20220617151517055](https://raw.githubusercontent.com/a1254898873/images/master/202206171515241.png)





#### 1、创建客户端handler

```java
/**
 * @author shuang.kou
 * @createTime 2020年05月14日 20:39:00
 */
@Sharable
public class HelloServerHandler extends ChannelInboundHandlerAdapter {

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) {
        try {
            ByteBuf in = (ByteBuf) msg;
            System.out.println("message from client:" + in.toString(CharsetUtil.UTF_8));
            // 发送消息给客户端
            ctx.writeAndFlush(Unpooled.copiedBuffer("你也好！", CharsetUtil.UTF_8));
        } finally {
            ReferenceCountUtil.release(msg);
        }
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) {
        // Close the connection when an exception is raised.
        cause.printStackTrace();
        ctx.close();
    }
}
```

这个逻辑处理器继承自`ChannelInboundHandlerAdapter` 并重写了下面 2 个方法：

1. `channelRead()` ：服务端接收客户端发送数据调用的方法
2. `exceptionCaught()` ：处理客户端消息发生异常的时候被调用





#### 2、创建客户端启动类

我们可以通过 `ServerBootstrap` 来引导我们启动一个简单的 Netty 服务端，为此，你必须要为其指定下面三类属性：

1. **线程组**（*一般需要两个线程组，一个负责处理客户端的连接，一个负责具体的 IO 处理*）
2. **IO 模型**（*BIO/NIO*）
3. **自定义 `ChannelHandler`** （*处理客户端发过来的数据并返回数据给客户端*）

```java
/**
 * @author shuang.kou
 * @createTime 2020年05月14日 20:28:00
 */
public final class HelloServer {

    private  final int port;

    public HelloServer(int port) {
        this.port = port;
    }

    private  void start() throws InterruptedException {
        // 1.bossGroup 用于接收连接，workerGroup 用于具体的处理
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        EventLoopGroup workerGroup = new NioEventLoopGroup();
        try {
            //2.创建服务端启动引导/辅助类：ServerBootstrap
            ServerBootstrap b = new ServerBootstrap();
            //3.给引导类配置两大线程组,确定了线程模型
            b.group(bossGroup, workerGroup)
                    // (非必备)打印日志
                    .handler(new LoggingHandler(LogLevel.INFO))
                    // 4.指定 IO 模型
                    .channel(NioServerSocketChannel.class)
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        public void initChannel(SocketChannel ch) {
                            ChannelPipeline p = ch.pipeline();
                            //5.可以自定义客户端消息的业务处理逻辑
                            p.addLast(new HelloServerHandler());
                        }
                    });
            // 6.绑定端口,调用 sync 方法阻塞知道绑定完成
            ChannelFuture f = b.bind(port).sync();
            // 7.阻塞等待直到服务器Channel关闭(closeFuture()方法获取Channel 的CloseFuture对象,然后调用sync()方法)
            f.channel().closeFuture().sync();
        } finally {
            //8.优雅关闭相关线程组资源
            bossGroup.shutdownGracefully();
            workerGroup.shutdownGracefully();
        }
    }
    public static void main(String[] args) throws InterruptedException {
        new HelloServer(8080).start();
    }

}
```

**1.创建了两个 `NioEventLoopGroup` 对象实例：`bossGroup` 和 `workerGroup`。**

- `bossGroup` : 用于处理客户端的 TCP 连接请求。
- `workerGroup` ： 负责每一条连接的具体读写数据的处理逻辑，真正负责 I/O 读写操作，交由对应的 Handler 处理。

**2.创建一个服务端启动引导/辅助类： `ServerBootstrap`，这个类将引导我们进行服务端的启动工作。**

**3.通过 `.group()` 方法给引导类 `ServerBootstrap` 配置两大线程组，确定了线程模型。**

```
    EventLoopGroup bossGroup = new NioEventLoopGroup(1);
    EventLoopGroup workerGroup = new NioEventLoopGroup();
```

**4.通过`channel()`方法给引导类 `ServerBootstrap`指定了 IO 模型为`NIO`**

- `NioServerSocketChannel` ：指定服务端的 IO 模型为 NIO，与 BIO 编程模型中的`ServerSocket`对应
- `NioSocketChannel` : 指定客户端的 IO 模型为 NIO， 与 BIO 编程模型中的`Socket`对应

**5.通过 `.childHandler()`给引导类创建一个`ChannelInitializer` ，然后指定了服务端消息的业务处理逻辑也就是自定义的`ChannelHandler` 对象**

**6.调用 `ServerBootstrap` 类的 `bind()`方法绑定端口** 。

```
//bind()是异步的，但是，你可以通过 `sync()`方法将其变为同步。
ChannelFuture f = b.bind(port).sync();
```







#### 3、创建服务端的handler

```java

/**
 * @author shuang.kou
 * @createTime 2020年05月14日 20:46:00
 */
@Sharable
public class HelloClientHandler extends ChannelInboundHandlerAdapter {

    private final String message;

    public HelloClientHandler(String message) {
        this.message = message;
    }

    @Override
    public void channelActive(ChannelHandlerContext ctx) {
        System.out.println("client sen msg to server " + message);
        ctx.writeAndFlush(Unpooled.copiedBuffer(message, CharsetUtil.UTF_8));
    }

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) {
        ByteBuf in = (ByteBuf) msg;
        try {
            System.out.println("client receive msg from server: " + in.toString(CharsetUtil.UTF_8));
        } finally {
            ReferenceCountUtil.release(msg);
        }
    }

    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) {
        cause.printStackTrace();
        ctx.close();
    }
}

```

这个逻辑处理器继承自 `ChannelInboundHandlerAdapter`，并且覆盖了下面三个方法：

1. `channelActive()` :客户端和服务端的连接建立之后就会被调用
2. `channelRead` :客户端接收服务端发送数据调用的方法
3. `exceptionCaught` :处理消息发生异常的时候被调用





#### 4、创建服务端启动类

```java
public final class HelloClient {

    private final String host;
    private final int port;
    private final String message;

    public HelloClient(String host, int port, String message) {
        this.host = host;
        this.port = port;
        this.message = message;
    }

    private void start() throws InterruptedException {
        //1.创建一个 NioEventLoopGroup 对象实例
        EventLoopGroup group = new NioEventLoopGroup();
        try {
            //2.创建客户端启动引导/辅助类：Bootstrap
            Bootstrap b = new Bootstrap();
            //3.指定线程组
            b.group(group)
                    //4.指定 IO 模型
                    .channel(NioSocketChannel.class)
                    .handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        public void initChannel(SocketChannel ch) throws Exception {
                            ChannelPipeline p = ch.pipeline();
                            // 5.这里可以自定义消息的业务处理逻辑
                            p.addLast(new HelloClientHandler(message));
                        }
                    });
            // 6.尝试建立连接
            ChannelFuture f = b.connect(host, port).sync();
            // 7.等待连接关闭（阻塞，直到Channel关闭）
            f.channel().closeFuture().sync();
        } finally {
            group.shutdownGracefully();
        }
    }
    public static void main(String[] args) throws Exception {
        new HelloClient("127.0.0.1",8080, "你好,你真帅啊！哥哥！").start();
    }
}
```

**1.创建一个 `NioEventLoopGroup` 对象实例** （*服务端创建了两个 `NioEventLoopGroup` 对象*）

**2.创建客户端启动的引导类是 `Bootstrap`**

**3.通过 `.group()` 方法给引导类 `Bootstrap` 配置一个线程组**

**4.通过`channel()`方法给引导类 `Bootstrap`指定了 IO 模型为`NIO`**

**5.通过 `.childHandler()`给引导类创建一个`ChannelInitializer` ，然后指定了客户端消息的业务处理逻辑也就是自定义的`ChannelHandler` 对象**

**6.调用 `Bootstrap` 类的 `connect()`方法连接服务端，这个方法需要指定两个参数：**

- `inetHost` : ip 地址
- `inetPort` : 端口号





### 编码器和解码器

#### 解码器

解码器负责处理“入站 InboundHandler”数据。 解码器实现了 ChannelInboundHandler，需要将解码器放在 ChannelPipeline中。

**Netty中主要提供了两个解码器抽象基类：**

- ByteToMessageDecoder: 用于将字节转为消息，需要检查缓冲区是否有足够的字节
- MessageToMessageDecoder: 用于从一种消息解码为另外一种消息

**核心方法：** `decode()方法是必须实现的唯一抽象方法。`



```java
public class StringMessageDecoder extends MessageToMessageDecoder<ByteBuf> {

	@Override
	protected void decode(ChannelHandlerContext ctx, ByteBuf msg, List<Object> out) throws Exception {
		System.out.println("StringMessageDecoder 消息正在进行解码...");
		//将 ByteBuf转换为 String，传递到下一个handler
		out.add(msg.toString(CharsetUtil.UTF_8));
	}

}

```







#### 编码器

编码器负责处理“出站 OutboundHandler” 数据。 编码器实现了 ChannelOutboundHandler，需要将解码器放在 ChannelPipeline中。它和上面的解码器的功能正好相反。

**Netty中主要提供了两个编码器抽象基类：**

- MessageToByteEncoder*：将消息编码为字节*
- MessageToMessageEncoder，：将消息编码为消息，T 代表源数据的类型

**核心方法：**`encode()方法是你需要实现的唯一抽象方法。`



```java
public class StringMessageEncoder extends MessageToMessageEncoder<String> {
	@Override
	protected void encode(ChannelHandlerContext ctx, String msg, List out) throws Exception {
		System.out.println("StringMessageEncoder 消息正在进行编码....");
		//将 String转换为 ByteBuf，传递到下一个handler
		out.add(Unpooled.copiedBuffer(msg, CharsetUtil.UTF_8));
	}
}

```





#### 配置

```java
public class MyStringServerHandler extends ChannelInboundHandlerAdapter {

	@Override
	public void channelActive(ChannelHandlerContext ctx) throws Exception {
		System.out.println("MyServerHandler 连接已建立...");
		super.channelActive(ctx);
	}

	@Override
	public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
		//获取客户端发送过来的消息
		System.out.println("Server Accept Client Context ("+ ctx.channel().remoteAddress() +")消息 ->" + msg);

	}

	@Override
	public void channelReadComplete(ChannelHandlerContext ctx) throws Exception {
		//发送消息给客户端
		//ByteBuf byteBuf = Unpooled.copiedBuffer("Server Receive Client msg", CharsetUtil.UTF_8);
		//ctx.writeAndFlush(byteBuf);
	}

	@Override
	public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
		//发生异常，关闭通道
		//cause.printStackTrace();
		ctx.close();
	}
}

```



```java
	private static class StringChannelInitializerImpl extends ChannelInitializer<SocketChannel> {

		@Override
		protected void initChannel(SocketChannel socketChannel) throws Exception {
			// 添加客户端通道的处理器
			socketChannel.pipeline()
					//添加解码器和编码器
					.addLast("stringMessageDecoder", new StringMessageDecoder())
					.addLast("stringMessageEncoder", new StringMessageEncoder())
					.addLast(new MyStringClientHandler());
		}
	}

```

