<a>[Top]</a>
# <a name="0">SQL函数</a>

导入数据表：

| id | name         | url                       | alexa | country |
|---|---|---|---|---|
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |

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

## 目录

* <a href="#1">COUNT() </a>  
&emsp;&emsp;<a href="#15">实例</a> 
&emsp;&emsp;<a href="#2">实例</a>  
* <a href="#3">SUM()</a>  
&emsp;&emsp;<a href="#4">基本语法</a>  
&emsp;&emsp;<a href="#5">实例</a>  
&emsp;<a href="#6">GROUP BY</a>  
&emsp;&emsp;<a href="#7">基本语法</a>  
&emsp;&emsp;<a href="#8">实例</a>  
* &emsp;<a href="#9">HAVING</a>  
&emsp;&emsp;<a href="#10">基本语法</a>  
&emsp;&emsp;<a href="#11">实例</a>  
* &emsp;<a href="#12">EXISTS</a>  
&emsp;&emsp;<a href="#13">基本语法</a>  
&emsp;&emsp;<a href="#14">实例</a>  


## <a name="1">COUNT() </a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>


### <a name="15">基本语法</a>

```sql
SELECT COUNT(column_name) FROM table_name;	#返回指定列的值的数目
SELECT COUNT(*) FROM table_name;	#返回表中的记录数
SELECT COUNT(DISTINCT column_name) FROM table_name;	#返回指定列的不同值的数目
```

### <a name="2">实例</a>

下面的 SQL 语句计算 "access_log" 表中 "site_id"=3 的总访问量：

```sql
SELECT COUNT(count) AS nums FROM access_log
WHERE site_id=3;
```

下面的 SQL 语句计算 "access_log" 表中不同 site_id 的记录数：

```sql
SELECT COUNT(DISTINCT site_id) AS nums FROM access_log;
```

## <a name="3">SUM()</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>

### <a name="4">基本语法</a>

SUM() 函数返回数值列的总数

```sql
SELECT SUM(column_name) FROM table_name;
```

### <a name="5">实例</a>
输入：

```sql
SELECT SUM(count) AS nums FROM access_log;
```

输出：

|nums|
|---|
|1569|

## <a name="6">GROUP BY</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>

### <a name="7">基本语法</a>
```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```

### <a name="8">实例</a>

* 简单应用

统计 access_log 各个 site_id 的访问量：

输入：

```sql
SELECT site_id, SUM(access_log.count) AS nums
FROM access_log GROUP BY site_id;
```

输出：

|site_id|count|
|---|---|
|1|275|
|2|10|
|3|521|
|4|13|
|5|750|

* 多表链接

输入：

```sql
SELECT Websites.name,COUNT(access_log.aid) AS nums FROM access_log
LEFT JOIN Websites
ON access_log.site_id=Websites.id
GROUP BY Websites.name;
```

输出：

|name|nums|
|---|---|
|Facebook|2|
|Google|2|
|微博|1|
|淘宝|1|
|菜鸟教程|3|

## <a name="9">HAVING</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>

### <a name="10">基本语法</a>

HAVING 子句可以筛选分组后的各组数据

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;
```

### <a name="11">实例</a>

查找总访问量大于 200 的网站

```sql
SELECT Websites.name, Websites.url, SUM(access_log.count) AS nums FROM (access_log
INNER JOIN Websites
ON access_log.site_id=Websites.id)
GROUP BY Websites.name
HAVING SUM(access_log.count) > 200;
```

## <a name="12">EXISTS</a><a style="float:right;text-decoration:none;" href="#index">[Top]</a>
### <a name="13">基本语法</a>

EXISTS 运算符用于判断查询子句是否有记录，如果有一条或多条记录存在返回 True，否则返回 False

```sql
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);
```

### <a name="14">实例</a>

查找总访问量(count 字段)大于 200 的网站是否存在

```sql
SELECT Websites.name, Websites.url 
FROM Websites 
WHERE EXISTS (SELECT count FROM access_log WHERE Websites.id = access_log.site_id AND count > 200);
```

EXISTS 可以与 NOT 一同使用，查找出不符合查询语句的记录

```sql
SELECT Websites.name, Websites.url 
FROM Websites 
WHERE NOT EXISTS (SELECT count FROM access_log WHERE Websites.id = access_log.site_id AND count > 200);
```
