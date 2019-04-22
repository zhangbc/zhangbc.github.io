---
title: 【数据库实践】T-SQL语言及其存储过程
type: categories
copyright: false
date: 2019-04-22 08:35:17
tags: 数据库实践
categories: 数据库技术
title_en: db_tsql_procedure
mathjax: true
---




> 本系列为《数据库系统原理与应用（刘先锋等著）》的读书笔记。
>> 课本第11～13章主要知识点

## 一，T-SQL语言

`T-SQL`语言是`Microsoft`公司在关系型数据库管理系统`SQL Server`中的`SQL-3`标准的实现，是`Microsoft`公司对结构化查询语言(`SQL`)的扩展。`T-SQL`语言是一种交互式的语言，具有功能强大，容易理解和掌握等特点。

1，**数据定义语言(`DDL`)**：`DDL`是指用于定义和管理数据库及数据库中各种对象的语句，包括`create`，`alter`，`drop`等。

- 创建表的语句格式为
```sql
create table 表名
```

- 增加列的语句格式为
```sql
alter table 表名 add 列名 列的描述
```

- 删除列的语句格式为
```sql
alter table 表名 drop column 列名
```

- 修改列定义为
```sql
alter table 表名 alter column 列名 列的描述
```

- 删除表的语句格式为
```sql
drop table 表名
```

2，**数据操纵语言(`DML`)**：`DML`是指用于查询，添加，修改和删除数据库中数据的语句，包括`select`，`insert`，`update`，`delete`等。

3，**数据控制语言(`DCL`)**：`DCL`是指用于设置或更改数据库用户或者角色权限的语句，包括`grant`，`revoke`，`deny`等。 

```sql                                                                         
-- 给所有用户授予select权限
grant select on student to public
go 
​
-- 给指定用户授予特定权限
grant insert,update,delete on student to LiMing,ZhangBin
go
​
-- 联级授权
grant select,update on student to user with [admin] option;
revoke create table from ZhangWei;
revoke select on sc from User;
deny select, insert,update,delete on student to LiMing,ZhangBin;
```

4，**其他语言元素(`ALE`)**

1）**`注释`**：程序代码中不执行的文本字符串（也称`注解`），有：`--`和`/* */`。

2）**变量(`@`)**：局部变量（`@`），全局变量（`@@`）。

```sql
use stuSystem
go
​
declare @row int 
set @row=(select count(1) from student)
```

3）**运算符**

> - 算术运算符：`+`，`-`，`*`，`/`，`%`；
> - 赋值运算符：`=`；
> - 位运算符：`&`，`|`，`^`；
> - 比较运算符：也称关系运算符，用于比较两个表达式的大小或者是否相同，其比较结果是布尔值`TRUE`，`FALSE`，`UNKNOWN`；
> - 逻辑运算符：`and`，`or`，`not`。优先级别：`not` > `and` > `or`；
> - 字符串串联运算符：允许通过加号(`+`)进行字符串串联，这个加号称为`字符串串联运算符`。

4）**函数**

在`T-SQL`语言中，`函数`用于执行一些特殊的运算以支持`SQL Server`的标准命令。

- `行集函数`：在`T-SQL`语句中当成表引用。
```sql
select * from openquery(local, 'select * from student;') ta;
```

- `聚合函数`：用于对一组值进行计算并返回一个单一的值。除`count`之外，聚合函数忽略空值。
```sql
select avg(score) as avg_score,sum(score) as total_score from sc;
```

- `Ranking函数`：为查询结果数据集分区中的每行返回一个序列值。有：`rank`，`dense_rank`，`ntile`，`row_number`。

- `标量函数`：用于对传递给它的一个或多个参数值进行处理和计算，并返回一个单一的值。

| 标量函数           | 说明                                                                                              |
|:---------------------|:-------------------------------------------------------------------------------------|
| 配置函数           | 返回当前的配置信息                                                                     |
| 游标函数           | 返回有关游标的信息                                                                     |
| 日期和时间函数| 对日期和时间输入值进行处理                                                       |
| 数学函数           | 对作为函数参数提供的输入值执行计算                                         |
| 元数据函数       | 返回有关数据库和数据库对象的信息                                             |
| 安全函数           | 返回有关用户和角色的信息                                                           |
| 字符串函数       | 对字符串(char或者varchar)输入值执行操作                                   |
| 系统函数           | 执行操作并返回有关SQL Server中的值、对象和设置的信息        |
| 系统统计函数    | 返回系统的统计信息                                                                      | 
| 文本和图像函数 | 对文本或者图像输入值或者列执行操作，返回有关这些值的信息 |

5）**流程控制语句**

- `if-else`语句
```sql
use stuSystem
select avg(score) from sc;
if (select avg(score) from sc) < 60
print '很抱歉，你没有通过考试！'
else
print '祝贺你，考试通过了！'
```

- `begin-end`语句：能够将多个`T-SQL`语句组合成一个语句块，并将它们视为一个单元处理。

- `go`语句：批的结束语句。`批`是一起提交并作为一个组执行的若干`T-SQL`语句。
```sql
use stuSystem
go

declare @msg varchar(50)
set @msg='Hello world!'
go
```

- `case`语句
```sql
use stuSystem
go
​
select 'score' = 
	case
	   when score is null then '没有成绩'
	   when score < 60 then '不及格'
	   when score < 85 and score >= 60 then '良好'
	   else '优秀' 
	end, 
  cast(sno as varchar(20)) as sno 
from sc where cno = 'C03' order by score;
```

- `while-continue-break`语句

- `goto`语句
```sql
goto label
...
label:
```

- `waitfor`语句
```sql
waitfor [delay 'time'|time 'time']
```

- `return`语句

## 二，存储过程

1，**`存储过程`定义**：几乎包含了所有的`T-SQL`语句，是为了完成特定功能而汇集在一起的一组`SQL`程序语句，经编译后存储在数据库中。

2，**存储过程的调用方法**

```sql
exec proc 存储过程名
exec proc 存储过程名  参数值[,参数值...]
```

3，**存储过程分类**

> i）系统存储过程（前缀为`SP_`）；
> ii）扩展存储过程（前缀为`XP_`）；
> iii）用户自定义存储过程。

4，**存储过程的创建和执行**

-  创建存储过程
```sql
create procedure dbo.pro_sc_insert
@sno char(10),@cno char(2),@score real
as
begin
	insert into sc(sno, cno, score) values(@sno, @cno, @score)
end
​```

- 执行存储过程
```sql
exec pro_sc_insert '2010009', 'C1', 88
```

5，**存储过程中的游标**

1）`游标`的定义：可以把游标看成一种数据类型，用于遍历结果集，相当于指针，或是数组中的下标，分为`局部游标(local)`和`全局游标(global)`。

2）游标的使用方法

- 创建游标
```sql
-- 默认为global；
-- forward_only(默认值)：游标只能前进的，只能从头到尾提取记录；
-- scoll：可以在行间来回跳转。
declare 游标名 cursor [local|global] [forward_only|scoll]
for 
select 查询语句
```

- 使用游标：增加了服务器的负担，使用游标的效果远没有使用默认结果集的效率高，因此，能不用游标尽量不要用。
```sql
declare cur_select_name cursor for select sname from student;
​
declare @name varchar(20)
open cur_select_name
fetch next from cur_select_name into @name
-- fetch_status取值：0正常执行；-1超出了结果集；-2所指向的行已不存在。
  while(@@fetch_status = 0)
	begin
		print '姓名：' + @name
		fetch next from cur_select_name into @name
	end
close cur_select_name
deallocate cur_select_name
```

6，**自动执行的存储过程**

```sql
use master
exec sp_procoption '存储过程名', 'startup', 'on'
```

7，**存储过程的查看，修改和删除**

1）查看存储过程

```sql
-- 显示存储过程的参数及其数据类型
sp_help[[@name=]name]
​
-- 显示存储过程的源代码
sp_helptext[[@objname=]name]
​
-- 显示和存储过程相关的数据库对象
sp_depends[@objname=]'object'
​
-- 返回当前数据库中的存储过程列表
sp_stored_procedures[[@sp_name=]'name']
		[,[@sp_owner=]'owner']
	[,[@sp_qualifier=]'sp_qualifier']
```

2）修改存储过程

```sql
alter procedure stu_info
with encryption
as
  select sno, age from student order by age desc;
go
```

3）删除存储过程

```sql
drop procedure {procedure}[, ...n]
```

8，**`扩展存储过程`**：`SQL Server` 动态装载并执行的动态链接库（`DDL`），只能添加到`master`数据库中。

## 三，触发器及其应用

1，**触发器的概念和工作原理**

1）**`触发器`的概念**：`触发器`是一种特殊类型的存储过程，在执行语言事件时自动生效。其`特殊性`表现：它是在执行某些`T-SQL`语句时自动生效的。

2）**`DML`触发器**：在数据库中发生`DML`事件时启动。将触发器和触发它的语句作为可在触发器内回滚的单个事务对待，如果检测到错误，则整个事务即自动回滚。

3）**`DDL`触发器**：是`SQL Server 2005`的新功能，当服务器或者数据库中发生`DDL`事件时将调用这些触发器。

2，**创建触发器**

1）`DML`触发器**主要优点**

> i）`DML`触发器可通过数据库中相关表实现联级更改；
> ii）`DML`触发器可以防止恶意或者错误的`INSERT`，`UPDATE`及`DELETE`操作，并强制执行比`CHECK`约束定义的限制更为复杂的其他限制；
> iii）`DML`触发器可以评估数据修改前后表的状态，并根据该差异采取措施。

2）`insert`型`DML`触发器：通常用于更新时间标记字段，或者验证被触发器监控的字段中数据满足要求的标准，以确保数据的完整性。

```sql
/* 检测sc表添加数据的合法性，即添加的数据与student表的数据不匹配时，将删除此数据. */
create trigger tri_sc_ins on sc
for insert 
as
begin
    declare @sno char(10)
    select @sno=inserted.sno from inserted 
    if not exists(select sno from student where student.sno=@sno)
        delete from sc where sno=@sno
end
```

3）`update`型`DML`触发器：当在一个有`update`触发器的表中修改记录时，表中原来的记录被移动到删除表中，修改过的记录插入到了插入表中，触发器可以参考删除表和插入表及被修改的表，以确定如何完成数据库操作。

```sql
/* 防止用户修改SC表的成绩 */
create trigger tri_sc_update on sc
for update
as
if update(scorce)
begin
    raiserror('不能修改成绩！', 16, 10)
    rollback transaction
end
go
```

4）`delete`型`DML`触发器

> - 为了防止确实需要删除但会引起数据一致性问题的记录的删除；
> - 执行可删除主记录的子记录的级联删除操作。

```sql
/* 当删除student表中的记录时，自动删除sc表对应学号的记录. */
create trigger tri_del_sc 
on student
for delete
as
begin
    delete @sno char(10)
    select @sno=deleted.sno from deleted
    delete from sc where sno=@sno
end
```

5）**`DDL`触发器**

- `DDL`触发器目的：
> i）防止对数据库架构进行某些更改；
> ii）希望数据库中发生某种情况以响应数据库架构中的更改；
> iii）要记录数据库架构中的更改或者事件。

```sql
/* 防止数据库中的任意表被修改或者删除. */
create trigger tri_safety
on database
for drop_table, alter_table
as
print 'You must disable trigger "tri_safety" to drop or alter tables!'
rollback

/* 防止在数据库中创建表 */
create trigger tri_ban_create
in database
for create_table
as
print 'create table issued.'
select eventdata().value('(/event_instance/TSQLCommand/CommandText)[1]', 'nvarchar(max)')
raiserror('New tables cannot be created in this database.', 16, 1)
rollback
```

3，**查看，修改和删除触发器**

1）查看触发器

```sql
/* 查看触发器的一般信息，如触发器的名称，属性，类型和创建时间 */
sp_help 'trigger_name'

/* 查看触发器的正文信息 */
sp_helptext 'trigger_name'

/* 查看指定触发器所引用的表或者指定的表涉及的所有触发器 */
sp_depends 'trigger_name'
sp_depends 'table_name'
```

2）修改触发器
```sql
sp_rename oldname, newname
```

3）删除触发器
```sql
drop trigger {trigger}[, ...n]
```

4，**触发器的用途**

> 1）可以实现比约束更为复杂的数据约束； 
> 2）可以检查SQL所做的操作是否被允许；
> 3）当一个SQL语句对数据表进行操作时，触发器可以根据该SQL语句的操作情况对另一个数据表进行操作；
> 4）约束的本身是不能调用存储过程的，但是触发器可以调用一个或者多个存储过程；
> 5）在执行完SQL语句之后，触发器可以判断更改过的记录是否达到一定条件，如果达到，触发器可以自动调用SQL Mail来发邮件；
> 6）约束不能返回信息，而触发器可以返回信息；
> 7）可以修改原本要操作的SQL语句；
> 8）为了保护已经建好的数据表，触发器可以在接收到以drop和alter开头的SQL语句中，不进行对数据表的操作。

## 四，嵌入式SQL
    
1，**嵌入式`SQL`简介**

1）`嵌入式SQL`定义：`嵌入式SQL`是一种将`SQL`语句直接写入`C`语言，`COLBOL`，`FORTRAN`，`Ada`等编程语言的源代码中的方法。将`SQL`语句嵌入的目标源码的语言称为`宿主语言`。

2，**嵌入式`SQL`的工作原理**
	
> 提供对于嵌入式SQL的支持，需要数据库厂商除了提供DBMS之外，还必须提供一些工具。为了实现对于嵌入式SQL的支持，技术上必须解决以下4个问题:
>> 1.宿主语言的编译器不可能识别和接受SQL文，需要解决如何将SQL的宿主语言源代码编译成可执行码;
>> 2.宿主语言的应用程序如何与DBMS之间传递数据和消息;
>> 3.如何把对数据的查询结果逐次赋值给宿主语言程序中的变量以供其处理;
>> 4.数据库的数据类型与宿主语言的数据类型有时不完全对应或等价，如何解决必要的数据类型转换问题。
> 嵌入式SQL源码的处理流程 为了解决上述这些问题，数据库厂商需要提供一个嵌入式SQL的预编译器，把包含有嵌入式SQL文的宿主语言源码转换成纯宿主语言的代码。这样一来，源码即可使用宿主语言对应的编译器进行编译。通常情况下，经过嵌入式SQL的预编译之后，原有的嵌入式SQL会被转换成一系列函数调用。因此，数据库厂商还需要提供一系列函数库，以确保链接器能够把代码中的函数调用与对应的实现链接起来。

3，**嵌入式`SQL`的一般形式**

> - 预编译
> - 修改和扩充主语言使之能处理`SQL`语句
