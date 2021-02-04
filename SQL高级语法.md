# <a name="0">SQL高级语法</a>

导入数据表：

`Websites`

| id | name         | url                       | alexa | country |
|---|---|---|---|---|
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |

`access_log`

| aid | site_id | count | date       |
|---|---|---|---|
|   1 |       1 |    45 | 2016-05-10 |
|   2 |       3 |   100 | 2016-05-13 |
|   3 |       1 |   230 | 2016-05-14 |
|   4 |       2 |    10 | 2016-05-14 |
|   5 |       5 |   205 | 2016-05-14 |
|   6 |       4 |    13 | 2016-05-15 |
|   7 |       3 |   220 | 2016-05-15 |
|   8 |       5 |   545 | 2016-05-16 |
|   9 |       3 |   201 | 2016-05-17 |

## <a name="目录">**目录**</a>
<a href="#0">SQL高级语法</a>  
* &emsp;<a href="#1">SQL IN</a>  
* &emsp;<a href="#2">SQL BETWEEN</a>  
* &emsp;<a href="#3">SQL别名(AS)</a>  
&emsp;&emsp;<a href="#4">基本语法</a>  
&emsp;&emsp;<a href="#5">实例</a>  
* &emsp;<a href="#6">SQL连接(JOIN)</a>  
&emsp;&emsp;<a href="#7">JOIN</a>  
&emsp;&emsp;<a href="#8">LEFT JOIN</a>  
&emsp;&emsp;<a href="#9">RIGHT JOIN</a>  

## <a name="1">SQL IN</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>

**IN 操作符允许在 WHERE 子句中规定多个值**
```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...);
```

**IN 与 = 的转换**

	`IN`
```sql
select * from Websites where name in ('Google','菜鸟教程');
```

	`=`
```sql
select * from Websites where name='Google' or name='菜鸟教程';
```

## <a name="2">SQL BETWEEN</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>

BETWEEN 操作符选取介于两个值之间的数据范围内的值，可以是数值、文本或者日期
```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```
## <a name="3">SQL别名(AS)</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>

### <a name="4">基本语法</a>

列的 SQL 别名语法

```sql
SELECT column_name AS alias_name
FROM table_name;
```

表的 SQL 别名语法

```sql
SELECT column_name(s)
FROM table_name AS alias_name;
```

### <a name="5">实例</a>

输入：

```sql
SELECT name AS n, country AS c
FROM Websites;
```

输出：

| n | country |
|---|---|
| Google|USA|
| 淘宝|CN|
| 菜鸟教程|CN|
| 微博|CN|
| Facebook|USA|
| stackoverflow|IND|

输入：

```sql
SELECT name, CONCAT(url, ', ', alexa, ', ', country) AS site_info
FROM Websites;
```

输出：

| name         | site_info |
|---|---|
| Google       | https://www.google.cm/,1,USA     |
| 淘宝          | https://www.taobao.com/,13,CN      |
| 菜鸟教程      | http://www.runoob.com/,4689,CN      |
| 微博          | http://weibo.com/,20,CN      |
| Facebook     | https://www.facebook.com/,3,USA     |
| stackoverflow | http://stackoverflow.com/,0,IND     |

## <a name="6">SQL连接(JOIN)</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>

```sql
SELECT column_name(s)
FROM table1
[INNER/LEFT/RIGHT] JOIN table2
ON table1.column_name=table2.column_name;
```

### <a name="7">JOIN</a>

"Websites" 表中的 "id" 列指向 "access_log" 表中的字段 "site_id",上面这两个表是通过 "site_id" 列联系起来的

```sql
SELECT Websites.id, Websites.name, access_log.count, access_log.date	#输入字段
FROM Websites	#导入表
INNER JOIN access_log	连接表
ON Websites.id=access_log.site_id;	连接条件
```
### <a name="8">LEFT JOIN</a>

下面的 SQL 语句将返回所有网站及他们的访问量（如果有的话）

以下实例中我们把 Websites 作为左表，access_log 作为右表：

输入：

```sql
SELECT Websites.name, access_log.count, access_log.date
FROM Websites
LEFT JOIN access_log
ON Websites.id=access_log.site_id
ORDER BY access_log.count DESC;
```

输出：

|name|count|date|
|---|---|---|
|Facebook|545|2016-05-16|
|Google|230|2016-05-14|
|菜鸟教程|220|2016-05-15|
|Facebook|205|2016-05-14|
|菜鸟教程|201|2016-05-17|
|菜鸟教程|100|2016-05-13|
|Google|45|2016-05-10|
|微博|13|2016-05-15|
|淘宝|10|2016-05-14|
|stackoverflow|NULL|NULL|

*LEFT JOIN 关键字从左表（Websites）返回所有的行，即使右表（access_log）中没有匹配*

### <a name="9">RIGHT JOIN</a>
RIGHT JOIN 关键字从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL
