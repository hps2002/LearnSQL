# SQL语句：

1、group_concat([DISTINCT] 要连接的字段 [Order BY ASC/DESC 排序字段] [Separator '分隔符'])
通过group_concat将查询到的不同字段进行连接，并且按照排序字段从之排序输出，通过分隔符进行分割

2、conditions 条件构造，构造查询的条件，使用模糊查询

3、聚合搜索
select * from tab_a;可以看作是一个表， 所以可以将这个查询出来的结果作为一个表进行再次查询

## 比较运算符
* <=>    安全等于运算符，安全地判断两个值、表达式是否相等，与等号运算符相同。只不过加入了NULL的类型。简单点说就是，等号两边为NULL的话返回1，等号有一边为NULL返回0，而不是返回NULL。
 select * from table where A <=> B
* =      等于运算符，判断两个值是否相等，相等返回1，不相等返回0
使用规则：
等号两边都为字符串，按照字符串中的字符的ANSI编码是否相等；
等号两边都为证书，则比较两个整数是否相等
等号两边一边为整数一边为字符串，将字符串转化为整数进行比较
等号两边有一个为NULL，结果为NULL
 select * from table where A = B
* <>或者!=  不等于运算符，判断两个值或者字符串或者表达式是否不等于 select * from table where A != B
* <    小于运算符，判断前面的值、字符串或者表达式是否小于后面的值、字符串或者表达式 select * from table where A < B;
* >    大于运算符，判断前面的值、字符串或者表达式是否大于后面的值、字符串或者表达式     select * from table where A > B;
* >= 和 <=  分别为大于等于运算符和小于等于运算符，作用类似大于和小于运算符;

## 非符号运算符
* IS NULL&emsp;为空运算符&emsp;select * from table where A is NULL;      
* IS NOT NULL&emsp;不为空运算符&emsp;select * from table where A IS NOT NULL;
* LEAST &emsp;最小值运算符&emsp;select * from table where C least(A, B);
* GREATEST&emsp;最大值运算符&emsp;select * from table where C greatest(A, B);
* BETWEEN AND &emsp;两值之间运算符&emsp;select * from table where C between A and B;//闭区间[A, B]
* ISNULL&emsp;为空运算符&emsp;select * from table where C isnull;
* IN&emsp;属于运算符&emsp;select * from table where A in (C, B);
* NOT IN&emsp;不属于运算符&emsp;select * from table where A not in (C, B);
* LIKE&emsp;模糊匹配&emsp;select * from table where A like B;
* REGEXP&emsp;正则表达式&emsp;select * from table where A regexp B;
* RLIKE&emsp;正则表达式&emsp;select * from table where A rlike B;

## 运算符优先级
括号的优先级最大，赋值的优先级最低 （具体查看mysql运算符优先级表）

##  使用正则表达式查询
正则表达式被用来检索或者替换某个模式的文本内容，根据指定的匹配模式匹配文本中符合要求的特殊字符串。
*  ^, 匹配文本的开始字符串     '^b'匹配以字母b开头的字符串
*  '$', 匹配文本的结束字符串     'st$'匹配以st结束的字符串
*  ., 匹配任何单个字符         'b.k'匹配任何b和k之间有一个字符的字符串
*  *, 匹配0个或多个在它前面的字符串     'f * n'匹配n前面有任意和f的字符串
*  +, 匹配前面的字符一次或多次  'ba+'匹配以b开头后面紧跟a的字符串
*  '指定文本', 匹配包指定文本的字符串   'str'匹配所有包含"str"的字符串
*  [字符集合], 匹配包含字符集合中任何一个字符串     '[zx]'匹配包含x或者z的字符串
*  [^], 匹配不包含括号内的任何一个字符  '[^abc]匹配不包含a或者b或者c的字符串
*  指定字符串{n}，匹配前面的字符串至少n次   b{2}匹配至少有2个b的字符串
*  指定字符串{n, m}, 匹配前面的字符串至少n次，至多m次   str{2, 3}匹配至少2个，最多3个的字符串

## 排序
使用orderby子句进行排序：ASC（升序）、DESC（降序），order by在select语句的结尾
* 单列排序
直接在order by后面进行排序的对象中直接使用升序或者降序进行输出即可：select id, name from table ORDER BY id ASC;
* 多列排序
多列排序的时候，只有在第一列的值相同的情况下第二列的排序才会触发

## 分页
MySQL中使用LIMIT进分页, LIMIT [位置偏移量], [行数]
位置偏移量表示从哪行开始，行数表示返回的记录条数
比如:select * from table LIMIT 5, 10;&emsp;表示从第6条记录开始，返回10条记录条数。

* 分页显示公式:(当前页数 - 1) * 每页条数, 每页条数
LIMIT (PageNo - 1) * PageSize, PageSize;
* LIMIT必须放在整个select语句的最后面, 
* 在不同的DNMS中，分页的关键字不同，在MySQL、PostreSQL、MariaDB、SQLite中使用LIMIT关键字

## 多表查询
多表查询是为了解决笛卡尔积不匹配的问题
多表查询分了很多类
* 等值连接和非等等值连接
等值连接是指将表A和表B中的一摸一样的列作为连接点，构建出一张新的表，一般等值连接都是采用一个表A的主键和另外一个表B的外键，其中表B选择的外键其实就是表A的主键；
非等值连接其实就是在表A中找到一列属性，这列属性在表B的某两个属性的范围内进行的连接。一般在员工的薪资等级的逻辑中出现。

* 自连接和非自连接<br>
自连接就是自己连接自己形成一个新的表，使用别名的方式虚拟成两张表，进行内连接、外连接进行查询<br>
非自连接就是将两个不同的表进行内连接或者外连接进行查询<br>
* 内连接和外连接
**内连接**：合并具有同一列的两个以上的表的行，返回不包含一个表与另一个表不匹配的行；<br>
**外连接**：两个表在连接的过程中除了返回满足连接条件的行以外， 还返回左或者右表中不满足条件的行，不满足条件的行表中相应的列应该为空。<br>
**左外连接**：连接条件中左边的表为主表右边的表为从表<br>
**右外连接**：连接条件中右边的表为主表左边的表为从表<br>

### 多表连接的方式
#### SQL92中使用(+)创建连接
在SQL92种采用(+)表示从表所在的位置，即左（右）外连接中，(+)表示哪个是从表<br>
**使用语法**<br>
```
select table2.name, table1.id from table1, table2 where table1.id = table2.id(+); --表示table2是从表
```
#### SQL99多表查询的方法

**内连接(INSERT JOIN ... ON)**
使用JOIN ... ON 语句创建连接的语法结构
```
SELECT a.name, b.id, c.class
FROM a
    JOIN b on a.id = b.id
        JOIN c on c.name = b.name;
```
上面的SQL语句表达的是，先将表a和表b按照id对应的条件进行连接形成一张新的表, 新的表按照原来的表b和新的表c按照name对应的形成一张表。<br>
按照这种规则将表a、b、c连接起来.

除了`JOIN ... ON`还有`INSERT JOIN ... ON`和`CROSS JOIN`。

上面提到的这几种关键字进行的都是内连接，其中`INSERT JOIN ... ON`表示内连接，不过在SQL语句中可以进行省略

**外连接(OUTER JOIN)**

*左外连接*`LEFT OUTER JOIN ... ON`
```
SELECT a.id, b.name FROM a LEFT JOIN b ON a.id = b.id;
```
在实际敲代码的时候可以省略其中的`OUTER`，就像`INSERT`一样。

*右外连接*`RIGHT OUTER JOIN ... ON`
```
SELECT a.id, b.name FROM a RIGHT JOIN b ON a.id = b.id;
```

*满外连接*`FULL JOIN`
满外连接的结果 = 左右表匹配的数据 + 左表没有匹配的数据 + 右表没有匹配的数据
满外连接还可以使用UNION代替

**UNION和UNION ALL**
`UNION`和`UNION ALL`关键字都是合并查询结果，可以配合多条`SELECT`语句使用，将他们呢查询到的结果集合并到一起，两个表对应的列数和数据类型必须相同，并且相互对应。
不同点在于前者会去除重复项，后者不会去除重复项，两者之间的效率比较`UNION ALL`的开销明显小于`UNION`，因此当确定两个表中不会存在重复项是尽量使用`UNION ALL`
```
-- 使用UNION
SELECT * FROM A 
UNION
SELECT * FROM B;

-- 使用UNION ALL
SELECT * FROM A
UNION ALL
SELECT * FROM B;
```
**注意**
```
在进行多表查询中，可能会遇到不同表之间存在列名重复的现象，此时回抛出异常。在列名前加上所属的表即可解决
因此在进行多表查询中一定要在列名之前街上所属的表，所属的表可以有两种形式进行表达。一种是表的全名，一种的表的别名，表的别名在引入表的时候定义；
但是要注意的是：在多表查询的语句中，要么全部使用表的全名，要么全部使用表的别名。
```

#### SQL99新特性
**自然连接**
在SQL99中的自然连接相当于SQL92中的等值连接，通过`NATURAL JOIN`进行自然连接
```
-- 在SQL92中
SELECT a.id, b.name FROM a JOIN b ON a.id = b.id;

-- 在SQL99中可以写成
SELECT a.id, b.name FROM a NATURAL JOIN b
```
**UNSIG连接**
SQL99中能使用`UNSIG`指定数据表中的同名字段进行连接
```
FROM a.id, b.name FROM a JOIN b UNSIG (id);
```

### 多表查询总结
对于其中多种类型的连接：
* 内连接：求两个表的交集
* 左(右)外连接：求主表的所有条数和从表与主表的交集条数
* 满连接：求所有的记录条数包括交集和不相交的
* 求其他的集合可以在后面通过where进行限制<br>如求左外连接中主表中没有从表元素的记录：select * from a left join b on a.id = b.id where b.id = NULL;
* where适用于所有关联查询
* ON只能和JOIN一起使用
* UNSIG只能和JOIN一起使用

## 单行函数
在MySQL中字符串的下标从1开始

单行函数
* 操作数据对象
* 接收参数返回一个结果
* 只对一行进行变换
* 每行返回一个结果
* 可以嵌套
* 参数可以是一列或一个值

函数一般是数据库中的内嵌函数
包括数值函数、字符串函数、日期和时间函数、流程控制函数、加密解密函数、MySQL信息函数、其他函数

这些内置的库函数主打一个熟能生巧

## 聚合函数
聚合函数通常配合 Group By使用
聚合函数不能嵌套调用，如：AVG(SUM(字段))是不允许的

常用的聚合函数
SUM()对数值求和函数、COUNT()求记录条数、AVG()求平均数、MAX()求最大值、MIN()求最小值

GROUP BY可以将表中的数据分成若干组，但是GROUP BY要在WHERE后面使用，因为分组之前要先筛选过滤条件
GROUP BY还可以对多个列进行分组
在GROUP BY之后可以使用WITH ROLLUP，使用这个关键字之后再分组记录后增加一条记录，该记录查询出所有记录的总和，即统计记录数量，但是使用了WITH ROLLUP之后不能使用ORDER BY对结果进行排序

HAVING
HAVING是在使用GROUP BY分组之后对分组之后的集合进行过滤输出满足HAVING的条件

**HAVING与WHERE的区别**
having是先连接后筛选，where是先筛选后连接
但是having能使用分组中的计算函数，where不能使用分组中的计算函数。

## 查询语句的执行过程
语句结构：
SELECT ... FROM ... WHERE ... AND/OR ... GROUP BY ... HAVING ... ORDER BY ...  LIMIT...
或者
SELECT ... FROM ... JOIN ... ON ... WHERE ... AND/OR ... GROUP BY ... HAVING ... ORDER BY ... LIMIT
以上关键字的顺序不能颠倒

查询语句执行的顺序：
先执行from引入要查询的所有表，如果是多表查询会先连接表（通过内连接、或者外连接），处理完所有表之后得到一张逻辑上的新表

然后执行where进行筛选过滤，再次得到一张逻辑上的新表

然后执行group by对表进行分组，这里再次得到一张逻辑上的表，如果没有group by的话就不进行分组

然后执行having再次过滤，再次得到一张新表

然后执行select 中要显示的字段，这里再次得到一张新表

然后Order by进行排序，LIMIT进行分页

最后输出就是要查找的集合

## 子查询
子查询是指在查询语句的内部嵌套另一个查询语句
注意事项：
* 子查询要包含在括号内；
* 将子查询放在比较条件的右侧；
* 单行操作符对应单行子查询，多行操作符对应多行子查询；
* 单行子查询是指从查询到的结果返回的是一个值，且上一层的查询能够以这个结果作为筛选条件，得出下一个表。
* 多行子查询是指在子查询中返回多个记录条数，作为上一层的筛选条件，也可以看作是通过子查询得到一张新的表，从这个表里面使用多行操作符查找想要的记录
* 相关联子查询：如果子查询需要执行多次，即采用循环的方式，先从外部查询开始，每次都传入子查询进行查询，然后将结果反馈给外部，这种嵌套的执行方式称为相关子查询。
* 不相关联子查询：子查询中查询了数据结果，如果这个数据结果只查询了一次，然后这个数据结果作为主查询的条件进行执行，那么这样的子查询叫做不相关子查询。

在单行子查询中使用多行运算符会报错；

多行子查询的运算符要配合单行子查询使用：IN, ANY, ALL, SOME
例如，返回其他job_id，中比job_id为"IT_PORT"部门任一工资低的员工号、姓名、job_id、以及salary
```
select employee_id, last_name, job_id, salary
from employees
where salary < ANY
                    (select salary
                    from employees
                    where job_id = 'IT_PORT'
                    )
AND job_id != 'IT_PORT';
```
返回其他job_id中比job_id为'IT_PORT'部门所有工资都低的员工号、姓名、job_id以及salary
```
select employees_id, name, job_id, salary
from employees
where salary < ALL
                    (select salary
                    from employees
                    where job_id = 'IT_PORT'
                    )
AND job_id != 'IT_PORT';
```
对于单行子查询和多行子查询来说，都是不返回空值的

### 相关联子查询
在子查询中需要依赖外部表的情况就叫做相关联子查询

在from中，order by中都可以使用，具体的使用要结合实际的情况构造查询算法

EXISTS和NOT EXISTS关键字<br>
关联子查询中使用EXISIS操作符一起使用，用来检查子查询中是否存在满足条件的行<br>
* 如果子查询中不存在满足条件的行，条件返回false，继续在子查询中查找；
* 如果子查询中存在满足条件的行，不在子查询中查找，条件返回true

```
select id, name 
from emp e1
where exists 
(
    select * from emp e2 where man_id = e1.id    
);
```

### 总结
子查询除了用于查询语句还可以用于更新、删除、修改表结构

如果自连接和子查询中选，选自连接

因为自连接是通过已知的自身数据表进行条件判断，而子查询实际是通过对未知表查询后进行的条件判断，大部分DBMS都对自连接处理进行优化

## 创建表和管理表

数据库中的类型

### 创建表
```
create table [if not exists] 表名(
    字段1, 数据类型，[约束条件], [默认值]，
    字段2, 数据类型，[约束条件]，[默认值],
    ····
    [表约束条件]
);

-- 通过AS subquery方式，将创建表和插入数据结合起来

CREATE TABLE tableA
    [(cloumn, column ...)]
AS subquery;
指定列和子查询中的列要一一对应、通过列名和默认值定义列。
```
其中加上了if not exists关键字表示表不存在的时候创建表，如果表存在的话就不创建表。创建表的时候必须有**表名、字段名（列名）、数据类型、长度**, 可选条件是**约束条件、默认值**

字段约束条件：

表约束条件：

### 修改表
通过ALTER TABLE关键字对表进行修改数据库中已经存在的数据表的结构。包括向表中添加列、修改现有的列、删除表中的列、重命名现有的列；

* 追加一个列：ALTER TABLE dept800 ADD job_id varchar(15);
* 修改一个列的数据类型、长度、默认值、位置。修改默认值：ALTER TABLE 表名 MODIFY 字段 数据类型 defalut 默认值;
* 重命名一个列：ALTER TABLE 表名 CHANGE 字段名 新字段名 新数据类型;
* 删除一个列：ALTER TABLE 表名 DROP 字段名;
* 重命名表：```RENAME TABLE 表名 TO 新表名;``` 或者```ALTER TABLE 表名 RENAME TO 新表名；(TO可省略)```
* 删除表：当一张表和其他表没有形成关联关系的时候，可以将其删除。```DROP TABLE [IF EXISTS] 表1， 表2 ...;```注意：DROP TABLE不能回滚
* 清空表： TRUNCATE TABLE语句：删除表中所有的数据，释放表中存储的空间。 ```TRUNCATE TABLE 表名```；<br>
还可以使用delete语句进行删除，其中TRUNCATE TABLE语句不能回滚，DELETE可以回滚。尽管TRUNCATE比delet快，但还是建议优先使用delete；

### 对表的内容进行增删改
* 增：使用insert into语句对表进行添加，```insert into 表名 values (值， 值，值，)；```使用这种方法的话要求对values中的值要按照表定义的时候顺序进行赋值。或者可以使用```insert into 表名(字段1，字段2，字段3...) values (字段1的值，字段2的值，字段3的值...)```使用这种方式增加要求values中的值要与列出的字段名相同。
* 更新数据：```update 表名 set 字段1 = 字段1的值(...) where 筛选条件；```
* 删除数据：```delete from 表名 where 删除条件```,如果没有删除的条件的话那就是将表中的全部数据都删除。

新特性：计算列
在定义表的时候可以设置列c的值可以由列b的值和列a的值通过计算得到。

## 数据类型

**json**类型，josn类型是一种轻量级的 `数据交换格式`。简介和清晰的层次结构使得json成为理想的交换语言。JSON可以讲JavaScript对象中表示的一组数据转换为字符串，然后就可以在网络或者程序之间轻松的传递这个字符串，并在需要的时候将他还原为各编程语言所支持的数据格式。当检索JSON类型的字段中数据的某个具体值时，可以使用 `-> ` 和 `->>` 符号。  

## 约束

**NOT NULL**, 非空约束，作用于一列，添加not null约束之后当前的列的值不能为空；添加 `NOT NULL` 约束，创建表的时候在每一列的定义后面加上 `NOT NULL`即可，或者使用alter table修改表中列的约束。删除的NOT NULL约束，使用 `alter table` 修改表。

**unique**, 唯一约束，可以用于单列约束，也可以用于复合约束，添加单列唯一约束后该列的值不能出现相同的值，添加复合唯一约束之后，被约束的列值的组合不能出现相同。删除唯一约束只能通过删除唯一索引，即 `ALTER TABLE USER DROP index <索引名(添加唯一约束的时候的名字)>

**PRIMARY**, 主键约束，支持单列、复合约束，相当于唯一约束和非空约束的综合使用，可以在列级中添加primary key，也能在表级中添加。一个表不能出现两个主键。删除主键的操作通过  `alter table 表名 drop primary key。

**auto_increment**, 自增约束，一个表最多只有一个自增约束、只能在键列中使用（主键列、唯一键列）、设置自增的数据类型必须是整形，在建表时增加自增约束：在声明键列后面增加自增约束，建表后增加自增约束，通过修改表增加自增约束，删除自增约束：通过修改表语句，在语句中去掉自增约束即可。            

**check 约束**，用于检查某个字段是否符合 xx 要求，一般指的是值的范围，如在创建表的时候声明列: `sex char check('男' or '女')`表示sex列只能有 `男` 或者 `女` 的选项。

**default**，当插入数据时没有显式赋值，则赋值为默认值

### foreign key 外键约束
**foreign key**, 限定某个表的某个字段的引用完整性，**从表**是引用别人参数的那个表，**主表**是被别人引用参数的那个表
（1）从表的外键列必须使用主表中被主键或者唯一约束的键列
（2）创建外键约束的时候如果不给外键约束命名，系统的外键默认名不是列名，而是自动产生一个外键名，也可以指定外键约束名。
（3）创建表的时候约束外键的话先创建主表再创建从表
（4）删除表时，先删除从表或者先删除外键约束，再删除主表
（5）当主表中的记录被依赖的时候不能直接删除，要先删除从表中依赖该记录的数据再删除主表中对应的数据。
（6） 在从表中建立外键约束，一个表中可以存在多个外键约束
（7）从表中的外键列于主表中被依赖的键列名字可以不一样，但是数据类型和逻辑意义必须一样。
（8）当创建外键约束的时候，系统默认会在所在列上建立普通索引。但是索引名时外键的约束名
（9）删除外键约束必须手动删除索引

建表时，添加外键约束，在声明完数据类型之后：`[constraint <外键约束名>] foreign key (从表的某个字段) references 主表名(被参考字段)`

建表后，添加外键约束，`alter table 表名 add [constraint <外键约束名>] foreign key (从表的某个字段) references 主表名(被参考字段)`

**修改等级**
cascade(级联修改)：修改/删除主表被依赖记录的时候，从表匹配的记录会被一同修改/删除。
Set null:修改/删除主表中被依赖的记录，从表中匹配的记录会被一同置空；
No action:修改/删除主表中被依赖的记录，不允许修改
Restrict:同No action
Set default:主表有表更的时候，从表将外键列设置为一个默认的值，但Innodb不能被识别。

如果在增加外键约束的时候没有设置等级，相当于使用Restrict模式；

**对于外键约束最好使用 on update cascade on delete Restrict 的方式**，表示修改主表的时候级联修改主表，删除主表中依赖的时候不允许操作

**删除外键约束**，先通过 `information_schema.table_constraints` 查看约束名，删除外键约束，再查看索引名，手动删除索引；








