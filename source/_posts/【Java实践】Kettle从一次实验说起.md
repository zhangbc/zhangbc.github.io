---
title: 【Java实践】Kettle从一次实验说起
type: categories
copyright: false
date: 2019-11-12 12:16:52
tags: Java实践
categories: Java
title_en: java_kettle_01
---


# 一，安装Kettle

## 1，关于简易安装Kettle

第一次接触`kettle`（以前只是听过罢了），摸索了几天，在`mac`源码安装失败，转而快速安装。在`mac`上安装最新版`kettle`并成功启动代码如下：

```bash
☁  ~  brew install kettle
☁  ~  cd /usr/local/Cellar/kettle/8.2.0.0-342/
☁  8.2.0.0-342  cd libexec
☁  libexec  spoon.sh
```

## 2，关于源码尝试安装kettle

- [【Kettle源码下载】：https://github.com/pentaho/pentaho-kettle](https://github.com/pentaho/pentaho-kettle)

```bash
git clone https://github.com/pentaho/pentaho-kettle
# or
git clone git@github.com:pentaho/pentaho-kettle.git
```

- 设置 `setting.xml`

将  `setting.xml` 参见： [settings.xml](https://raw.githubusercontent.com/pentaho/maven-parent-poms/master/maven-support-files/settings.xml)  在你的`Maven`启动目录`/.m2`中。

```bash
☁  pentaho-kettle [master] ⚡  ll /Users/zhangbocheng/.m2
total 8
drwxr-xr-x  97 zhangbocheng  staff  3104 11  8 17:28 repository
-rw-r--r--   1 zhangbocheng  staff  2345 11  8 20:10 setting.xml
```

- 安装

```bash
☁  pentaho-kettle [master] mvn clean install >> /Users/zhangbocheng/Desktop/kettle.log
```

- 关于`error.log`

未设置 `setting.xml`报错问题

```
.....................................
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 47:49 min
[INFO] Finished at: 2019-11-08T17:44:01+08:00
[INFO] Final Memory: 230M/985M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal on project pdi-ce: Could not resolve dependencies for project org.pentaho.di:pdi-ce:pom:9.0.0.0-SNAPSHOT: Could not transfer artifact org.hitachivantara.karaf.assemblies:client:zip:9.0.0.0-20191107.125717-160 from/to pentaho-public (http://nexus.pentaho.org/content/groups/omni/): Failed to transfer file http://nexus.pentaho.org/content/groups/omni/org/hitachivantara/karaf/assemblies/client/9.0.0.0-SNAPSHOT/client-9.0.0.0-20191107.125717-160.zip with status code 502 -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/DependencyResolutionException
[ERROR] 
[ERROR] After correcting the problems, you can resume the build with the command
[ERROR]   mvn <goals> -rf :pdi-ce
```

设置 `setting.xml`后，就一直处在等待中。


# 二，实验案例

关于课程实验，第一次需要亲手搭建`Kettle`，这算是一次比较有意思的工程实践机会，花最少的时间来认识认识比较流行而且强大的`ETL`工具之一--`Kettle`。

## 1，关于实验题目

任务描述：用`kettle`完成下列实验，结果存储到`MySQL`（或者`CSV`）。已知`Excel`文件，包含列（姓名，年龄，身份证号码，性别，挂号日期时间，门诊号），数据若干。

> 生成数据1，包含列（日，性别，儿童/青年/中年/老年，人次），其中儿童/青年/中年/老年的年龄段自己定义；
> 生成数据2，包含列（省份，hour，人次）

第一次接触`kettle`，力求简单，仅考虑输入输出均为`Excel`，首先按照题目要求捏造一批数据，如下图所示：

![元数据](/images/kettle_20191111230208.png)

`Excel`字段说明：

```
姓名：字符串
年龄：整型
身份证号码：字符串
性别：字符串
挂号日期时间：日期时间型	
门诊号：整型
```

进入安装目录`/usr/local/Cellar/kettle/8.2.0.0-342/libexec`启动`kettle`:

![welcome](/images/kettle_20191111230516.png)

根据实验要求，其实所涉及的问题仅仅是输入和输出，转换（分组统计）。创建任务之初，有必要先百度`or Google`看看`kettle`的输入输出是如何实现的？

## 2，实例预热

最容易实现的简单案例就是生成随机数，并存储到`txt`文件。

1）新建一个转换保存为`test_random`（后缀为`.ktr`）通过拖拽插件方式，在核心对象->输入和输出分别拖拽“生成随机数”和“文本文件输出”两个按钮，然后点击“生成随机数”并按下`sheft`键，用鼠标指向“文本文件输出”，以生成剪头，表示数据流向。如下图：

![test_random](/images/kettle_20191111232316.png)

2）编辑输入流，即“生成随机数”按钮，如图所示：

![生成随机数](/images/kettle_20191111232402.png)

关于支持的随机数据类型有：

![随机数据类型](/images/kettle_20191111232421.png)

3）然后编辑输出流，即“文本文件输出”按钮，如图所示：

![文本文件输出](/images/kettle_20191111232535.png)

输出文件名支持预览模式，即点击图中“显示文件名...”按钮：

![显示文件名](/images/kettle_20191111232550.png)

4）最后执行，看看结果。

![log](/images/kettle_20191111234356.png)

![preview_data](/images/kettle_20191111234347.png)

![text](/images/kettle_20191111234407.png)

## 3，实验步骤

通过上述简单实验，我们知道了输入输出流的基本操作，下面开始进入正题。

1）将上述实验中的输入输出全部改为`Excel`。进行相关配置说明如下：

`Excel`输入：

在文件选项下，表格类型根据实际进行适配（`xls or xlsx`），在文件或目录后，点击“浏览”选择自己的源数据文件，然后点击“添加”；

在工作表选项下，点击“获取工作表名称...”添加工作表，即`Excel`中的`sheet`；

在字段选项下，点击“获取来自头部数据的字段...”自动获取字段，由于原`Excel`中整型数据转入会变成浮点型，所以需要进行更改，如图所示：

![字段配置](/images/kettle_20191112095739.png)

最后可以进行预览。

![预览数据](/images/kettle_20191112100005.png)

`Excel`输出：只需要配置输出文件名即可，其他均为默认。

![Excel输出](/images/kettle_20191112100334.png)

2）接下来需要处理的就行核心步骤，即转换。首先针对`生成数据1`进行分析，由于`kettle`中分组需要首先进行排序，从而需要处理的点有：

> （1）将挂号日期时间截取到日；
>
> （2）对年龄按照一定标准进行转换（自己定义）；
>
> （3）按照待分组的字段进行排序；
>
> （4） 进行分组统计。

按照上述思路，在“转换”和“统计”核心对象中，分别找到对应组件，完成基本数据流节点配置，如图所示：

![数据流节点](/images/kettle_20191112101832.png)

在“字段选择”组件中，对时间进行处理。在元数据选项中，需要对`Date`进行转换成`String`，格式设置为`yyyy-MM-dd`,同时可以对字段进行更名操作。另外还可以对字段进行选择，修改，移除。如图所示：

![时间处理](/images/kettle_20191112103403.png)

注意，这里如果不将时间设置为`String`，进行一个小实验可以可以发现，最后存储的依然是带时间的日期，本次实验过程中在这个坎纠结了，错误地以为是`kettle`不支持多关键字（两个以上）排序，如下图所示：

![error1](/images/keetle_20191112104001.png)

![error2](/images/keetle_20191112104011.png)

经过与各位大佬沟通确认，`kettle`是不可能不支持对多关键的排序的，对此深信不疑，那么问题就从`kettle`本身存在的可能`bug`消失了，对一个小白而言，不熟悉`kettle`本身应遵守的规则，这是致命的，只能对怀疑的其他种种可能进行逐一实验了。期间怀疑过待排序关键字的顺序问题，测试发现都不是问题的根本原因，整个过程下来只有对日期做过预处理，而且从错误中发现，引起错排的唯一合理解释就是日期按照预处理之前的原始数据的日期时间型排序的。单独对日期设计实验，如果对预处理生效，那么输出也是预期结果。

- 验证日期实验

输入流，如图所示：

![日期输入流](/images/kettle_20191112105608.png)

假设日期类型不改成`String`，如图所示：

![Date](/images/kettle_20191112110354.png)

输出流，结果预览，如图所示：

![结果预览](/images/kettle_20191112110430.png)

输出流，`Excel`输出，如图所示：

![Excel输出](/images/kettle_20191112110535.png)

验证实验室结果发现，预览数据并没有存储到输出`Excel`中去，然后尝试转换为`String`，输出便一致了。再次验证，`kettle`对日期类数据处理有待提高。

在“数值范围”组件中，对年龄进行处理，划分标准自己定义（如下定义可能存在瑕疵）如图所示。

![年龄处理](/images/kettle_20191112111401.png)

在“排序记录”组件中，按照生成数据要求，需要对日期，性别，年龄段进行来袭，如图所示。

![排序记录](/images/kettle_20191112112231.png)

在“分组”组件中，进行分组统计，如图所示。

![分组](/images/kettle_20191112112450.png)

3）执行，结果如图所示。

![运行结果](/images/kettle_20191112112719.png)

![Excel输出](/images/kettle_20191112112916.png)ß

## 4，实验二简要说明

针对`生成数据2`进行分析，需要处理的点有：

> （1）将挂号日期时间设置`String`，由于不能直接从预设格式中提取日，需要采取字符串截取；
>
> （2）对日期和身份证进行字符串截取，分别提取日和省份代码（身份证前两位）；
>
> （3）按照待分组的字段进行排序；
>
> （4）对省份和时间段进行值映射；
>
> （4） 进行分组统计。

整体设计数据流图，如图所示：

![整体设计数据流图](/images/kettle_20191112115812.png)

在“剪切字符串”组件，设置如下：

![剪切字符串](/images/kettle_20191112115803.png)

在“省份值映射”和“时间值映射”组件中，分别设置如下：

![省份值映射](/images/kettle_20191112120229.png)

![时间值映射](/images/kettle_20191112120242.png)

运行结果，如图所示：

![运行结果previewdata](/images/kettle_20191112120513.png)

![运行结果excel](/images/kettle_20191112120550.png)

# 三，总结

通过本次实验，初步认识了一下强大的`ETL`工具之`kettle`，要想获取更多知识就得更多实验，从错误中反思学到的远比从成功中收获更多。作为工具，只有多多实验才能更好的掌握好它，印证了那句经典--“实践出真知”。
