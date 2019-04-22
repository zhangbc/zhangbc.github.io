---
title: 【数据库实践】 数据表及其SQL基本操作
type: categories
copyright: false
date: 2019-04-21 20:05:44
tags: 数据库实践
categories: 数据库技术
title_en: db_table_sql
mathjax: true
---


> 本系列为《数据库系统原理与应用（刘先锋等著）》的读书笔记。
>> 课本第09～10章主要知识点


## 一，表的概述

1，**`表`的定义**：表是包含 `SQL Server 2005`数据库中的所有数据的对象。表定义是一个列集合。

2，**`表`的类型**

> 1）`分区表`：将数据水平划分为多个单元的表，这些单元可以分布到数据库中的多个文件组中。
> 2）`系统表`：存储服务器配置及其所有表的数据。
> 3）`用户表`：用户自己创建的数据表和表示示例数据表，用于存储用户的信息，用户可以随意更改。
> 4）`临时表`：分为`本地临时表`和`全局临时表`，存储在`tempdb`中，当不再使用临时表时会自动将其删除。

## 二，创建表

1，**表列的数据类型**

1）精确数字型
	
- 整数类型（`bigint`，`int`，`smallint`，`tinyint`）

> - `bigint`：存储范围为 $-2^{63}$~$2^{63}-1$；占用8个字节。
> - `int`：存储范围为 $-2^{31}$~$2^{31}-1$；占用4个字节。
> - `smallint`：存储范围为 $-2^{15}$~$2^{15}-1$；占用2个字节。
> - `tinyint`：存储范围为 0~255；占用1个字节。
 
- 位数据类型（`bit`）

> - `bit`：存储范围是0和1，占用1个字节。常作为逻辑变量使用，用于表示真、假或者是、否等二值选择。

- 数值类型（`decimal`，`numeric`）

>  - `decimal`：和`numeric`一样，可以用2～17个字节来存储$-10^{38}+1$到$10^{38}-1$之间但是固定精度和小数位数位的数字。表示形式`decimal(p,s)`，其中`p`确定精确的总位数，默认为18位；`s`确定小数位数，默认为0。

- 货币类型（`money`，`smallmoney`）：必须在有效位置前加一个货币单位符号。

> - `money`：用于存储货币值，数值分为一个正数和一个小数分别存储在两个4字节的整型值中，存储范围为$-2^{63}$~$2^{63}-1$，精确到货币单位的千分之一。
> - `smallmoney`：与`money`数据类型相似，但是存储范为$-2^{31}$~$2^{31}-1$。

2）近似数据类型：`real`，`float`。

> - `real`：存储十进制数值，最大可以为7位精确位数。存储范围为$-3.40 \times 10^{-38}$ ~ $3.40 \times 10^{38}$，占用4个字节。
> - `float`：可以精确到15位小数。存储范围为$-1.79 \times 10^{-308}$ ~ $1.79 \times 10^{308}$，占用8个字节。`float(n)`：`n`指定`float`数据的精度，`n`为1～15的整数值。当`n`为1～7时，实际上是定义了一个`real`类型的数据，占用4个字节；当`n`为8～15时，系统认为其是`float`类型，占用8个字节。

3）日期和时间数据类型：`datetime`和`smalldatetime`。

> - `datetime`：存储日期和时间的结合体，存储从1753年1月1日0时到9999年12月31日23时59分59秒，其精确度可以达到三百分之一秒，即3.33ms。占用8个字节，日期和时间分别占用4个字节。默认格式为 `MM DD YYYY hh:mm A.M./P.M.`。
> - `smalldatetime`：与`datetime`类型相似，但是存储范围为1900年1月1日至2079年6月6日。占用4个字节，时间和日期分别占用2个字节，精确度为1min。

4）字符数据类型：`char`，`varchar`，`text`。

> - `char`：定义形式为`char(n)`，`n`表示所有字符占用的存储空间，其取值为1～8000，默认值为1。如果输入数据的字符串长度小于`n`，则系统自动在其后面添加空格来填充；如果输入的数据过长，将会截掉其超出部分。如果定义一个`char`数据类型，且允许该列为空，则该字段被当成`varchar`来处理。
> - `varchar`：定义形式为`varchar(n)`，可存储长达8000个字符的客人变长度字符串。其存储空间是根据存储在表的每列值的字符数变化的。
> - `text`：用于存储文本数据，其容量理论上为 1~$2^{31}-1$，实际应用根据硬盘的存储空间而定。

5）`unicode`字符数据类型：`nchar`，`nvarchar`，`ntext`。

>  - `nchar`：定义形式为`nchar(n)`，`n`的取值为1～4000。与`char`类似，但采用`unicode`标准字符集，`unicode`标准用2个字节为1个存储单位。
>  - `nvarchar`：定义形式为`nvarchar(n)`，`n`的取值为1～4000。与`varchar`类似，但采用`unicode`标准字符集。
>  - `ntext`：理论上容量为$2^{30}-1$，与`text`类似，但采用`unicode`标准字符集。

6）二进制数据类型：`binary`，`varbinary`，`image`。

> - `binary`：定义形式为`binary(n)`，数据存储长度是固定的，即n+4个字节。当输入的二进制数据长度小于n时，余下部分填充0。二进制数据类型的最大长度为8000，常用于存储图像等数据。
> - `varbinary`：定义形式为`varbinary(n)`，数据存储长度是变化的，为实际输入数据的长度加上4个字节，其他类似`binary`。
> - `image`：用于存储图像数据，其理论容量为$2^{31}-1$个字节。

7）其他数据类型：`sql_variant`，`table`，`timestamp`，`uniqueidentifier`，`xml`，`cursor`等。

> - `sql_variant`：用于存储除文本、图形数据和`timestamp`类型数据外的其他任何合法的`SQL Server 2005`数据。
> - `table`：用于存储对表或者视图处理后的结果集。
> - `timestamp`：时间戳数据类型，提供数据库范围内的唯一值。
> - `uniqueidentifier`：用于存储一个16字节长的二进制数据类型，是`SQL Server 2005`根据计算机网络适配器地址和`CPU`时钟产生的全局唯一标识符代码（`GUID`），通过调用 `SQL Server 2005` 的`NEWID()`函数获取。
> - `xml`：存储`xml`类型的数据，其数据容量不能超过2GB。
> - `cursor`：是变量或者存储过程`OUTPUT`参数的一种数据类型，这些参数包含对游标的引用。

8）用户自定义数据类型

- 语法格式如下：

```sql
sp_addtype [@typename=]type, [@phystype=]system_data_type[, [@nulltype=]'null_type'][, [@owner=]'owner_name']
```

- 举例：自定义一个地址(`address`)数据类型。

```sql
exec sp_addtype address, 'varchar(80)', 'not null'
```

2，**列的其他属性**

1）`NULL`，`NOT NULL`和默认值
在数据库中，`NULL`是一个特殊值，表示未知值的概念。默认值是指如果插入行时没有为列指定值，默认值则指定列中使用的值。
	
2）`IDENTITY`属性：实现标识符列。

> - 一个表只能有一个使用`IDENTITY`属性定义的列，且必须通过使用`bigint`，`int`，`smallint`，`tinyint`或者`decimal`，`numeric`数据类型类定义该列；
> - 可指定种子和增量，两者的默认值均为1；
> - 标识符列不允许为空值，也不能包含`default`定义或者对象；
> - 在设置`IDENTITY`属性后，可以使用`$IDENTITY`关键字在选择列表中引用该列，也可以通过名称引用该列；
> - `objectproperty`函数可以用于确定一个表是否具有`IDENTITY`列，`columnproperty`函数可以确定`IDENTITY`列的名称；
> - 通过使值能显示插入，`set identity_insert`可以用于禁用该列的`IDENTITY`属性。

3，**表的创建**

```sql
use stuSystem
go
create table student
(
	studentID int not null primary key,
	studentName char(10) not null,
	class char(10) not null,
	depart char(16) not null,
	yearClass char(6) not null
)
```

## 三，维护表

1，**修改表名与表结构**

```sql
-- 修改表名，将studentcourse修改为course
exec sp_rename 'studentcourse', 'course';
​
-- 修改表结构
alter table student add sex char(2);
alter table student drop column yearClass;
alter table student alter column studentName char(8);
```

2，**删除表**

```sql
drop table course;
```

3，**表数据的维护**

1）添加数据（`insert`）

```sql
insert into course values('10015', '计算机网络', '张三', 2, null);
```

2）更新表数据（`update`）

```sql
update course set cname='计算机组成原理' where cno='10015';
```

3）删除表数据（`delete`）

```sql
delete from student where sno=200901020023;
```

##  四，表数据完整性

1，**`SQL Server`提供的数据类型完整性组件**

| 完整性类型        | 组件                                                                                              |
|:----------------------|:------------------------------------------------------------------------------------|
| 实体完整性        | 索引，unique约束，primary key约束和identity属性                     |  
| 域完整性            | foreign key约束，check约束，default定义，not null定义和规则 |
| 参照完整性        | foreign key约束，check约束和触发器                                          |
| 用户定义完整性 | create table中的所有列级和表级约束，存储过程和触发器          |

2，**`primary key`约束**

表通常具有一列或者一组列可以用于唯一标识表中的一行，这样的一列或者多列称为表的`主键`（`PK`），用于强制表的实体完整性。

3，**`foreign key`约束**

`外键`是用于建立和加强两个表数据之间的链接的一列或者多列，用于强制参照完整性。

> - 一个表 中最多可以有253个参照表，因此每个表最多可以有253个`foreign key`约束；
> - 在`foreign key`约束中，只能参照同一个数据库中的表；
> - `foreign key`子句中的列数目和每个列指定的数据类型必须和`reference`子句中的相同；
> - `foreign key`约束不能自动创建索引；
> - 参照同一个表中的列时，必须只使用`reference`子句，而不能使用`foreign key`子句；
> - 在临时表中，不能使用`foreign key`约束。

4，**`check`约束**：通过限制列可接受的值，`check`约束可以强制域的完整性。

5，**表关系**：显示某个表中的列如何链接到另一个表的列；可以防止出现冗余数据。

## 五，视图

1，**视图概述**

1）`视图`是一个虚拟表，是由若干个表或者视图中导出的表，其结构和数据是建立在对表的查询基础上的，其内容由查询定义。

2）视图的**主要优点**和**作用**：

> i）着重于特定数据：视图使用户能够着重于他们所感兴趣的特定数据和所负责的特定任务，不必要的数据或者敏感数据可以不出现在视图中；
> ii）简化数据操作：视图可以简化用户处理数据的方式；
> iii）提供向后兼容性：视图能够在表的架构更改时为表创建向后兼容接口；
> iv）自定义数据：视图允许用户以不同方式查看数据，即使在他们同时使用相同的数据时也是如此；
> v）导出和导入数据：可使用视图将数据导出到其他应用程序；
> vi）跨服务器组合分区数据：使用分区视图，可以使用多个服务器对数据进行分区。

3）视图**分类**：

> i）标准视图：组合了一个或者多个表中的数据，可以获得使用视图的大多数好处，包括将重点放在特定数据上及简化数据操作。
> ii）索引视图：被具体化了的视图，即它已经经过计算并存储，可以为视图创建唯一的聚集索引。
> iii）分区视图：在一台或者多台服务器间水平连接一组成员表的分区数据。

2，**创建视图**

1）创建视图**原则**

> i) 只能在当前数据库中创建视图；
> ii) 视图名称必须遵循标志符的规则，且对每个用户必须唯一；
> iii) 可以在其他视图和引用视图的过程之上创建视图；
> iv) 定义视图的查询不能包括`order by`，`compute`，`compute by`子句或者`into`关键字；
> v) 不能在视图上定义全文索引定义；
> vi) 不能创建临时视图，也不能在临时表上创建视图；
> vii) 不能对视图执行全文查询，但是如果查询所引用的表被配置为支持全文索引，就可以在视图定义中包含全文查询。

- 举例：

```sql
use stuSystem
go
​
create view view_teacher_choice as 
select b.tname, a.chsu from course a, teacher b where a.tid=b.tid;
```

3，**使用视图**

1）使用视图进行数据检索

```sql
select * from view_teacher_choice;
```

2）通过视图修改数据

> - 如果在视图定义中使用了`with check option`子句，则所有在视图上执行的数据修改语句都必须符合定义视图的`select`语句中所设定的条件。
> - `SQL Server` 必须能够明确地解析对视图所引用基表中的特定行所做的修改操作。
> - 对基表中须更新而又不允许空值的所有列，其值在`insert`语句或者`default`定义中指定。
> - 如果在视图删除数据，在视图定义的`from`子句中只能列出一个表。
> - 视图修图数据通过`insert`，`update`，`delete`语句来完成。

4，**修改视图**

```sql
use stuSystem
go
​
alter view view_teacher_choice as 
select b.tname, a.chsu from course a, teacher b where a.tid=b.tid and a.chsu > 30;
```

5，**重命名视图**

```sql
exec sp_rename 'view_teacher_choice', 'view_teacher_choice_total';
```

6，**查看视图**

```sql
use stuSystem
go
​
exec sp_helptext 'view_teacher_choice_total';
```

7，**删除视图**

```sql
use stuSystem
go
​
drop view view_teacher_choice_total;
```

## 六，索引

1，**索引概述**

1）**`索引`定义**：`索引`是对数据库表中一个或者多个列的值进行排序而创建的一种存储结构。

2）索引分类：

> i）`聚集索引(clustered)`：保证数据库表中记录的物理顺序与索引顺序相同，一个表只能有一个聚集索引。
> ii）`非聚集索引(nonclustered)`：数据库表中记录的物理顺序与索引顺序可以不相同，一个表可以有多个非聚集索引。
> iii）**唯一索引(`unique`)**：表示表中的任何两个记录的索引值都不相同，与表的主键类似，确保索引列不包括重复的值；
> iv）**组合索引**：将两个或者多个字段组合起来的索引，而单独的字段允许不是唯一的值。

2，**创建索引**

```sql
use stuSystem
go
​
create index idx_time in course(choice_time asc);
```

3，**查看索引**

```sql
exec sp_helpindex idx_time;
```

4，**删除索引**

```sql
drop index course.idx_time;
```

## 七，SQL操作查询

1，**简单查询**

```sql
select sno,sn,age from student;
select * from student;
select distinct sno from course;
select sn as sname,sno,age from student;
select sn,age-5 from student;
select avg(age) from student;
```

2，**带条件的列查询**

> - 比较大小和确定范围
> - 部分匹配查询
> - 查询的排序

```sql
select sno,score from sc where cno='C01';
select sno,cno,score from sc where score>80;
select sno,cno,score from sc where (cno='C01' or cno='C02') and score>80;
select sno,sn from student where age between 18 and 20;
select sno,sn,cno from student where score is null;
select sno,cno,score from sc where cno in('C01', 'C02');
select sno,sn from student where sn like '李%';
select sno,score from sc where cno='C01' order by score desc;
select sum(score) as total_score, avg(score) as avg_score from sc where sno='0001';
select max(score) as max_score,min(score) as min_score,max(score)-min(score) as diff from sc where cno='C01';
select count(distinct dept) as dept_num from student;
select sno,sum(score) aas total_score from sc where score >= 60 group by sno having(*) >= 3 order bu sum(score) desc; 
```

3，**多表查询**

所谓`多表查询`，即在两个或者两个以上的表中进行的查询操作，分为：`连接查询`和`子查询`（`嵌套查询`）。

- **连接查询**

> 1）内连接
>> - `等值连接`：在连接条件中使用等于号(`=`)运算符，与被连接列的列值进行比较，在查询结果中列出被连接表中的所有列，包括其中的重复列。
>> - `不等连接`：在连接条件中使用除等于号(`=`)以外的其他比较运算符，与被连接列的列值进行比较，这些运算符包括`>`，`>=`，`<`，`<=`，`!>`，`!<`，`<>`。
>> - `自然连接`：在连接条件中使用等于号(`=`)运算符，与被连接列的列值进行比较，在查询结果中列出被连接表中的所有列，但会删除其中的重复列。

```sql
select * from student a inner join sc b on a.sno=b.sno;
```

> 2）交叉连接（笛卡尔积）：两个关系中所有元组的任意组合。

```sql
select * from student cross join sc;
```

> 3）自连接：如果在一个连接查询中，涉及的两个表都是同一张表，这种查询称为`自连接查询`。

```sql
select a.* from student a inner join student b on a.cno=b.cno and b.sno='20090701027';
```

> 4）外连接：其查询结果既包含那些满足条件的行，又包含其中某个表的全部行。
>> - 左外连接（`left join`）
>> - 右外连接（`right join`）
>> - 全外连接（`full join`）

```sql
select a.sno,a.sname,a.class,a.cno,b.score from student a left join sc b on a.sno=b.sno;
select a.sno,a.sname,a.class,a.cno,b.score from student a right join sc b on a.sno=b.sno;
select a.sno,a.sname,a.class,a.cno,b.score from student a full join sc b on a.sno=b.sno;
```

- **子查询**

> - 在where子句中包含一个形如select-from-where的查询块，此查询称为`子查询`或者`嵌套查询`，包含子查询的语句称为`父查询`或者`外部查询`。
> 基本关键字：`any`，`in`，`all`，`exists`

```sql
select tname from teacher where tno=any(select tno from tc where cno='C05');
select tname from teacher where tno in (select tno from tc where cno='C05');
select tname,sal from teacher where sal>all(select sal from teacher where dept='电力系') and dept!='电力系';
select tname from teacher where exists (select 1 from tc where teacher.tno=tc.tno and cno='C05');
```