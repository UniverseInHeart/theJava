## 单机安装部署



启动kafka： 

命令行下进入kafka目录 

修改配置文件 `config/server.properties` 

打开 `listeners=PLAINTEXT://localhost:9092` 

```bash
.\zookeeper-server-start.bat ..\..\config\zookeeper.properties

.\kafka-server-start.bat ..\..\config\server.properties
```



简单性能测试 

```
.\bin\windows\kafka-producer-perf-test.bat --topic testk --num-records 100000 --record-size 1000 --throughput 100000 --producer-props bootstrap.servers=localhost:9092

100000 records sent, 30102.347983 records/sec (28.71 MB/sec), 697.54 ms avg latency, 959.00 ms max latency, 721 ms 50th, 946 ms 95th, 955 ms 99th, 958 ms 99.9th.
```



```
.\bin\windows\kafka-consumer-perf-test.bat  --bootstrap-server localhost:9092 --topic testk --fetch-size 1048576 --messages 100000 --threads 1
 
start.time, end.time, data.consumed.in.MB, 
2021-01-16 23:43:39:646, 2021-01-16 23:43:40:882

MB.sec, data.consumed.in.nMsg, nMsg.sec, rebalance.time.ms, fetch.time.ms, fetch.MB.sec, fetch.nMsg.sec
95.6631, 77.3973, 100310, 81156.9579, 1610811820258, -1610811819022, -0.0000, -0.0001
```

