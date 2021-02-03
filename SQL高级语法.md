# SQL高级语法

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

## SQL连接(JOIN)

### 实例

"Websites" 表中的 "id" 列指向 "access_log" 表中的字段 "site_id",上面这两个表是通过 "site_id" 列联系起来的

```sql
SELECT Websites.id, Websites.name, access_log.count, access_log.date	#输入字段
FROM Websites	#导入表
INNER JOIN access_log	连接表
ON Websites.id=access_log.site_id;	连接条件
```

## SQL别名(AS)

### 基本语法

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

### 实例

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