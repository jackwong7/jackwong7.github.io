---
title: Laravel Api架构下,如何隐藏用户id
date: 2020-07-14 16:49:41
tags:
- Laravel
- PHP
---

本文以 tymon/jwt-auth 和 Composer包为例



#### 首先在 用户模型 修改这个方法

```
public function getJWTIdentifier()
{
  #加密主键ID
  return Hashids::encode($this->getKey());
}
```

<!--more-->

#### 然后自己实现 用户的 Provider Driver

```
<?php
namespace App\Providers;
use App\Models\User;
use Illuminate\Auth\AuthenticationException;
use Illuminate\Auth\EloquentUserProvider;
use Vinkla\Hashids\Facades\Hashids;
class JWTCustomUserProvider extends EloquentUserProvider
{
  /**
   * 当获取用户的时候解密 user_id
   *
   * 下面这些方法是为了加密和解密 jwt 中的 user_id
   * @see \App\Extensions\Illuminate\Auth\JWTCustomUserProvider::retrieveById()
   * @see \App\User::findForPassport()
   * @see \App\OauthAccessToken::setUserIdAttribute()
   *
   * @param int|string $identifier
   * @return User
   * @throws AuthenticationException
   */
  public function retrieveById($identifier)
  {
      $model = $this->createModel();

      /**
       * If Id is a string, then we need to decrypt $identifier.
       *
       * @see \App\Models\User::findForPassport()
       */
      if (!is_numeric($identifier)) {
          #解密之前在User类加密的id
          $identifier = current(Hashids::decode($identifier));
      }
  
      return $model->newQuery()
          ->where($model->getAuthIdentifierName(), $identifier)
          ->first();
  }
}
```

注册 上面写的 Provider

#### 在 app/Providers/AuthServiceProvider.php 文件 boot 方法中添加:

use Auth;

```
/**
 * Register @see \App\Extensions\Illuminate\Auth\JWTCustomUserProvider
 */
Auth::provider('jwtcustom', function ($app, $config) {
    $model = $config['model'];
    return new JWTCustomUserProvider($app['hash'], $model);
});
```

修改 config/auth.php 里面对应 Providers,将 driver改成 上面写的 jwtcustom
