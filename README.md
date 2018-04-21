# MARK-SQL-SERVER-
使用 SQL SERVER 的一些笔记

1.远程链接数据库/跨服务器链接数据库

数据库1:192.168.3.12

数据库2:192.168.3.23\SQL2008R2

实现数据库1上可以直接访问数据库2上的表:

```
 EXEC  sp_addlinkedserver
 @server='XDERP',   --链接服务器别名
 @srvproduct='',
 @provider='SQLOLEDB',
 @datasrc='192.168.3.23\SQL2008R2'  --要访问的的数据库所在的服务器的ip
 GO


 EXEC sp_addlinkedsrvlogin
 'DBMES',                  --链接服务器别名
 'false', 
  NULL,
 'sa',                     --要访问的数据库的用户              
 'xxxx'                    --要访问的数据库，用户的密码
  GO
```


***

2.查询数据的所有表,及每个表包含的数量

```
 --表和表的行数.
 select object_name(id),rows,* from sysindexes 
 where indid in (0,1)
 order by rows desc

 --表.
 select * from sys.tables

 --表.
 select * from INFORMATION_SCHEMA.TABLES

 --表里面的列.
 select * from INFORMATION_SCHEMA.COLUMNS
```


