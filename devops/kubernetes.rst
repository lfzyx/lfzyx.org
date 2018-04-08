Kubernetes -  自动化的容器部署、扩展和管理
=============================================

Kubernetes 是一个生产级别的容器编排系统，用于自动部署，扩展和管理容器化应用程序。它将组成应用程序的容器组合成逻辑单元，以便于管理和服务发现。

对象
-----

* Pod

  Pod是Kubernetes的基本构建块，是创建或部署的对象模型中最小和最简单的单元。一个Pod封装了应用程序容器，存储资源和唯一的网络IP。

* Service

  定义了Pod的逻辑集合以及访问它们的策略

* Volume

  一个卷就是一个目录，包含一些数据，可以通过一个容器中的容器访问。

* Namespace

  由同一物理集群支持的多个虚拟集群。 这些虚拟集群被称为命名空间。


组件
-----

Master Node
^^^^^^^^^^^^^

Master 提供集群控制，作出集群的全局决策，检测并响应群集事件。

Master 组件可以运行在集群的任意一台机器上。但一般来说，运行 Master 组件的机器不会运行用户级容器。

* kube-apiserver

  kube-apiserver 用于暴露 Kubernetes API。

* etcd

  etcd 用作群集数据的一致性且高可用的键值存储。

* kube-scheduler

  监控新创建但没有被分配至 node 的 Pod

* kube-controller-manager

  一个循环控制器，通过 apiserver 监视集群的共享状态，并进行更改，试图将当前状态移动到所需的状态。

Worker Node
^^^^^^^^^^^^

Worker Node 组件运行在每一个 Worker node 上，维持 pods 的运行。 Master node 控制每一个 Worker node 。

* kubelet

  保证容器运行在 pod 上。

* kube-proxy

  维持主机上的网络规则并执行连接转发

* Container Runtime

  Kubernetes 支持的运行时: Docker, rkt, runc

安装
-----

系统需求：

* Debian 9
* RAM - minimum 2GB, Swap disabled
* CPU - minimum 2 cores
* docker - 17.03+

1. 安装 kubeadm

::

 apt update && apt install -y apt-transport-https

 curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

 echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

 apt update

 apt install -y kubelet kubeadm kubectl

* kubeadm : 启动集群
* kubelet : 在集群中所有主机上运行，等待 kubeadm 的调用，启动 pods
* kubectl : 用于管理集群的命令行工具


2. master node 初始化

``kubeadm init``

输出::

 You can now join any number of machines by running the following on each node
 as root:

 kubeadm join --token <token> <master-ip>:<master-port> --discovery-token-ca-cert-hash sha256:<hash>


``cp -i /etc/kubernetes/admin.conf $HOME/.kube/config``

3. 安装 pod 网络

``kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.9.1/Documentation/kube-flannel.yml``

运行 ``kubectl get pods --all-namespaces`` ，查看 kube-dns 和 kube-flannel 是否处于运行状态::


    NAMESPACE     NAME                       READY     STATUS    RESTARTS   AGE
    kube-system   etcd-                      1/1       Running   0          6m
    kube-system   kube-apiserver-            1/1       Running   0          6m
    kube-system   kube-controller-manager-   1/1       Running   0          6m
    kube-system   kube-dns-                  3/3       Running   0          7m
    kube-system   kube-flannel-              1/1       Running   0          4m
    kube-system   kube-proxy-                1/1       Running   0          7m
    kube-system   kube-scheduler-            1/1       Running   0          6m

4. 使 master node 参与调度

``kubectl taint nodes --all node-role.kubernetes.io/master-``

使得 master node 可以运行 pods

5. 加入其他 nodes

在其他 nodes 上运行 ``kubeadm join --token <token> <master-ip>:<master-port> --discovery-token-ca-cert-hash sha256:<hash>``

输出::

 Node join complete:
 * Certificate signing request sent to master and response
   received.
 * Kubelet informed of new secure connection details.
