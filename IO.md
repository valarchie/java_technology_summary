# java中IO流的体系？
Java中的流分为两种，一种是字节流，另一种是字符流，分别由四个抽象类来表示（每种流包括输入和输出两种所以一共四个）:InputStream，OutputStream，Reader，Writer。基于这四种IO流父类根据不同需求派生出其他IO流。

# BIO,NIO,AIO?
BIO是同步阻塞IO，NIO是同步非阻塞IO，AIO是异步非阻塞IO；三种IO方式相比较而言，BIO是一个客户端对应一个线程，  
优化的话可以用线程池进行线程复用，但本质还是一个客户端-服务端通信对应一个线程；NIO只需要一个线程负责多路复用  
器selector的轮询，就可以处理不同客户端channel中的读/写事件，所以多个客户端实际只对应一个线程，另外服务器端  
和客户端均使用缓冲区的方式进行读写；AIO不需要轮询去查看读写事件是否就绪，而是由内核通过回调函数通知并完成后续  
操作。


# 讲讲IO里面的常见类，字节流、字符流、接口、实现类
InputStream和OutStream属于字节流。底下有ByteArray、Object、File等实现类，以及Buffer增强。
Reader和Writer属于字符流。底下有String、File、Buffer等实现。

# 讲讲NIO。
NIO通过观测多个缓冲区，哪个缓冲区就绪的话，就处理哪个缓冲区。原本的IO会对一个缓冲区等待，效率比较慢，  
而现在NIO是对一堆缓冲区进行等待，效率比较高。
