# 使用Redis应用构建web项目
> 使用Redis来管理用户登陆的对话(session)

## 登陆与cookie的缓存

  - cookie的分类
    * 签名 sign cookie  存储用户名或者用户id，或者最后一次登陆的时间，或者是登陆信息的签名，以防信息的篡改
    * 令牌 token cookie 是一种随机字符串作为用户的令牌，具有有效期，有效期过后会被新的token替代
  - 选择token cookie来存储登陆信息
## 
