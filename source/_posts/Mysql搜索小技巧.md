---
title: Mysql搜索小技巧
date: 2020-07-14 16:39:24
tags:
- Mysql
---

##### 自定义排序：

```
order by FIELD(字段名,字符串1，字符串2) 来排序，用法  order by FIELD(status,'apply','success','delete')
```

<!--more-->

##### 格式化时间分组（时间戳）：

```
group by from_unixtime(create_time,'%Y-%m')
```

##### 分组写条件：

```
group by (reg_source <> 'offline' or reg_source is null)
```

##### 查询结果将NUll转为任意字符串：

```
coalesce(reg_source,'')
```

##### 查询结果做if逻辑判断：

```
select *,if(sva=1,"男","女") as ssva from taname where sva != ""
```
