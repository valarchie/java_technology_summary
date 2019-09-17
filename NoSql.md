# 1.redis的主从复制怎么做的？
    Redis主从复制可以根据是否是全量分为全量同步和增量同步。以下对其相应的同步过程及原理做下简要说明。
    增量同步

    Redis增量同步主要指Slave完成初始化后开始正常工作时，Master发生的写操作同步到Slave的过程。通常情况下，
    Master每执行一个写命令就会向Slave发送相同的写命令，然后Slave接收并执行。
    全量同步

    Redis的全量同步过程主要分三个阶段：

    同步快照阶段：Master创建并发送快照给Slave,Slave载入并解析快照。Master同时将此阶段所产生的新的写命令存储到缓冲区。
    同步写缓冲阶段：Master向Slave同步存储在缓冲区的写操作命令。
    同步增量阶段：Master向Slave同步写操作命令。


# 2.redis为什么读写速率快性能好？
1. Redis将数据存储在内存上，避免了频繁的IO操作
2. Redis其本身采用字典的数据结构，时间复杂度为O(1),且其采用渐进式的扩容手段
3. Redis是单线程的，避免了上下文切换带来的消耗，采用网络IO多路复用技术来保证在多连接的时候， 系统的高吞吐量。

# 3.redis为什么是单线程的？
我们要去理解为什么多线程好，多线程好在IO慢的时候才能凸显出高性能，因为在一个线程等待IO的时候马上切换线程运行。
而在Redis中数据都在内存当中，IO非常快，如果还是采用多线程的话，反而会浪费时间在线程的上下文切换。

# 4.缓存的优点？
1、 减少了对数据库的读操作，数据库的压力降低  2、 加快了响应速度 


# 5.aof与rdb的优点和区别？
    持久化RDB（redis database）和AOF(append only file)的区别

    RDB持久化是指在指定的时间间隔内将内存中的数据集快照写入磁盘，实际操作过程是fork一个子进程，先将数据集写入临时文件，  
    写入成功后，再替换之前的文件，用二进制压缩存储。 RDB持久化保存键空间的所有键值对(包括过期字典中的数据),并以二进制
    形式保存，符合rdb文件规范，根据不同数据类型会有不同处理。

    AOF持久化以日志的形式记录服务器所处理的每一个写、删除操作，查询操作不会记录，以文本的方式记录，可以打开文件看到详细
    的操作记录。AOF持久化保存redis服务器所执行的所有写命令来记录数据库状态,在写入之前命令存储在aof_buf缓冲区。

    持久化RDB和AOF的优缺点：

    RDB存在哪些优势呢？
    (1) 一旦采用该方式，那么你的整个Redis数据库将只包含一个文件，这对于文件备份而言是非常完美的。比如，你可能打算每个小时
    归档一次最近24小时的数据，同时还要每天归档一次最近30天的数据。通过这样的备份策略，一旦系统出现灾难性故障，我们可以非常
    容易的进行恢复。
    (2) 对于灾难恢复而言，RDB是非常不错的选择。因为我们可以非常轻松的将一个单独的文件压缩后再转移到其它存储介质上。
    (3) 性能最大化。对于Redis的服务进程而言，在开始持久化时，它唯一需要做的只是fork出子进程，之后再由子进程完成这些持久化
    的工作，这样就可以极大的避免服务进程执行IO操作了。
    (4) 相比于AOF机制，如果数据集很大，RDB的启动效率会更高。

    RDB又存在哪些劣势呢？

    (1) 如果你想保证数据的高可用性，即最大限度的避免数据丢失，那么RDB将不是一个很好的选择。因为系统一旦在定时持久化之前出现
    宕机现象，此前没有来得及写入磁盘的数据都将丢失。
    (2). 由于RDB是通过fork子进程来协助完成数据持久化工作的，因此，如果当数据集较大时，可能会导致整个服务器停止服务几百毫秒，
    甚至是1秒钟。

    AOF的优势有哪些呢？
    (1) 该机制可以带来更高的数据安全性，即数据持久性。Redis中提供了3种同步策略，即每秒同步、每个命令同步和不同步。事实上，
    每秒同步也是异步完成的，其效率也是非常高的，所差的是一旦系统出现宕机现象，那么这一秒钟之内修改的数据将会丢失。而每个命令
    同步，我们可以将其视为同步持久化，即每次发生的数据变化都会被立即记录到磁盘中。可以预见，这种方式在效率上是最低的。至于无
    同步，由操作系统决定任何将缓冲区里面的命令写入磁盘里面,在这种模式写,服务器遭遇意外停机时,丢失命令的数据是不确定的
    (2) 由于该机制对日志文件的写入操作采用的是append模式，因此在写入过程中即使出现宕机现象，也不会破坏日志文件中已经存在的
    内容。然而如果我们本次操作只是写入了一半数据就出现了系统崩溃问题，不用担心，在Redis下一次启动之前，我们可以通过redis-
    check-aof工具来帮助我们解决数据一致性的问题。
    (3) 如果日志过大，Redis可以自动启用rewrite机制。主要是为了解决aof文件不断增长减少重复键读写操作的日志, 通过定时触发,
    重新根据实际内存键值情况写入新的aof文件, 再进新旧替换文件,实现aof文件最小化。
    (4) AOF包含一个格式清晰、易于理解的日志文件用于记录所有的修改操作。事实上，我们也可以通过该文件完成数据的重建。

    AOF的劣势有哪些呢？
    (1) 对于相同数量的数据集而言，AOF文件通常要大于RDB文件。RDB 在恢复大数据集时的速度比 AOF 的恢复速度要快。
    (2) 根据同步策略的不同，AOF在运行效率上往往会慢于RDB。总之，每秒同步策略的效率是比较高的，同步禁用策略的效率和RDB
    一样高效。

    二者选择的标准，就是看系统是愿意牺牲一些性能，换取更高的缓存一致性（aof），还是愿意写操作频繁的时候，不启用备份来
    换取更高的性能，待手动运行save的时候，再做备份（rdb）。rdb这个就更有些 eventually consistent的意思了。不过生产环境
    其实更多都是二者结合使用的。
    
    
# 6.redis的List能用做什么场景？
1.消息队列。2.排行榜。3.最新列表。