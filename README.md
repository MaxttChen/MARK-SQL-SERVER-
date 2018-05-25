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
***
3.续1的跨服务器连接数据;发现两个数据库的排序不一致导致能关联数据
数据库1: xx
数据库2:Chinese_PRC_CS_AS
```
--使用 collate 转换
;WITH CTE AS (
XXX
)SELECT T1.*,T2.ITEM_CODE,T2.ITEM_NAME,T2.ITEM_SPECIFICATION FROM CTE  AS T1
JOIN ITEM AS T2(NOLOCK) 
ON  T1.NAME = T2.ITEM_NAME collate  Chinese_PRC_CS_AS 
AND T1.cInvStd collate  Chinese_PRC_CS_AS = T2.ITEM_SPECIFICATION 
AND T2.ITEM_CODE LIKE 'F%'
```

***
4. for xml path 的妙用,合并同元素  ,结合  STUFF(text_value,start,len,text_replace) 函数

SELECT B.sName,LEFT(StuList,LEN(StuList)-1) as hobby FROM (  
SELECT sName,  
STUFF((SELECT ','+hobby FROM student   
  WHERE sName=A.sName   
  FOR XML PATH('')),1,1,'') AS StuList  
FROM student A   
GROUP BY sName  
) B    




***
