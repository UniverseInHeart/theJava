## Redis 真的变慢了吗？

查看 Redis 的响应延迟，当前环境下的 Redis 基线性能

从 2.8.7 版本开始，`redis-cli`  命令提供了`–intrinsic-latency` 选项，可以用来**监测和统计测试期间内的最大延迟**，这个延迟可以作为 Redis  的基线性能。其中，测试时长可以用 `–intrinsic-latency` 选项的参数来指定。

```bash
// 举个例子，比如说，我们运行下面的命令，该命令会打印 120 秒内监测到的最大延迟。

./redis-cli --intrinsic-latency 120
Max latency so far: 17 microseconds.
Max latency so far: 44 microseconds.
Max latency so far: 94 microseconds.
Max latency so far: 110 microseconds.
Max latency so far: 119 microseconds.

36481658 total runs (avg latency: 3.2893 microseconds / 3289.32 nanoseconds per run).
Worst run took 36x longer than the average latency.

// 可以看到，这里的最大延迟是 119 微秒，也就是基线性能为 119 微秒。
// 一般情况下，运行 120 秒就足够监测到最大延迟了，所以，我们可以把参数设置为 120。
```



你要把运行时延迟和基线性能进行对比，如果你观察到的 Redis 运行时延迟是其基线性能的 2 倍及以上，就可以认定 Redis 变慢了。

判断基线性能这一点，对于在虚拟化环境下运行的  Redis  来说，非常重要。这是因为，在虚拟化环境（例如虚拟机或容器）中，由于增加了虚拟化软件层，与物理机相比，虚拟机或容器本身就会引入一定的性能开销，所以基线性能会高一些。

下面的测试结果，显示的就是某一个虚拟机上运行 Redis 时测的基线性能。

```bash
$ ./redis-cli --intrinsic-latency 120
Max latency so far: 692 microseconds.
Max latency so far: 915 microseconds.
Max latency so far: 2193 microseconds.
Max latency so far: 9343 microseconds.
Max latency so far: 9871 microseconds.
```

可以看到，由于虚拟化软件本身的开销，此时的基线性能已经达到了 9.871ms。如果该 Redis 实例的运行时延迟为 10ms，这并不能算作性能变慢，因为此时，运行时延迟只比基线性能增加了  1.3%。如果你不了解基线性能，一看到较高的运行时延迟，就很有可能误判 Redis 变慢了。

不过，我们通常是通过客户端和网络访问 Redis  服务，为了避免网络对基线性能的影响，刚刚说的这个命令需要在服务器端直接运行，这也就是说，我们只考虑服务器端软硬件环境的影响。

如果你想了解网络对  Redis 性能的影响，一个简单的方法是用 `iPerf` 这样的工具，测量从 Redis  客户端到服务器端的网络延迟。如果这个延迟有几十毫秒甚至是几百毫秒，就说明，Redis  运行的网络环境中很可能有大流量的其他应用程序在运行，导致网络拥塞了。这个时候，你就需要协调网络运维，调整网络的流量分配了。