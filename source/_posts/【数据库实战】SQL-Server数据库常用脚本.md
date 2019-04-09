---
title: 【数据库实战】SQL Server数据库常用脚本
date: 2019-04-07 16:40:27
tags: 数据库实战
categories: 数据库技术
copyright: false
title_en: sql_used_script
---

#### 1，创建链接远程服务器及其删除
```sql
exec sp_addlinkedserver  'web','','SQLOLEDB','192.168.10.106'
exec sp_addlinkedsrvlogin 'web','false',null,'sa','123'

--删除链接服务器
exec sp_dropserver 'web','droplogins'
```
#### 2，重置SQLSERVER表的自增列，让自增列重新计数  
语法：
```
DBCC CHECKIDENT
(
  table_name
  [, { NORESEED | { RESEED [,new_reseed_value ] } } ]
 )
 [ WITH NO_INFOMSGS ]
 ```

> 参数：
>> - table_name:是要对其当前标识值进行检查的表名。指定的表必须包含标识列。表名必须符合标识符规则。
>> - NORESEED:指定不应更改当前标识值。
>> - RESEED:指定应该更改当前标识值。
>> - new_reseed_value:用作标识列的当前值的新值。
>> - WITH NO_INFOMSGS:取消显示所有信息性消息。

查看某表当前的种子值，示例：
```sql
dbcc checkident('mainTable',noreseed);
```
```
-------------显示如下--------------
--检查标识信息: 当前标识值 '2707'，当前列值 '2707'。
--DBCC 执行完毕。如果 DBCC 输出了错误信息，请与系统管理员联系。
```
重置表mainTable的当前标识值为1，示例：
```sql
dbcc checkident('mainTable',reseed,1);
```
```
-------------显示如下--------------
--检查标识信息: 当前标识值 'NULL'，当前列值 '1'。
--DBCC 执行完毕。如果 DBCC 输出了错误信息，请与系统管理员联系。
```

#### 3，几个有用的存储过程
- 修改xx表中所有值null
```sql
/*************************
   功能：修改xx表中所有列为NULL=''
   作者：by zhangbc
   时间：2015-10-19
*************************/
if (OBJECT_ID('modifyNull','P') is not null)
     drop procedure modifyNull
go
create procedure modifyNull(@table char(100))
as
begin
    --定义游标
    declare col_cur cursor scroll dynamic --scroll表示可以向前或向后移动    dynamic：表示可写也可读
 for
 select b.name from sysobjects a
  inner join syscolumns b on a.id=b.id
  where a.name=@table
    --打开游标
    open col_cur
 declare @columnName nvarchar(100)
 fetch next from col_cur into @columnName
 declare @sql nvarchar(1000)
 while (@@FETCH_STATUS=0)
 begin
   set @sql='update ' + @table + ' set ' + @columnName + ' = ISNULL(' + @columnName + ', '''')'
   exec(@sql)
   fetch next from col_cur into @columnName
 end
 --关闭游标
 close col_cur
 --释放游标
 deallocate col_cur
end
```
- 修改数据库中所有表的所有列为null
```sql
/*************************
   功能：修改数据库中所有表的所有列为NULL=''
   作者：by zhangbc
   时间：2015-10-19
*************************/
create procedure [dbo].[modifyAllNull]
as
begin
  declare tab_cur cursor scroll dynamic --scroll表示可以向前或向后移动    dynamic：表示可写也可读
 for
 select name from sysobjects where xtype='U'
    --打开游标
    open tab_cur
 declare @tableName nvarchar(100)
 fetch next from tab_cur into @tableName
 declare @sql nvarchar(1000)
 while (@@FETCH_STATUS=0)
 begin
   set @sql='exec dbo.modifyNull ' +'''' + @tableName + ''''
   exec(@sql)
   fetch next from tab_cur into @tableName
 end
 --关闭游标
 close tab_cur
 --释放游标
 deallocate tab_cur
end
```