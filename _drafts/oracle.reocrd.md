数据库的组成：
------

1. `instance`: 一块共享内存区域和一组后台进程， 他是数据库对外提供服务的接口
2. `database`: 组成数据库的一组文件集合。

**我们不能绕过`instance` 直接访问数据库文件**

------

`oltp` 系统: 在线交易系统， 适合事务短并发高的业务


实例和数据库之间的关系
----

`oracle` 数据库和实例之间的关系是一对多的关系，一个实例只能和一个数据库关联，一个数据库可以和多个实例关联。
  `rac`保护实例。`dbguid`保护数据库。


------------------


- 查看当前数据库的`sql` 语句的解析情况：

        select name,value from v$sysstat where name like 'parse%'

- 查看软解析命中率

        select sum(pinhits)/sum(pins)*100 from v$librarycache;
