---
title: PHP导出Excel提高性能的办法
date: 2020-07-14 16:40:57
tags:
- PHP
- Excel
---

phpExcel的性能很差，占用内存，用这个一般都会限制导出行数，如果不过多的考虑样式之类，可以使用t的方式导出excel，

举个例子

<!--more-->

```
<?php
header("Content-type:application/vnd.ms-excel");
header("Content-Disposition:attachment;filename=test_data.xls");
//输出内容如下：

echo   "姓名"."\t";
echo   "年龄"."\t";
echo   "学历"."\t";
echo   "\n";
echo   "张三"."\t";
echo   "25"."\t";
echo   "本科"."\t";

?>
```

我们公司现在大部分导出excel数据都用t,性能完爆phpExcel，题主可以去测试一下性能比
