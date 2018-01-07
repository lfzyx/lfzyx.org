GoCD - 持续交付流水线
=====================

GoCD 是开源的 `持续集成 <https://en.wikipedia.org/wiki/Continuous_integration>`_ 与 `持续交付 <https://en.wikipedia.org/wiki/Continuous_delivery>`_ 系统，与
jenkins 相比，GoCD 注重 workflows 的组合，流水线化 **构建-测试-发布**  生命周期，完全践行了《持续交付:发布可靠软件的系统方法》书中的论点。

安装
----

GoCD 由 GoCD Server 和 GoCD Agents 组成，进行构建需要至少一个 GoCD
Agents.

GoCD Server 提供界面给用户用于控制整个 GoCD 系统，并且分发工作给 agents.

GoCD Agents 完成 server 下发的工作（运行命令，部署工程，等等..）

.. image:: https://www.gocd.org/assets/images/go.cd_server-and-agents-8a7f4cd3.svg

GoCD Server
^^^^^^^^^^^^^^^^

系统资源需求：

-  RAM - minimum 1GB, 2GB recommended
-  CPU - minimum 2 cores, 2GHz
-  Disk - minimum 1GB free space
-  env - Java Runtime Environment (JRE) version 8

::

 echo "deb https://download.gocd.org/" | sudo tee /etc/apt/sources.list.d/gocd.list
 curl https://download.gocd.org/GOCD-GPG-KEY.asc| sudo apt-key add -
 sudo apt update
 sudo apt install apt-transport-https openjdk-8-jre
 sudo apt install go-server

安装后产生如下文件：

::

    /var/lib/go-server #包含二进制文件和数据
    /etc/go #包含 pipeline 配置
    /var/log/go-server #包含 server 日志
    /usr/share/go-server #包含启动脚本
    /etc/default/go-server #包含默认的环境变量

启动： ``sudo systemctl start go-server``

GoCD Agents
^^^^^^^^^^^^^^^^

系统资源需求：

-  RAM - minimum 128MB, 256MB recommended
-  CPU - minimum 2GHz
-  env - Java Runtime Environment (JRE) version 8

在安装 GoCD Server 的过程中已经处理了系统依赖，这里仅需要安装 GoCD Agents ``sudo apt install go-agent`` 安装后产生如下文件：

::

    /var/lib/go-agent #包含二进制文件
    /usr/share/go-agent #包含启动脚本
    /var/log/go-agent #包含 agent 日志
    /etc/default/go-agent #包含默认的环境变量

配置:

如果 GoCD Agents 和 GoCD server 不在同一台服务器上，则需要配置 GoCD
server 的 IP 地址

#. 编辑 /etc/default/go-agent
#. 修改 GO_SERVER_URL=\ https://127.0.0.1:8154/go 这一行，将 127.0.0.1
   改成 GoCD server 的 IP 地址

启动： ``sudo systemctl start go-agnet``

使用
----

在新建 Pipelines 前，我们先了解一些基本的概念

.. image:: https://www.gocd.org/assets/images/go.cd_stage-jobs-tasks-a58d5b3b.svg

-  Task

Task 是 Pipelines 中的最小单元，用于执行一个 linux 命令

-  Job

Job 是 tasks 的集合，其中的 tasks 按照指定的顺序运行。如果其中一个 task
运行失败了，那这个 job 也随之失败，剩下的 task 也不再执行。

-  Stage

Stage 是 jobs 的集合，其中的 job 之间不存在顺序依赖，所以一个 Stage 内的
jobs 可以同时运行。如果其中一个 job 运行失败了，那这个 stage
也随之失败，但其他的 job 依然会执行完成。

-  Pipeline

Pipeline 是 stages 的集合，其中的 stage 按照指定的顺序运行。如果其中一个
stages 失败了，那这个 pipeline 也随之失败，剩下的 stages 也不再执行。

.. image:: https://www.gocd.org/assets/images/go.cd_pipelines-materials-e64b8575.svg

-  Material

material 可以引发 pipeline 运行，material通常是一个源码仓库。GoCD Server
持续的访问 material ，当检测到变更时，pipeline 就开始运行了

新建 Pipelines
^^^^^^^^^^^^^^^^

#. 在 Add pipeline 页面，我们使用项目名作为 pipeline 的名称，并使用 git仓库中的分组作为 pipeline 的分组
#. 配置 Materials，填写 git 仓库地址，并填入分支
#. 配置 Stage/Job，这里 stage 命名为CI , 表明是持续集成阶段
#. 指定 task 动作，这里我们运行一个自带测试的 docker 容器

运行 Pipelines
^^^^^^^^^^^^^^^^

在每次 git 仓库提交后，会触发 Pipelines 运行，GoCD Server 会记录各个阶段的构建结果
