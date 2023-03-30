# SQL语句：

group_concat([DISTINCT] 要连接的字段 [Order BY ASC/DESC 排序字段] [Separator '分隔符'])
通过group_concat将查询到的不同字段进行连接，并且按照排序字段从之排序输出，通过分隔符进行分割

conditions 条件构造，构造查询的条件，使用模糊查询