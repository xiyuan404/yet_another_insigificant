# Building the Internet's missing identity layer


WebAuthn and FIDO 2 - enabling passwordless login
OpenID Connect
JSON Web Token (JWT) and its underpinnings
OAuth 2.0 specifications
OpenID Certification Program - enabling an interoperable identity ecosystem


OpenID Connect
OAuth 2.0
JSON Web Token (JWT)
Identity Federations
Passwordless Login


认证: who you are
授权: what you can do
  - 基于资源控制ACL
  - 基于角色控制RBAC
```sql
select per_desc, per_url
from permisson
where id IN (
  select per_id
  from role_perm
  where role_id IN (
    select role_id
    from user_role
    where user_id = 1
  )
)
```
权限：accessbleAPI
## 账号认证
手机号、邮箱
学号、工号、身份证号



## 第三方认证
3-party Social Connections

第三方登录
单点登录

JSON WEB TOKENS in Detail
```js
const encodedHeader = base64(JSON.stringify(header))
const encodedPayload = base64(JSON.stringify(header))
const signature = base64(hmac([encodedHeader,encodedPayload].join('.'),secret, sha256))
const jwt = [encodedHeader,encodedPayload,signature].join('.')
```
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9. <- header
eyJpdGVtcyI6WzAsMiw0XSwiaWF0IjoxNDkzMTM5NjU5LCJleHAiOjE0OTMxNDMyNTl9. <-playload
932ZxtZzy1qhLXs932hd04J58Ihbg5_g_rIrj-Z16Ja <- signature
```

```js
// prefixed by /protected
app.use('/protected', expressJwt({
  secret:
  audience: process.enve.AUTHO_API_ISSUER
  issuer: process.env.AUTHO_API_ISSUER
  requestProperty:
  getToken: req => {
    return req.cookies['access_token']
}
}))

// validate the access (authorization) to the API
// validate the integrity of the client side data
```



JSON Web Signatures

JSON Web Encryption

JSON Web Keys

JSON Web Algorithm
