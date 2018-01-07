Linux内核需要为临时对象如任务或者设备结构和节点分配内存，缓存分配器管理着这些类型对象的缓存。现代Linux内核部署了该缓存分配器以持有缓存，称之为片,不同类型的片缓存由片分配器维护,该命令显示了实时内核片缓存信息。
语法[编辑]

-s 根据指定的排序标准对这些字段排序
a: sort by number of active objects
b: sort by objects per slab
c: sort by cache size
l: sort by number of slabs
v: sort by number of active slabs
n: sort by name
o: sort by number of objects (the default)
p: sort by pages per slab
s: sort by object size
u: sort by cache utilization