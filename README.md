运行在k8s（openshift）上的kafka集群
====================

kafka可以在不同的broker之间保留副本，对高可用性有一定保障，且它是消息类的，可以不用保证消息的持久化，因此可以放在k8s上承载。
这个编排需要用到之前的zookeeper编排，先生成一个3节点的zk集群，利用这个集群保存kafka的主从信息，和topic信息。
*当前的简单实现中，并未对zk的鉴权和kafka鉴权进行设置*

构建镜像
------------------------

```oc new-build https://github.com/asiainfoLDP/kafka-openshift-orchestration.git --context-dir='image' ```
*注意修改编排文件中的镜像名*

创建编排
------------
创建服务

```
oc create -f kafka-service.yaml
```

创建rc

```
oc create -f kafka-rc.yaml
```

需要替换其中的*instanceid* *zookeeper服务名*

`KAFKA_CREATE_TOPICS`=topic名字|分区数|副本数量|
推荐设置为6 3


绑定和解除绑定
------------
由于没有用户名和密码的认证，简单返回kafka集群的地址就好了。*kafka-service.yaml* 中的内容。
*注意还需要返回zk的地址，便于客户端连接。*

