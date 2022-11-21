### 下载安装RocketMQ
这里已经存在rocketmq-al-4.3.0的zip压缩包

```
# wget http://mirrors.hust.edu.cn/apache/rocketmq/4.3.0/rocketmq-all-4.3.0-source-release.zip
# unzip rocketmq-all-4.3.0-source-release.zip
# mvn -Prelease-all -DskipTests clean install -U
# cd rocketmq-all-4.3.0-bin-release
```

rocket 修改broker和namesrv默认设置的堆内存大小
    修改runbroker.sh JAVA_OPT="${JAVA_OPT} -server -Xms1g -Xmx1g -Xmn512m"
    修改runserver.sh JAVA_OPT="${JAVA_OPT} -server -Xms1g -Xmx1g -Xmn512m -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"

### 启动NameServer
启动后NameServer的端口是9876，请确保自己的9876端口未被占用
```
# nohup sh bin/mqnamesrv &
# tail -f ~/logs/rocketmqlogs/namesrv.log
  The Name Server boot success...
```

### 启动Broker
```
# nohup sh bin/mqbroker -n localhost:9876 &
# tail -f ~/logs/rocketmqlogs/broker.log 
  The broker[%s, 172.30.30.233:10911] boot success...
```

```
在Springboot启动类( SpringbootRocketmqApplication.java 右键点击run )启动后,在浏览器访问 http://localhost:8081/hello/push?msg=23 ， 
    8081 是在application.properties中配置的server.port
    hello/push?msg=123 是有关springboot的requestMapping，在controller/TestController.java中设置的
页面提示{"MsgId":"AC100AB660C618B4AAC2XXXXXXXX"}就表示消息发送成功啦。
我们可以再IDE控制台中看到输出的结果，
```text
发送响应：MsgId:AC100AB660C618B4AAC2XXXXXXXX，发送状态:SEND_OK
接收到了消息：hello
```
这时候我们的整合基本上就完成啦。 