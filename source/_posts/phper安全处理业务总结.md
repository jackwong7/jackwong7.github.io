---
title: phper安全处理业务总结
date: 2021-08-19 17:01:51
tags:
- PHP
- Laravel
- Mysql
---

### curl处理

1. api请求要自己做好幂等性，做好超时处理，异常处理

   <!--more-->

### mysql相关

1. 操作Builder类的update方法一定要判断受影响行数（int类型），禁止全等判断是否是 true或者false，操作Model类的update/save方法也要判断结果（bool）

   2.操作update方法一定要做乐观锁，例如 update order set status=20 where status = 10 and id =1;

   3.涉及到资产相关接口，redis要设置大于 php-fpm最大执行时间的锁（>300s）, 如果api和admin同时操作同一个资产要注意redis锁前缀是否不一样

   4.针对可能出现重复数据的，db设计要做唯一索引

   5.所有的业务都需要考虑 db 响应过慢的问题，比如60s以上的响应速度，业务会不会出现问题



注意事项：操作api划转禁止参与事务内。划出资产一定要确保资产扣除成功扣再去操作api划出。操作用户资产的sql一定一定要注意，操作需要用mysql递增递减实现。扣除资产一定要做金额判断。

```
#例如：
update user_asset_1 set amount = amount – "100.00000"
where user_id = 1 and asset_code = "USDT" and amount >= "100.00000"
```

如果decimal整数位+小数位超过16位（不含16），不能直接操作加减法（因为php带过去的都是字符串），需要修改一下sql

例如：

```
update user_asset_1 set amount = amount – cast('0.000000000000000001' as decimal(40,18)) 
where user_id = 1 and asset_code = "USDT" and amount >= '0.000000000000000001';
```

### laravel常见db操作返回值：

```
更新数据区分：

model类：
$user = User::where('id',1)->first();
$user->status = 20;
$user->update(); //返回bool 重复执行，也一直返回 true
//执行语句 update `user` set `status` = ?, `user`.`updated_at` = ? where `id` = ?

$user->save(); //返回bool 重复执行，也一直返回 true
//执行语句 update `user` set `status` = ?, `user`.`updated_at` = ? where `id` = ?


builder类：
$user = User::where('id',1)->first();
$user->lockForUpdate()->update(['status'=>20]); //返回int，更新全表,并且每次都能成功
//执行语句 update `user` set `status` = ?, `user`.`updated_at` = ?

DB::table('user')->where('id',1)->update(['status'=>20]); //返回int
//执行语句 update `user` set `status` = ? where `id` = ?

User::where('id',1)->update(['status'=>20]); //返回int，每次都能成功
//执行语句 update `user` set `status` = ?, `user`.`updated_at` = ? where `id` = ?


插入数据区分：
create，forceCreate 返回对象
insert,save 返回bool
insertGetId 返回int
```
