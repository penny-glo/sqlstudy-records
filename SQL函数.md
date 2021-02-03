# SQL函数

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


## COUNT() 

###基本语法

```sql
SELECT COUNT(column_name) FROM table_name;	#返回指定列的值的数目
SELECT COUNT(*) FROM table_name;	#返回表中的记录数
SELECT COUNT(DISTINCT column_name) FROM table_name;	#返回指定列的不同值的数目
```

### 简单应用

下面的 SQL 语句计算 "access_log" 表中 "site_id"=3 的总访问量：

```sql
SELECT COUNT(count) AS nums FROM access_log
WHERE site_id=3;
```

下面的 SQL 语句计算 "access_log" 表中不同 site_id 的记录数：

```sql
SELECT COUNT(DISTINCT site_id) AS nums FROM access_log;
```

## GROUP BY

### 基本语法
```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```

### 简单应用

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

### 多表链接

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

## HAVING

### 基本语法

HAVING 子句可以筛选分组后的各组数据

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;
```

### 简单应用

查找总访问量大于 200 的网站

```sql
SELECT Websites.name, Websites.url, SUM(access_log.count) AS nums FROM (access_log
INNER JOIN Websites
ON access_log.site_id=Websites.id)
GROUP BY Websites.name
HAVING SUM(access_log.count) > 200;
```

