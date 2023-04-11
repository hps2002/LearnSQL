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
* IS NULL         为空运算符      select * from table where A is NULL;      
* IS NOT NULL     不为空运算符    select * from table where A IS NOT NULL;
* LEAST           最小值运算符    select * from table where C least(A, B);
* GREATEST        最大值运算符    select * from table where C greatest(A, B);
* BETWEEN AND     两值之间运算符  select * from table where C between A and B;//闭区间[A, B]
* ISNULL          为空运算符      select * from table where C isnull;
* IN              属于运算符      select * from table where A in (C, B);
* NOT IN          不属于运算符    select * from table where A not in (C, B);
* LIKE            模糊匹配        select * from table where A like B;
* REGEXP          正则表达式      select * from table where A regexp B;
* RLIKE           正则表达式      select * from table where A rlike B;

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