# CanalTree
阿里Canal技术研究

Mysql主备复制原理

![](https://i.imgur.com/kw224B2.png)

<pre>
复制分成三步：

    1)master将改变记录到二进制日志(binary log)中（这些记录叫做二进制日志事件，
      binary log events，可以通过show binlog events进行查看）；
    2)slave将master的binary log events拷贝到它的中继日志(relay log)；
    3)slave重做中继日志中的事件，将改变反映它自己的数据。
</pre>

Canal工作原理

![](https://i.imgur.com/QiwaVsq.png)

<pre>
      1）Canal模拟mysql slave的交互协议，伪装自己为mysql slave，向mysql master 发送
         dump协议
      2）mysql master收到dump请求，开始推送binary log给slave（也就是canal）
      3）canal解析binary log对象。
</pre>

利用binlog结合canal恢复数据

![](https://i.imgur.com/qSmFMMl.png)

<pre>
解析：
      Camus:
           是Linkedin开源的一个从Kafka到HDFS的数据管道，实际上它是一个MapReduce作业。

      Camus作业三个阶段：
           1）Setup Stage:
                    从Kafka的Zookeeper获取可用的topics,partions,offset等元信息（metadata）
           2) Hadoop Job Stage:
                    开始用若干个task执行topic的数据获取，并写到HDFS
           3) Cleanup Stage：

      Hadoop Stage:
           1) Pulling the data:
                   根据Setup Stage的数据建立Kafka请求，拉取数据，每个task都生成4个文件：
                          Avro data files,
                          Count statistics files,
                          Updated offset files,
                          Error files
           2) Committing the data
                   当一个task完成时，其拉取的数据都被提交到output目录。
           3）Stroring the offset
                   每个partition都有offset，这些offset信息存储在HDFS中。
</pre>