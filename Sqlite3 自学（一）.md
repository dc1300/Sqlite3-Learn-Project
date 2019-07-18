安装之类的就不缀叙了，文章用于本人自学，如有不对之处，望不吝赐教，从菜鸟教程Sqlite学习开始入门

环境：mac电脑，Sqlite3  sqlite-autoconf-3290000.tar.gz

创建数据库 sqlite3 testDB.db

创建两张表

> CREATE TABLE COMPANY( ID INT PRIMARY KEY NOT NULL, NAME TEXT NOT NULL, AGE INT NOT NULL, ADDRESS CHAR(50), SALARY REAL);
>
> CREATE TABLE DEPARTMENT( ID INT PRIMARY KEY NOT NULL, DEPT CHAR(50) NOT NULL, EMP_ID INT NOT NULL);

查看数据库 .databases

查看表名 .tables

查看表结构  .schema 

![image-20190718102833533](/Users/daichen1/Library/Application Support/typora-user-images/image-20190718102833533.png)

如果忘了句尾的分号，就会变成换行了，效果如下：

![img](https://upload-images.jianshu.io/upload_images/3621105-700976e4154252aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

再重新补上分号即可恢复正常

设置显示模式为列模式：

`.mode column`

打印系统时间 

`SELECT CURRENT_TIMESTAMP;`

打印当前时区的时间 

`select datetime('now','localtime');`

插入数据

```sqlite
INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (6, 'Kim', 22, 'South-Hall', 45000.00 );
```

或

```sqlite
INSERT INTO COMPANY VALUES (7, 'James', 24, 'Houston', 10000.00 );
```

查询

1.1 模糊查询

```sqlite
sqlite> SELECT * FROM COMPANY WHERE NAME LIKE 'Ki%';
```

或

```sqlite
sqlite> SELECT * FROM COMPANY WHERE NAME GLOB 'Ki*';
```



1.2  Like子句

百分号（%）代表零个、一个或多个数字或字符。下划线（_）代表一个单一的数字或字符。这些符号可以被组合使用

| 语句                      | 描述                                            |
| :------------------------ | :---------------------------------------------- |
| WHERE SALARY LIKE '200%'  | 查找以 200 开头的任意值                         |
| WHERE SALARY LIKE '%200%' | 查找任意位置包含 200 的任意值                   |
| WHERE SALARY LIKE '_00%'  | 查找第二位和第三位为 00 的任意值                |
| WHERE SALARY LIKE '2_%_%' | 查找以 2 开头，且长度至少为 3 个字符的任意值    |
| WHERE SALARY LIKE '%2'    | 查找以 2 结尾的任意值                           |
| WHERE SALARY LIKE '_2%3'  | 查找第二位为 2，且以 3 结尾的任意值             |
| WHERE SALARY LIKE '2___3' | 查找长度为 5 位数，且以 2 开头以 3 结尾的任意值 |

1.2  Glob子句

大小写敏感

星号（*）代表零个、一个或多个数字或字符。问号（?）代表一个单一的数字或字符。这些符号可以被组合使用。

| 语句                      | 描述                                            |
| :------------------------ | :---------------------------------------------- |
| WHERE SALARY GLOB '200*'  | 查找以 200 开头的任意值                         |
| WHERE SALARY GLOB '*200*' | 查找任意位置包含 200 的任意值                   |
| WHERE SALARY GLOB '?00*'  | 查找第二位和第三位为 00 的任意值                |
| WHERE SALARY GLOB '2??'   | 查找以 2 开头，且长度至少为 3 个字符的任意值    |
| WHERE SALARY GLOB '*2'    | 查找以 2 结尾的任意值                           |
| WHERE SALARY GLOB '?2*3'  | 查找第二位为 2，且以 3 结尾的任意值             |
| WHERE SALARY GLOB '2???3' | 查找长度为 5 位数，且以 2 开头以 3 结尾的任意值 |

1.3  Limit 子句

```sqlite
sqlite> SELECT * FROM COMPANY LIMIT 3 OFFSET 2;
```

从下标为2的元素开始查，向后查三个元素

![image-20190718114803999](/Users/daichen1/Library/Application Support/typora-user-images/image-20190718114803999.png)

![image-20190718114825573](/Users/daichen1/Library/Application Support/typora-user-images/image-20190718114825573.png)

1.4 Order By 子句 和 Group By 子句

先插入两条数据

```sqlite
INSERT INTO COMPANY VALUES (8, 'James', 44, 'Texas', 5000.00 );
INSERT INTO COMPANY VALUES (9, 'James', 45, 'Texas', 5000.00 );
INSERT INTO COMPANY VALUES (10, 'James', 45, 'Texas', 5000.00 );
```

再把 ORDER BY 子句与 GROUP BY 子句一起使用

```
SELECT NAME, SUM(SALARY) 
         FROM COMPANY GROUP BY NAME ORDER BY NAME DESC;
```

1.5 Having 子句

HAVING 子句允许指定条件来过滤将出现在最终结果中的分组结果。

WHERE 子句在所选列上设置条件，而 HAVING 子句则在由 GROUP BY 子句创建的分组上设置条件。在一个查询中，HAVING 子句必须放在 GROUP BY 子句之后，必须放在 ORDER BY 子句之前。

```sqlite
 SELECT * FROM COMPANY GROUP BY name HAVING count(name) < 2 Order by name asc;
```

![image-20190718115640271](/Users/daichen1/Library/Application Support/typora-user-images/image-20190718115640271.png)

1.6 Distinct 关键字

用于消除重复记录的 DISTINCT 关键字的基本语法如下：

```sqlite
SELECT DISTINCT name FROM COMPANY;
```