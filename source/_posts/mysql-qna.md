---
title: mysql_qna
date: 2016-05-26 16:07:45
tags: mysql
---


## sql_mode
### error

```
mysql> SELECT `id`, GROUP_CONCAT(`user`) AS `user`, SUM(`num`) AS `num` FROM `abc` AS `abc` WHERE `abc`.`status` = '0' GROUP BY id;
ERROR 1055 (42000): Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'table1.abc.id' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
```

### solution

```
set global sql_mode='';
```
