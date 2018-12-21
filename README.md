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