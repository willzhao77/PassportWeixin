# Passport 授权码模式

This is server side. 

Client side is Bili.





>   配套视频地址：https://www.bilibili.com/video/av74879198?p=7

------

1. 哔哩哔哩提供一个“微信登陆”的链接，用户点击跳转到微信授权服务器。
2. 用户根据微信授权服务器提示登陆微信并确认授权给哔哩哔哩。
3. 微信授权服务器返回用户代理（浏览器）一个授权码。
4. 用户代理（浏览器）把这个授权码传给哔哩哔哩。
5. 哔哩哔哩凭借授权码向微信授权服务器请求令牌。
6. 微信授权服务器发送令牌给哔哩哔哩。

#### **服务器端（微信）**

###### **配置**

```php
composer create-project --prefer-dist laravel/laravel laravel6
.env 数据库配置
修改数据库默认字符串长度
composer require laravel/passport

```



```php
app\User.php
    
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Laravel\Passport\HasApiTokens; //Trait 添加到 App\User 模型中 // 提供一些辅助函数检查已认证用户的令牌和使用范围

class User extends Authenticatable
{
    use HasApiTokens, Notifiable;
```





安装前端必备的东西（脚手架）

```php
下载 node   https://nodejs.org/en/
composer require laravel/ui
php artisan ui vue --auth
npm install cnpm -g --registry=https://registry.npm.taobao.org
cnpm install
cnpm run prod
composer require guzzlehttp/guzzle // 伪造 http 请求
// config/auth.php

'api' => [
    'driver' => 'passport',
    'provider' => 'users',
    'hash' => false,
],

php artisan migrate // 创建表来存储客户端和 access_token 等
php artisan passport:keys // 加密生成的 access_token
   
// 注册路由 AuthServiceProvider
use Laravel\Passport\Passport;
     
    public function boot()
    {
        $this->registerPolicies();
        Passport::routes();
        Passport::tokensExpireIn(now()->addDays(15)); // access_token 过期时间
        Passport::refreshTokensExpireIn(now()->addDays(60)); // refresh_token 过期时间
    }
```

###### **创建客户端**

```php
php artisan passport:client
//php 7.4.0 has problem. Received error:  Abort.
//downgrade to 7.3.12   on system environment Variables
```

**Config Sample as below:**

```cmd
Which user ID should the client be assigned to?:

 >


 What should we name the client?:

 > bili

 Where should we redirect the request after authorization? [http://localhost/auth/callback]:

 > http://bili.com/auth/callback

New client created successfully.
Client ID: 1
Client secret: YuX9yuCn38Dl0sJWSFw7VVacInYz8KjRuW5iug8L
```







