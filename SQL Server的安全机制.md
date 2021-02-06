# SQL Server的安全机制

<a name="目录">**目录**</a>

* &emsp;<a href="#0">身份认证模式</a>  
* &emsp;<a href="#1">登录账户</a>  
* &emsp;<a href="#2">数据库用户</a>  
* &emsp;<a href="#3">权限管理</a>  
&emsp;&emsp;<a href="#4">对象级别的权限</a>  
&emsp;&emsp;<a href="#5">语句级别的权限</a>  
* &emsp;<a href="#6">角色</a>  
&emsp;&emsp;<a href="#7">固定服务器角色（系统角色）</a>  
&emsp;&emsp;<a href="#8">固定数据库角色（系统角色&用户定义角色）</a>  
&emsp;&emsp;<a href="#9">用户定义的角色</a>  

## <a name="0">身份认证模式</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>
* Windows身份验证模式

	SQL将通过Windows操作系统获得用户信息，并对登录名和密码进行验证
* 混合身份验证模式
	
	允许Windows授权用户和SQL授权用户登录到SQL服务器

## <a name="1">登录账户</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>
登录账户：

* SQL自身负责身份验证的登录账户
* 登录到SQL的Windows网络账户，可以是组账户或用户账户

---

1. 建立登录账户

	CREATE  LOGIN login_name

	```sql
	CREATE LOGIN SQL_User1WITH PASSWORD=‘a1b2c3XY’
	```

2. 修改登录帐户属性

	ALTER  LOGIN login_name

	```sql
	ALTER LOGIN SQL_User1 WITH PASSWORD=‘a4b5c6XY’
	ALTER LOGIN SQL_User3 WITH NAME=NewUser
	```

3. 删除登录帐户

	DROP  LOGIN login_name

## <a name="2">数据库用户</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>

登录账户：链接到SQL Server服务器，不具有访问任何数据库的权限

*映射*

数据库用户：访问该数据库

*新建立的数据库只有一个用户：dbo

---
1. 建立数据库用户

	CREATE USER user_name[|FOR|FROM]

	LOGIN login_name	#登录账户

2. Guest用户（匿名访问）启用与禁用

	启用：GRANT CONNECT TO guest

	禁用：REVOKE CONNECT TO guest
3. 删除数据库用户

	DROP USER user_name

## <a name="3">权限管理</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>

### <a name="4">对象级别的权限</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>
SELECT（查询）

INSERT（插入）

UPDATE（更新）

DELETE（删除）

REFERENCES（有外键约束）

EXECUTE（执行储存过程和标量函数）

1. 授权语句

	```sql
	GRANT 对象权限 
		ON 
			对象 
			TO （主体：数据库用户名或角色)
			[WITH GRANT OPTION]
	```

	**实例**

	```sql
	GRANT SELECT ON Addres TO abc
	GRANT EXECUTE ON OBJECT::HR.EI TO abc
	GRANT REFERENCES（EmployeeID）ON vEmp TO abc WITH GRANT OPTION
	```

2. 拒绝权限

	```sql
	DENY 对象权限 ON
		对象
			TO （主体：数据库用户名或角色）
		[CASCADE]
		[AS主体]
	```

	**实例**

	```sql
	DENY SELECT ON Addres TO abc
	DENY EXECUTE ON OBJECT::HR.EI TO abc
	DENY REFERENCES（EmployeeID）ON vEmp TO abc CASCADE
	```

3. 收权语句

	```sql
	REVOKE 对象权限
		ON
			对象
			TO （主体：数据库用户名或角色）
			[CASCADE][AS角色]
	```

	**实例**

	```sql
	REVOKE SELECT ON Addres TO abc
	REVOKE EXECUTE ON OBJECT::HR.EI TO abc
	REVOKE REFERENCES（EmployeeID）ON vEmp TO abc CASCADE
	```

### <a name="5">语句级别的权限</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>
CREATE DATABASE

CREATE PROCEDURE

CREATE TABLE

CREATE VIEW

CREATE FUNCTION

BACKUP DATABASE

BACKUP LOG

1. 授权语句：GRANT CREATE TABLE, CREATE VIEW TO user1. user2
2. 拒绝权限：DENY CREATE VIEW TO user1
3. 收权语句：REVOKE CREATE DATABASE FROM user0

## <a name="6">角色</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>

角色：可以将一组具有相同权限的用户组织在一起

### <a name="7">固定服务器角色（系统角色）</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>

|固定服务器角色|描述|
|---|---|
|Bulkadmin|执行BULK INSERT语句权限|
|Dbcreator|创建、修改、删除还原数据库权限|
|Diskadmin|具有管理磁盘文件的权限|
|Processadmin|管理运行进程权限|
|Securtyadmin|专门管理登录账户、读取错误日志执行CREATE DATABASE权限的账户|
|Serveradmin|服务器级别的配置选项和关闭服务器权限|
|Setupadmin|添加删除链接服务器。|
|Sysadmin|系统管理员 ，Windows超级用户自动映射为系统管理员|
|Public|系统预定义服务器角色|

1. 为固定服务器角色添加成员

	将user1添加到sysadmin角色中

	EXEC Sp_addsrvrolemember ‘user1’，‘sysadmin’

2. 删除固定服务器角成员

	从sysadmin删除user1成员

	EXEC Sp_dropsrvrolemember ‘user1’，‘sysadmin’

### <a name="8">固定数据库角色（系统角色&用户定义角色）</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>

|固定数据库角色|描述|
|---|---|
|Db_accessadmin|添加或删除数据库权限|
|Db_backupoperator|备份数据库、日志权限|
|Db_datareader|查询数据库数据权限|
|Db_datawriter|具有插入、删除、更改权限|
|Db_ddladmin|执行数据定义的权限|
|Db_denydatareader|不允许具有查询数据库中所有用户数据的权限|
|Db_denydatawriter|不允许具有插入、删除、更改数据库中所有用户数据权限|
|Db_owner|具有全部操作权限，包括配置、维护、删除数据库|
|Db_securityadmin|具有管理数据库角色、角色成员以及数据库中语句和对象的权限|

1. 为固定数据库角色添加成员

	EXEC Sp_addrolemember ‘Db_datareader’，‘SQL_User2’
2. 删除固定服务器角成员

	EXEC Sp_droprolemember ‘Db_datareader’,‘SQL_User2’

### <a name="9">用户定义的角色</a><a style="float:right;text-decoration:none;" href="#目录">[Top]</a>

用户定义的角色属于数据库一级

角色中的成员拥有的权限=成员自身权限+所在角色权限，但若某个权限在角色中被拒绝，则成员不再拥有

1. 创建用户定义的角色 CREATE ROLE

	CREATE ROLE MathDept [AUTHORIZATION Software]
2. 删除用户定义角色 DROP ROLE

	DROP ROLE MathDept
