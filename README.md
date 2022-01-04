# TiGi

TiDB for postgresql on Gitlab, TiDB hackathon 2021.



# 团队成员

- Orion7r：现就职于神州数码，TiDB for PostgreSQL核心研发，分布式数据库爱好者
- xiaoqianer：现就职于神州数码，强大的PM，温柔的小姐姐（单身）
- Mystery-cyf：现就职于神州数码，分布式数据库爱好者，没有解决不了的问题

# 项目介绍

TiDB for PostgreSQL是一个兼容PostgreSQL 11 的计算引擎，能够兼容原生TiDB的存储引擎TiKV，以及调度层PD，组成一个兼容pg协议的TiDB集群，具备postgreSQL的语法优势以及TiDB的分布式架构特点，为日益壮大的 PostgreSQL 用户，提供一个可水平扩展的、高可用的分布式数据库 TiDB For PostgreSQL。

而在本次hackathon上，我们希望能够让Gitlab运行在TiDB for PostgreSQL的集群上，同时，想要做一个疯狂的举措，让几十个Gitlab实例跑在TiDB for PostgreSQL的集群上，看看其能否承受住这个压力。



# 背景

在最新数据库流行程度排行榜，开源数据库领域，PostgreSQL 已经来到第四名的位置，可以说，其受众群体非常广泛。但是 PostgreSQL 数据库作为关系型数据库来说，有一定的局限性，比如无法水平扩展。所以，我们想把 TiDB 集群与 PostgreSQL 进行融合，让 TiDB For PostgreSQL 能够发挥他们各自的优点。



# 项目设计

我们的设计其实就是得益于TiDB整个集群的高度分层架构。纵观整个TiDB集群，三个主要组件TiDB、TiKV、PD各司其职，既然TiDB-Server兼容的是MySQL协议，那是不是也能有一个兼容PostgreSQL协议的TiDB-Server？这就有了我们的idea，做一个兼容PostgreSQL协议的TiDB。

那么问题来了，怎么去体现TiDB For PG替代TiDB之后的集群的高可用性？在TiDB集群中，使用TiDB For PG替代TiDB之后，整个集群是否可用？是否高可用？极限在哪？与原集群的差异在哪？

所以才有了现在hackathon的项目。我们将使用Gitlab 作为应用层，数据存储采用TiDB For PG替代TiDB的TiDB For PG集群。如下是我们的项目架构图：

![image-20211230162624875](https://raw.githubusercontent.com/Orion7r/PicGo-img/main/img/image-20211230162624875.png)









# 功能模块

在本次Hackathon中，我们预计实现的基本功能有：

- 基础PostgreSQL协议的兼容；
- 针对Gitlab的PG关键字的兼容；
- 针对Gitlab的PG系统函数的兼容；
- PostgreSQL相关系统表的兼容；



ps： 后续我们会逐步完善对PostgreSQl的兼容，包括兼容PostgreSQL数据库结构、:: 转义符、PostgreSQL特有数据类型的兼容等。



# 兼容性测试

当兼容了一部分 PostgreSQL 系统库、函数、关键字等一些关键点时，我们就可以开始尝试验证兼容性。验证内容为：

1. 登录页面访问
2. 用户登录
3. 项目查看
4. 分支查看
5. 新建分支
6. 删除分支
7. 用户退出



# 高可用测试

我们会部署100个 gitlab 实例，并发对 TiDB4PG 集群进行压测，并记录测试数据。

100个  gitlab 实例会使用不同的用户和不同的项目，来创建分支、删除分支的动作进行压测。

