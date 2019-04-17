### kafka的安装及使用

##### 1. 安装

```
tar -xzf kafka_2.11-2.1.0.tgz
cd cd kafka_2.11-2.1.0
```

##### 2. 启动服务

* 运行kafka需要使用Zookeeper，所以你需要先启动Zookeeper，如果你没有Zookeeper，你可以使用kafka自带打包和配置好的Zookeeper

    ```
    //前台启动
    bin/zookeeper-server-start.sh config/zookeeper.properties 
    
    //后台启动
    bin/zookeeper-server-start.sh config/zookeeper.properties > /dev/null 2>&1 &
    
    //停止
    bin/zookeeper-server-stop.sh
    ```

* 现在启动kafka服务
    
    ```
    //前台启动
    bin/kafka-server-start.sh config/server.properties
    
    //后台启动
    bin/kafka-server-start.sh config/server.properties > /dev/null 2>&1 &
    
    //停止
    bin/kafka-server-stop.sh
    ```
    
##### 3. 创建一个主题(topic)

* 创建一个名为“mytopic”的Topic，只有一个分区和一个备份

    ```
    bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic mytopic
    ```

* 创建好之后，可以通过运行以下命令，查看已创建的topic信息

    ```
    bin/kafka-topics.sh --list --zookeeper localhost:2181
    ```
* 删除主题

    ```
    vi server.properties
    //添加
    delete.topic.enable=true
    
    bin/kafka-topics.sh --delete --topic tp_db_log --zookeeper localhost:2181
    ```

##### 4. 发送消息

Kafka提供了一个命令行的工具，可以从输入文件或者命令行中读取消息并发送给Kafka集群。每一行是一条消息。
运行producer（生产者）,然后在控制台输入几条消息到服务器
(生产中一般不用)

```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic mytopic
``` 

##### 5. 消费消息
Kafka也提供了一个消费消息的命令行工具，将存储的信息输出出来
(生产中一般不用)

```
//从开始消费数据
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic mytopic --from-beginning

//消费最新数据
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic mytopic 
```

##### 6. 后期配置

搭建好kafka服务器后, 本机访问ok, 外面的机器的ip无法访问.
要想外网可以访问, 有两种方式, 一种在外网服务器中修改/etc/hosts文件, 这个太麻烦, 在这里不推荐; 我们介绍另一种方式.

```
vi config/server.properties

//找到 #advertised.listeners=PLAINTEXT://your.host.name:9092 这一行, 然后添加:
advertised.listeners=PLAINTEXT://192.168.31.51:9092

//保存退出

```


