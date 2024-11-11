# Nexio API

## URL `/register`

该 API 用于用户注册。
该 API 仅接受以 POST 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### POST

使用 POST 方法请求该 API 即表示用户请求注册。

#### 请求体

请求体的格式为：

```json
{
    "userName": "Ashitemaru",
    "password": "123456",
    "phoneNumber": "123456",
    "emailAddress": "123456",
}
```

上述字段的说明为：

- `userName`。表示用户名，应当为非空字符串，且长度不大于 50
- `password`。表示用户的密码，应当为非空字符串，且长度不大于 20
- `phoneNumber`。表示用手机号码，可以为空。
- `emailAddress`。表示用户的邮箱，可以为空。

#### 成功响应

注册成功时，需要签发 JWT 令牌，设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed",
    "token": "***.***.***", // JWT
    "user": {
        "userId": "1",
        "userName": "xxx",
        "phoneNumber": "123",
        "emailAddress": "abc@123",
        "avatarUrl": "url"
    }
}
```

#### 错误响应

- 当在已有用户时使用同样的userName注册时，会将状态码设置为 402，错误响应格式为：

```json
{
    "code": 2,
    "info": "User already exists"
}

```

## URL `/login`

该 API 用于用户登录。
该 API 仅接受以 POST 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### POST

使用 POST 方法请求该 API 即表示用户请求登录。

#### 请求体

请求体的格式为：

```json
{
    "userName": "Ashitemaru",
    "password": "123456",
}
```

上述字段的说明为：

- `userName`。表示用户名，应当为非空字符串，且长度不大于 50
- `password`。表示用户的密码，应当为非空字符串，且长度不大于 20

#### 成功响应

登录成功时，需要签发 JWT 令牌，设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed",
    "token": "***.***.***", // JWT
    "user": {
        "userId": "1",
        "userName": "xxx",
        "phoneNumber": "123",
        "emailAddress": "abc@123",
        "avatarUrl": "url"
    }
}
```

#### 错误响应

- 当输入错误密码时，会将状态码设置为 401，错误响应格式为：

```json
{
    "code": 2,
    "info": "Wrong password"
}

```

- 当输入的用户名不存在时，会将状态码设置为 404，错误响应格式为：

```json
{
    "code": 3,
    "info": "User does not exist"
}

```

## URL `/unregister`

该 API 用于用户注销。
该 API 仅接受以 DELETE 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### DELETE

使用 DELETE 方法请求该 API 即表示请求注销指定的用户。

#### 请求头

使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

请求体的格式为：

```json
{
    "userId": "1",
    "password": "123456",
}
```

上述字段的说明为：

- `userId`。表示用id。
- `password`。表示用户的密码，应当为非空字符串，且长度不大于 20

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed"
}
```

#### 错误响应

- 当输入密码与jwt负载不匹配时，会将状态码设置为 403，错误响应格式为：

```json
{
    "code": 3,
    "info": "Cannot delete other users"
}

```

- 当输入的用户不存在时，会将状态码设置为 404，错误响应格式为：

```json
{
    "code": 4,
    "info": "User not found"
}

```

- 当jwt不匹配时，会将状态码设置为 401，错误响应格式为：

```json
{
    "code": 2,
    "info": "Invalid or expired JWT"
}

```

## URL `/logout`

该 API 用于用户登出。
该 API 仅接受以 DELETE 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### DELETE

使用 DELETE 方法请求该 API 即表示请求注销指定的用户。

#### 请求头

使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

请求体的格式为：

```json
{
    "userId": "Ashitemaru",
}
```

上述字段的说明为：

- `userId`。表示用户 id。

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed"
}
```

#### 错误响应

- 当输入密码与jwt负载不匹配时，会将状态码设置为 403，错误响应格式为：

```json
{
    "code": 3,
    "info": "Cannot logout other users"
}

```

- 当jwt不匹配时，会将状态码设置为 401，错误响应格式为：

```json
{
    "code": 2,
    "info": "Invalid or expired JWT"
}

```

## URL `/user/{userId}`

该 API 用于操作用户的 profile，包括获取 profile、修改 profile。

该 API 仅接受以 GET 与 PUT 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### GET

使用 GET 方法请求该 API 即表示请求 id 为 `userId` 的用户的 profile。

#### 请求头

使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

无。

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed",
    "user": {
        "userId": "1",
        "userName": "xxx",
        "phoneNumber": "123",
        "emailAddress": "abc@123",
        "avatarUrl": "url"
    }
}
```

#### 错误响应

- 当输入密码与jwt负载不匹配时，会将状态码设置为 403，错误响应格式为：

```json
{
    "code": 3,
    "info": "Cannot edit other users"
}

```

- 当jwt不匹配时，会将状态码设置为 401，错误响应格式为：

```json
{
    "code": 2,
    "info": "Invalid or expired JWT"
}

```

### PUT

使用 PUT 方法请求该 API 即表示修改 id 为 `userId` 的用户的 profile。

#### 请求头

使用 PUT 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

请求体的格式为：

```json
{  
    "oldPassword": "123456",
    "newPassword": "",
    "userName": "xxx",
    "phoneNumber": "123",
    "emailAddress": "abc@123",
    "avatarImage": "data:image/jpeg;base64,..."
}
```

上述字段的说明为：

- `oldpassword`。表示用户的旧密码，应当为非空字符串，且长度不大于 20。 `newPassword`。表示用户的新密码，应当为非空字符串，且长度不大于 20，只有在修改密码时需要验证原先的密码。
- `userName`，`phoneNumber`，`emailAddress`，`avatarImage`，都是可选的，新的 user profile。
- `avatarImage`。新的用户头像文件，将图片文件转换为 Base64 编码字符串，这样可以保证图片文件在传输过程中不会丢失信息。

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed",
    "user": {
        "userId": "1",
        "userName": "xxx",
        "phoneNumber": "123",
        "emailAddress": "abc@123",
        "avatarURL": "url"
    }
}
```

#### 错误响应

- 当输入密码与jwt负载不匹配时，会将状态码设置为 403，错误响应格式为：

```json
{
    "code": 3,
    "info": "Cannot edit other users"
}

```

- 当jwt不匹配时，会将状态码设置为 401，错误响应格式为：

```json
{
    "code": 2,
    "info": "Invalid or expired JWT"
}

```

- 当密码不匹配时，会将状态码设置为 401，错误响应格式为：

```json
{
    "code": 4,
    "info": "Missing or error password"
}

```

## URL `/users/search`

该 API 用于通过信息查询用户。

该 API 仅接受以 GET 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### GET

使用 GET 方法请求该 API 表示搜索指定的用户。

#### 请求头

使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
    "query": "xxx",
}
```

上述字段的说明为：

- `query`：搜索关键词（用户名、邮箱等）

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed",
    "user": {
        "userId": "1",
        "userName": "xxx",
        "phoneNumber": "123",
        "emailAddress": "abc@123",
        "avatarURL": "url"
    }
}
```



## URL `/friends/request`

该 API 用于用户申请添加好友。

该 API 仅接受以 POST 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### POST

使用 POST 方法请求该 API 表示搜索指定的用户。

#### 请求头

使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
    "targetUserId": "id",
    "message": "xxx",
}
```

上述字段的说明为：

- `targetUserId`：被申请为好友的用户 ID

- `message`（可选）：申请附带的消息

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  "code": 0,
  "info": "Succeed"
}
```

*表示信息发送成功，而非好友添加成功。*



## URL `/friends/request/{requestId}`

该 API 用于同意或拒绝好友申请。

该 API 仅接受以 PUT 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### PUT

使用 PUT 方法表示同意或拒绝指定 `requestId` 的好友请求。

#### 请求头

使用 PUT 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
    "action": "accept" | "reject)",
}
```

上述字段的说明为：

- `action`：接受 (`accept`) 或拒绝 (`reject`)

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  "code": 0,
  "info": "Succeed"
}
```



## URL `/friends/{friendId}`

该 API 用于用户删除指定好友。

该 API 仅接受以 DELETE 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### DELETE

#### 请求头

使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

无

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  "code": 0,
  "info": "Succeed"
}
```



## URL `/friends`

该 API 仅接受以 GET 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### GET

#### 请求头

使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

无

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  "code": 0,
  "info": "Succeed",
  "friends": [
    {
      "userId": "1",
      "userName": "Ashitemaru",
	  "phoneNumber": "123456",
      "emailAddress": "123456",
      "avatarUrl": "https://example.com/avatar/1.jpg",
      "group": "Family"
    },
    {
      "userId": "2",
      "userName": "JohnDoe",
      "phoneNumber": "123456",
      "emailAddress": "123456",
      "avatarUrl": "https://example.com/avatar/2.jpg",
      "group": "Work"
    }
  ]
}
```



## URL `/friends/{friendId}/group`

该 API 用于用户将好友分到特定的组。

该 API 仅接受以 PUT 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### PUT

#### 请求头

使用 PUT 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
	"group": "Family" | "Work"...,
}
```

上述字段的说明为：

- `group`：组名（例如 "Family" 或 "Work"）

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  "code": 0,
  "info": "Succeed",
}
```



