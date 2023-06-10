---
title: Mysql存储过程
date: 2021-08-11 17:00:46
tags:
- Mysql
---

用存储过程实现for循环执行sql语句
//默认情况下，DELIMITER是分号;。在MySQL中每行命令都是用“；”结尾，回车后自动执行，在存储过程中“；”往往不代表指令结束，马上运行，而DELIMITER原本就是“；”的意思，因此用这个命令转换一下“；”为“ ## ” ，这样只有收到“ ## ”才认为指令结束可以执行

<!--more-->

```
DELIMITER ##
#创建新的函数
CREATE PROCEDURE insertbatch()
BEGIN
	#定义变量 i
    DECLARE	i INT;
	#变量 i赋值 
	SET i = 0;
	WHILE i < 10000 DO
		#SQL语句
		SET i = i + 1;
	END WHILE;
END
##

#调用函数
CALL insertbatch() ##

#删除
DROP PROCEDURE insertbatch ##

#最后改回 DELIMITER分隔符默认的;
DELIMITER;
```
