MongoDB - 文档数据库
======================

MongoDB 是一个开源的跨平台的文档数据库，属于 NoSQL 数据库分类。MongoDB
避开了传统的关系型数据库结构，使用类JSON的动态文档，使得数据的整合更快更简单。 [#]_

主要功能
-------------

* 面向文档

 与存储标题和作者在两个关系型结构中不同，在 MongoDB
 中可以把标题，作者和其他标题相关的信息都存储在一个名为Book中的单个文件。

* 指定查询

 MongoDB
 支持通过字段，范围，正则表达式查询。查询可以返回的文档的特定字段，并且还包括用户定义的JavaScript函数。

* 索引

 MongoDB 文件中的所有字段都可以索引

* 复制

 MongoDB 提供了高可用的复制，一个复制集包含两个或多个数据副本。
 每个副本可以在任何时间成为主副本或者从副本。主副本提供读和写，从副本保持从主副本复制数据。当主副本下线，从副本将自动成为主副本。

* 负载均衡

 MongoDB 可以运行在多个服务器，负载均衡和复制数据，保证系统启动并在出现硬件故障时运行。

* 文件存储

 MongoDB 可以作为一个文件系统使用，有利于负载均衡和数据复制。这个功能，称为
 GridFS，被包含在MongoDB 驱动中。

安装
-----

::

 sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
 echo "deb http://repo.mongodb.org/apt/debian wheezy/mongodb-org/3.0 main" >> /etc/apt/sources.list.d/mongodb-org-3.0.list
 apt-get update
 apt-get install mongodb-org

配置
----

* /etc/mongod.conf mongoDB的配置文件

* /var/lib/mongodb 数据文件

* /var/log/mongodb 日志文件

入门知识
--------

* 文档

 文档是 MongoDB 中数据的基本单元，非常类似于关系数据库中的行。
 文档是键值对的一个有序集，键是字符串，值可以是多种不同的数据类型。
 文档必须有一个 \_id 键。键的值默认是ObjectId类型

* 集合

 集合是一组文档，相当于表。在一个集合里面，文档的 \_id 是唯一的。

* 数据库

 多个集合组成数据库

CRUD 操作
---------

* 插入

 db.test.insert({"":})
 插入的数据必须小于16M

* 删除

 db.test.remove({"":})
 emove函数接受键值参数作为删除条件

* 更新

   * $set

        $set 用来更新一个键的值，如果键不存在，则创建。
        db.blog.update({"_id":ObjectId("5603697db13466f29ba8e673")},{"$set":{"fa":"War
        and peace","wow":"lol"}})



   * $unset

        $unset 用来清除一个键值
        db.blog.update({"_id":ObjectId("5603697db13466f29ba8e673")},{"$unset":{"fa":1}})



   * $inc

        $inc 用来增加一个键的值，如果键不存在，则创建。
        db.blog.update({"_id":ObjectId("5603697db13466f29ba8e673")},{"$inc":{"fa":55}})



   * upsert

        将 update 的第三个参数设置为 true
        upsert 可以避免竟态问题
        db.blog.update({"_id":ObjectId("5603697db13466f29ba8e673")},{"$inc":{"fa":55}},
        true)



   * 更新多个文档

        update默认情况只能对符合匹配条件的第一个文档执行更新。
        要更新多个文档需要将 update 的第四个参数设置为 true
        db.blog.update({"title" : "second blog
        post"},{"$set":{"123":456}},true,true)

* 查询

 mongoDB使用 find 来查询集合中的文档。

 db.blog.find()

 批量返回集合blog中的所有文档

 db.blog.find({"age":27})

 返回键值包含 {"age":27} 的文档

 db.blog.find({"title":"second", "age":27})

 返回键值包含{"title":"second"} AND {"age":27} 的文档

 find 的第二参数用于限定要返回的文档的键值。

 db.blog.find({"title" : "second blog post"}, {"conntent":1})

 返回值仅包含 _id 和 conntent 键

 db.blog.find({"title" : "second blog post"}, {"conntent":0, "title":0})

 返回值不包含 conntent 和 title 键

 $lt $lte $gt $gte $ne 对应 < <= > >= ≠

 db.blog.find({"age":{"$gte":18,"$lt":30}})

 返回 age 大于18小于30的文档

文件存取操作
------------

文件存取基于二进制数据类型，下面用 python 进行文件存储操作

::

 from pymongo import MongoClient
 client = MongoClient()
 db = client.test
 from bson import binary
 file = open('test.txt', 'rb')
 bin = binary.Binary(file.read())
 db.test.insert({"file":bin})
 file.close()

验证

::

 cursor = db.test.find()
 for document in cursor:
 print (document)

写入

::

 cursor = db.test.find_one()
 file = open("buff.txt","wb")
 file.write(cursor['file'])
 file.close()

GridFS
------

Gridfs是用来支持大小超过16MB的文件的，不超过16M的文件存成文档就行了。Gridfs
的理念是：将大文件分割为多个块，将每个块作为独立的文档进行存储。

::

 mongofiles put index-bottom.png
 mongofiles list
 mongofiles gett index-bottom.png

.. rubric:: 参考文献

.. [#] `MongoDB - Wikipedia, the free encyclopedia <https://en.wikipedia.org/wiki/MongoDB>`_
