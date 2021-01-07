<h4>Docker</h4>
Docker是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的 Linux或Windows 机器上，也可以实现虚拟化。

docker相关命令：
```
docker build -t search_core:v1 .
docker run --name leol-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d search_core:v1
docker run -it -v c:\Users\shzhulin3\test.txt:/etc/test.txt nginx:latest
docker exec -it 容器id /bin/sh
```

<h4>kubernetes</h4>

<font color=red>*kube-apiserver*</font>

API服务器是kubernetes控制面的组件，kube-apiserver验证并配置api对象的数据，这些对象包括pods、services、replicationcontrollers 等。

<font color=red>*kube-scheduler*</font>

控制平面组件，负责监视新创建的、未指定运行节点（node）的 Pods，选择节点让 Pod 在上面运行。

<font color=red>*kube-controller-manager*</font>

在主节点上运行 控制器 的组件。

从逻辑上讲，每个控制器都是一个单独的进程， 但是为了降低复杂性，它们都被编译到同一个可执行文件，并在一个进程中运行。

这些控制器包括：节点控制器、副本控制器、端点控制器

<font color=red>*容器组Pod*</font>

Pod是k8s集群中运行部署应用或服务的最小单元，一个pod由一个或多个容器组成，Pod中的容器不可分割，同一个Pod中的容器共享网络、存储资源，Pod可以挂载多个共享的存储卷。

<font color=red>*kubelet*</font>

kubelet是在每个节点上运行的主要节点代理，kubelet的主要功能为：Pod管理、容器健康检查、容器监控。

kubectl命令：
```
kubectl get Pods -lapp=demo -owide
```

<font color=red>*kube-proxy</font>

kube-proxy 是集群中每个节点上运行的网络代理， 实现 Kubernetes 服务（Service） 概念的一部分。

kube-proxy 维护节点上的网络规则。这些网络规则允许从集群内部或外部的网络会话与 Pod 进行网络通信。

<h4>其它</h4>

基于数据库的XA协议本质上就是两阶段提交，但由于性能原因在互联网高并发场景下并不适用

Saga的核心就是补偿，一阶段就是服务的正常顺序调用（数据库事务正常提交），如果都执行成功，则第二阶段则什么都不做；但如果其中有执行发生异常，则依次调用其补偿服务（一般多逆序调用未已执行服务的反交易）来保证整个交易的一致性。应用实施成本一般。

TCC的特点在于业务资源检查与加锁，一阶段进行校验，资源锁定，如果第一阶段都成功，二阶段对锁定资源进行交易逻辑，否则，对锁定资源进行释放。应用实施成本较高。