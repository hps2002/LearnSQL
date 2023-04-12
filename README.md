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

* 自连接和非自连接
* * 自连接就是自己连接自己形成一个新的表，使用别名的方式虚拟成两张表，进行内连接、外连接进行查询
* * 非自连接就是将两个不同的表进行内连接或者外连接进行查询
* 内连接和外连接
内连接：合并具有同一列的两个以上的表的行，返回不包含一个表与另一个表不匹配的行；<br>
外连接：两个表在连接的过程中除了返回满足连接条件的行以外， 还返回左或者右表中不满足条件的行，不满足条件的行表中相应的列应该为空。<br>
左外连接：连接条件中左边的表为主表右边的表为从表<br>
右外连接：连接条件中右边的表为主表左边的表为从表<br>

<br>
在进行多表查询中，可能会遇到不同表之间存在列名重复的现象，此时回抛出异常。在列名前加上所属的表即可解决
因此在进行多表查询中一定要在列名之前街上所属的表，所属的表可以有两种形式进行表达。一种是表的全名，一种的表的别名，表的别名在引入表的时候定义；
但是要注意的是：在多表查询的语句中，要么全部使用表的全名，要么全部使用表的别名。