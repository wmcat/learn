# Jwt

## Jwt结构
---
> Header（头部）<br/>
Payload（负载）<br/>
Signature（签名）<br/>

### Header
```
// Header 部分是一个 JSON 对象，描述 JWT 的元数据，通常是下面的样子。
{
  "alg": "HS256", （表示签名的算法, algorithm）
  "typ": "JWT"
}
```
### Payload
Payload 部分也是一个 JSON 对象，用来存放实际需要传递的数据
```
    // 官方字段
    iss (issuer)：签发人
    exp (expiration time)：过期时间
    sub (subject)：主题
    aud (audience)：受众
    nbf (Not Before)：生效时间
    iat (Issued At)：签发时间
    jti (JWT ID)：编号
```
###  Signature
Signature 部分是对前两部分的签名，防止数据篡改
需要指定一个密钥（secret）。这个密钥只有服务器才知道，不能泄露给用户。然后，使用 Header 里面指定的签名算法（默认是 HMAC SHA256），按照下面的公式产生签名
```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```
> 将这三部分用.连接成一个完整的字符串,构成了最终的jwt:

## jwt的使用方式
客户端每次与服务器通信，都要带上这个 JWT。你可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求的头信息Authorization字段里面。
```
Authorization: Bearer <token>
```

## 缺点
占带宽
正常情况下要比 session_id 更大，需要消耗更多流量，挤占更多带宽，假如你的网站每月有 10 万次的浏览器，就意味着要多开销几十兆的流量。听起来并不多，但日积月累也是不小一笔开销。实际上，许多人会在 JWT 中存储的信息会更多
无法在服务端注销，那么久很难解决劫持问题
JWT 的卖点之一就是加密签名，由于这个特性，接收方得以验证 JWT 是否有效且被信任。但是大多数 Web 身份认证应用中，JWT 都会被存储到 Cookie 中，这就是说你有了两个层面的签名。听着似乎很牛逼，但是没有任何优势，为此，你需要花费两倍的 CPU 开销来验证签名。对于有着严格性能要求的 Web 应用，这并不理想，尤其对于单线程环境。

## koa中使用
生成公钥私钥
```
genrsa -out private.key 1024
rsa -in private.key -pubout -out public.key
```
```
    // 生成 
    const jwt = require('jsonwebtoken')
    const token = jwt.sign({ ...ctx.user }, PRIVATE_KEY, {
        expiresIn: 60 * 60 * 24 * 2,
        algorithm: "RS256"
    })
    // 获取
    const authorization = ctx.headers.authorization
    const token = authorization.replace("Bearer ", "")
    const result = jwt.verify(token, PUBLIC_KEY, {
        algorithms: ["RS256"]
    })
    ctx.user = result

```
http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html