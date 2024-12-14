# HTTP API

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
    "userName": "<string>",
    "password": "<string>",
    "phoneNumber": "<string>",
    "emailAddress": "<string>"
}
```

上述字段的说明为：

- `userName`。表示用户名，应当为非空字符串，且长度不大于 50
- `password`。表示用户的密码，应当为非空字符串，且长度不大于 20
- `emailAddress`。表示用户的邮箱，可以为空

> 后续添加邮箱验证时，`emailAddress` 将会改为不能为空

- `phoneNumber`。表示用手机号码，可以为空

#### 成功响应

注册成功时，需要签发 JWT 令牌，设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed",
    "token": "***.***.***", // JWT
    "user": {
        "userId": "<string>",
        "userName": "<string>",
        "phoneNumber": "<string>",
        "emailAddress": "<string>",
        "avatarUrl": "<string>"
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
    "userName": "<string>",
    "password":	"<string>",
}
```

上述字段的说明为：

- `userName`。表示用户名，应当为非空字符串，且长度不大于 50

> 后续添加邮箱验证后，可以使用 `userName` 或者 `emailAddress` 登录

- `password`。表示用户的密码，应当为非空字符串，且长度不大于 20

#### 成功响应

登录成功时，需要签发 JWT 令牌，设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed",
    "token": "***.***.***", // JWT
    "user": {
        "userId": "<string>",
        "userName": "<string>",
        "phoneNumber": "<string>",
        "emailAddress": "<string>",
        "avatarUrl": "<string>",
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
    "userId": "<string>",
    "password": "<string>",
}
```

上述字段的说明为：

- `userId`：表示请求注销的用户 id
- `password`：表示用户输入的密码，用来验证是否为本人注销

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
    "userId": "<string>",
}
```

上述字段的说明为：

- `userId`：请求登出的用户的 id

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed"
}
```

#### 错误响应

- 当jwt负载不匹配时，会将状态码设置为 403，错误响应格式为：

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
        "userId": "<string>",
        "userName": "<string>",
        "phoneNumber": "<string>",
        "emailAddress": "<string>",
        "avatarUrl": "<string>"
    }
}
```

#### 错误响应

- 当jwt负载不匹配时，会将状态码设置为 403，错误响应格式为：

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

使用 PUT 方法请求该 API 即表示修改 id 为 `userId` 的用户的 profile

#### 请求头

使用 PUT 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

请求体的格式为：

```json
{  
    "oldPassword": "123456",
    "newPassword": "654321",
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
    "token": "***.***.***", // JWT
    "user": {
        "userId": "user.id",
        "userName": "xxx",
        "phoneNumber": "123",
        "emailAddress": "abc@123",
        "avatarURL": "url"
    }
}
```

#### 错误响应

- 当jwt负载不匹配时，会将状态码设置为 403，错误响应格式为：

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

- 验证 newPassword（新密码）是否符合长度要求
```json
{
    "code": 7,
    "info": "newPassword must be a non-empty string with a max length of 20"
}
```

- 验证 oldPassword（旧密码）是否符合长度要求
```json
{
    "code": 8,
    "info": "oldPassword must be a non-empty string with a max length of 20."
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

#### 请求参数

```json
{
    "searchText": "xxx",
}
```

上述字段的说明为：

- `searchText`：搜索关键词（用户名、邮箱等）

#### 请求体

无

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed",
    "users": [
    {
      	"userId": "<string>",
     	"userName": "<string>",
	  	"phoneNumber": "<string>",
      	"emailAddress": "<string>",
      	"avatarUrl": "<string>"
    }
        ...
  ]
}
```
#### 失败响应

当请求方法不是 GET 时，返回如下响应：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

当没有jwt令牌时，返回如下响应：

```json
{
    "code": -2,
    "info": "Authorization token missing"
}
```

当搜索的关键词没有找到任何匹配的用户时，返回如下响应：

```json
{
    "code": 1,
    "info": "No users found"
}
```

当请求中没有提供 searchText 字段时，返回如下响应：

```json
{
    "code": -4,
    "info": "Missing searchText parameter"
}
```


## URL `/users/batch`

该 API 用于通过 userId 批量查询用户。

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
    "userIds": ["<string>", "<string>", ...];
}
```

上述字段的说明为：

- `userIds`：要查询的 User 的 id

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
    "code": 0,
    "info": "Succeed",
    "users": [
    {
      	"userId": "<string>",
     	"userName": "<string>",
	  	"phoneNumber": "<string>",
      	"emailAddress": "<string>",
      	"avatarUrl": "<string>"
    }
        ...
  ]
}
```
#### 失败响应


当请求方法不是 POST 时，返回此错误(405):
```json
{
    "code": -3,
    "info": "Bad method"
}
```


如果 JWT 令牌无效或缺失(403):
```json
{
    "code": -4,
    "info": "缺失令牌"
}
```

请求体格式错误（400）:
```json
{
    "code": -3,
    "info": "请求体格式错误"
}
```

如果没有传递 userIds（400）：
```json
{
    "code": -3,
    "info": "缺失 [userIds] 参数"
}
```


## URL `/friends/groups`

该 API 用于创建、删除和更改好友分组

该 API 仅接受以 POST 和 PATCH 和DELETE方法的请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### POST

使用 POST 方法新建一个空分组。（即，当前的组刚刚创建，还没有添加用户）

#### 请求头

使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
    "userId": "<string>",
	"groupName": "<string>",
}
```

上述字段的说明为：

- `userId`：分组创建者的用户 id，用于标识是谁创建的分组。
- `groupName`：组名，可以为任意类型的字符串，但是不可以和已有的组重复

> **注：**默认的组名是"My Friends"，见 appendix
>
> 组名不可以重复，因此，用户不应该添加已经添加过的组名

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
    "friendGroup": 
    {
        groupId: "<string>"
       	groupName: "My Friends",
       	friends: [
        {
      		"userId": "<string>",
     		"userName": "<string>",
	  		"phoneNumber": "<string>",
    		"emailAddress": "<string>",
    		"avatarUrl": "<string>",
    	},
    	{
     		"userId": "<string>",
      		"userName": "<string>",
      		"phoneNumber": "<string>",
      		"emailAddress": "<string>",
      		"avatarUrl": "<string>",
    	}
       	]
   	}
}
```

> 返回新创建好的 friendGroup，注意新加了groupId

#### 失败响应

当没有jwt令牌时，设置状态码403(之后出错的状态码统一403)，格式：

```json
{
  "code": -4,
  "info": "缺失令牌",
}
```

当创建者不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "用户不存在或已注销",
}
```

当要创建的组已存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "组已存在",
}
```

### DELETE

使用 DELETE 方法删除一个分组。

#### 请求头

使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
    "userId": "<string>",
	"groupName": "<string>",
}
```

上述字段的说明为：

- `userId`：分组创建者的用户 id，用于标识是谁创建的分组。
- `groupName`：需要删除的组名，可以为任意类型的字符串，但是不可以是 "My Friends"

> **注：**默认的组名是"My Friends"，见 appendix
>
> "My Friends" 不可以删除，可能会删掉所有的组
>
> 删除后，将原先组中所有的用户移动到 "My Friends" 中，前端会同步做这个操作

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
	"info": "Succeed",
	"friendGroups": [
        {
            groupId: "<string>",
           	groupName: "My Friends",
        	friends: [
      	       {
      				"userId": "<string>",
     				"userName": "<string>",
	  				"phoneNumber": "<string>",
    				"emailAddress": "<string>",
      				"avatarUrl": "<string>",
    			},
    			{
     				"userId": "<string>",
      				"userName": "<string>",
      				"phoneNumber": "<string>",
      				"emailAddress": "<string>",
      				"avatarUrl": "<string>",
    			}
            ]
        },
        {
            groupId: "<string>",
            groupName: "<string>",
            friends: [
                ...
            ]
        }
    ]
}
```

#### 失败响应

当没有jwt令牌时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "缺失令牌",
}
```

当用户不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "用户不存在或已注销",
}
```

当要删除的组不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "要删除的组不存在",
}
```

当试图删除默认组"My Friends"时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "不能删除默认分组",
}
```

### PATCH

使用 PATCH 更改好友的分组。即，将好友从一个组移动到另一个组。

#### 请求头

使用 PATCH 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
    "userId": "<string>",
    "friendUserId": "<string>",
	"fromGroupName": "<string>",
    "toGroupName": "<string>"
}
```

 上述字段的说明为：

-  `userId`：用户自己的 id

-  `friendUserId`：要移动的好友 id

- `fromGroupName`：原先的组名，必须是现有的组名
- `toGroupName`：新的组名，必须是现有的组名

> 需要处理：
>
> （1）组名是否合法
>
> （2）用户是否存在
>
> （3）用户是否是好友

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
     "friendGroups": [
        {
            groupId: "<string>",
           	groupName: "My Friends",
        	friends: [
      	       {
      				"userId": "<string>",
     				"userName": "<string>",
	  				"phoneNumber": "<string>",
    				"emailAddress": "<string>",
      				"avatarUrl": "<string>",
    			},
    			{
     				"userId": "<string>",
      				"userName": "<string>",
      				"phoneNumber": "<string>",
      				"emailAddress": "<string>",
      				"avatarUrl": "<string>",
    			}
            ]
        },
        {
            groupId: "<string>",
            groupName: "<string>",
            friends: [
                ...
            ]
        }
    ]
}
```

> 返回改动后的组，或者返回所有的组，都行。只要"friendGroups"这包含那两个被改动的组，前端就可以出处理

#### 失败响应

当没有jwt令牌时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "缺失令牌",
}
```

当用户不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "用户不存在或已注销",
}
```

当好友不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "好友不存在或已注销",
}
```
当原组不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "原组不存在",
}
```

当新组不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "新组不存在",
}
```

当输入的两个用户并非好友关系时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "非好友关系",
}
```

当好友不在原组中时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "好友不在原组中",
}
```



## URL `/friends`

该 API 用于获取好友列表（支持分组），和删除好友

该 API 接受以 GET 和 DELETE 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### GET

#### 请求头

使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求参数

```json
{
    "userId": "string"
}
```

上述字段的说明为：

-  `userId`：用户本人的 userId

> 即，查询这个用户的所有好友

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
    "friendGroups": [
        {
            groupId: "<string>",
           	groupName: "My Friends",
        	friends: [
      	       {
      				"userId": "<string>",
     				"userName": "<string>",
	  				"phoneNumber": "<string>",
    				"emailAddress": "<string>",
      				"avatarUrl": "<string>",
    			},
    			{
     				"userId": "<string>",
      				"userName": "<string>",
      				"phoneNumber": "<string>",
      				"emailAddress": "<string>",
      				"avatarUrl": "<string>",
    			}
            ]
        },
        {
            groupId: "<string>",
            groupName: "<string>",
            friends: [
                ...
            ]
        }
    ]
}
```

>注意参考 `FriendGroups` 实现这个接口：
>
>```typescript
>export type User = {
>    userId: string;
>    userName: string;
>    phoneNumber: string;
>    emailAddress: string;
>    avatarUrl: string;
>};
>
>export type Friends = User[];
>
>export type FriendGroup = {
>    groupName: string;
>    friends: Friends;
>};
>
>export type FriendGroups = FriendGroup[];
>
>export const DEFAULT_GROUP_NAME = "My Friends";
>
>export const initialFriendGroups: FriendGroups = [
>    {
>        groupName: DEFAULT_GROUP_NAME,
>        friends: []
>    }
>];
>```
>
>数据结构比较复杂，要小心实现

#### 失败响应

当没有jwt令牌时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "缺失令牌",
}
```

当用户不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "用户不存在或已注销",
}
```

### DELETE

#### 请求头

使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
    "userId": "<string>"，
    ”friendId“: "<string>"
}
```

上述字段的说明为：

-  `userId`：用户本人的 userId
-  `friendId`：要删除的好友 userId

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

#### 失败响应

当没有jwt令牌时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "缺失令牌",
}
```

当用户不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "用户不存在或已注销",
}
```

当好友关系不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "好友关系不存在",
}
```

## URL `/friends/requests`

该 API 用于处理好友请求。

该 API 接受以 GET，POST，PATCH 方法请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### GET

GET 方法用来查看用户发送和接收到的所有好友请求。

#### 请求头

使用 GET方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求参数

```json
{
  	"userId": "<string>",
}
```

上述字段的说明为：

-  `userId`：查询者的 userId

#### 请求体

无

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
    "sentRequests": [
    	{
            requestId: "<string>",
    		createdAt: "<string>",
    		fromUserId: "<string>",
    		toUserId: "<string>",
    		status: "<FriendRequestStatus>"
        }
        ...
    ],
    "receivedRequests": [
    	{
            requestId: "<string>",
    		createdAt: "<string>",
    		fromUserId: "<string>",
    		toUserId: "<string>",
    		status: "<FriendRequestStatus>"
        }
        ...
    ]
    
}
```

>参考 `FriendRequest` 的定义：
>
>```typescript
>export enum FriendRequestStatus {
>    Pending = 'Pending', // 发送中
>    Accepted = 'Accepted', // 同意请求
>    Rejected = 'Rejected', // 拒绝请求
>    Canceled = 'Canceled', // 撤回请求
>    Failed = 'Failed' // 服务器错误
>}
>
>export type FriendRequest = {
>    requestId: string;
>    createdAt: string;
>    fromUserId: string;
>    toUserId: string;
>    status: FriendRequestStatus;
>};
>```
>
>返回两个 FriendRequest 的列表，一个是该用户发出的"sentRequests"，另一个是该用户接收到的"receivedRequests"

#### 失败响应

当没有jwt令牌时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "缺失令牌",
}
```

当用户不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "用户不存在或已注销",
}
```

### POST

POST 方法用来发送一个新的好友请求。

#### 请求头

使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
  	"fromUserId": "<string>",
  	"toUserId": "<string>"
}
```

上述字段的说明为：

-  `fromUserId`：发送者的 userId
-  `toUserId`：接收者的 userId

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
    "friendRequest": {
    	requestId: "<string>",
    	createdAt: "<string>",
    	fromUserId: "<string>",
    	toUserId: "<string>",
    	status: "<FriendRequestStatus>"
	}
}
```

#### 失败响应

当没有jwt令牌时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "缺失令牌",
}
```

当用户不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "用户不存在或已注销",
}
```

当用户试图添加自己为好友时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "不能添加自己为好友",
}
```

当要添加的用户已经是用户的好友时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "已经是好友",
}
```

当已经存在待处理的请求，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "已经发送过好友请求，请勿重复发送",
}
```

### PATCH

PATCH 方法用来更新一个好友请求的状态。

#### 请求头

使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
    "friendRequest": {
    	requestId: "<string>",
    	createdAt: "<string>",
    	fromUserId: "<string>",
    	toUserId: "<string>",
    	status: "<FriendRequestStatus>"
	}
}
```

上述字段的说明为：

-  `FriendRequest`：要更新的 FriendRequest

> 后端注意判断，该 FriendRequest 是否合法

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
    "friendRequest": {
    	requestId: "<string>",
    	createdAt: "<string>",
    	fromUserId: "<string>",
    	toUserId: "<string>",
    	status: "<FriendRequestStatus>"
	}
}
```

#### 失败响应

当没有jwt令牌时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "缺失令牌",
}
```

当好友请求不存在时，设置状态码403，格式：

```json
{
  "code": -4,
  "info": "好友请求不存在",
}
```


## GET `/chats`

该 API 用于用户获取与自己关联的所有聊天（包括单聊和群聊）

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求参数

```json
{
     "fromUserId": "<string>",
}
```

- `fromUserId`：查询者的 userId

### 请求体

无

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  "code": 0,
  "info": "Succeed",
  "chats": [
    {
      "chatId": "string",
      "createdAt": "datetime",
      "chatType": "Private|Group",
      "chatName": "string",
      "chatAvatarUrl": "string",
      "chatSettings": {
        "isMuted": false,
        "isPinned": false
      },
      "messageListId": "string",
      "participantListId": "string",
      "notificationListId": "string",
      "joinRequestListId": "string"
    }
  ]
}

```

> Chats 见前端 definition 中 Chat 的定义，是一个 Chat 的列表

### 错误响应

- 当使用不被允许的方法，设置状态码405，格式：
```json
{
    "code": -3,
    "info": "Bad method"
}
```
- 缺失或无效的令牌
```json
{
  "code": -4,
  "info": "Missing or invalid token",
  "status": 403
}
```
- 缺失或无效的‘fromUserId’参数
```json
{
  "code": -2,
  "info": "Missing or invalid [fromUserId]",
  "status": 400
}
```
- 未找到用户
```json
{
  "code": -5,
  "info": "User not found",
  "status": 404
}
```

## POST `/chats`

该 API 用于添加一个新的聊天，或者同步前端没有获取到的聊天。

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求体

```json
{
    "fromUserId": "<string>",
  	"chatType": "Private|Group",
  	"participantIds": ["<string>", "<string>", ...]
}
```

- `fromUserId`：添加者的 userId
- `chatType`：聊天类型
- `participantIds`：参与者的用户 id （应该包含 `fromUserId`，不包含认定为不合法）

> 注意可能有两种情况：
>
> - 第一种情况是聊天确实是新添加的
>
> - 第二种情况是由于某种原因，前端没有来得及在数据库中同步这个聊天，误认为这个聊天不存在。后端此时应先返回已经建立好的 Chat 对象
>
> 如果是新添加的聊天，后端需要自行设置 Chat  ChatName ChatAvatarUrl
>

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
  	"chat": "<Chat>"
}
```
### 错误响应
- 未找到用户
```json
{
  "code": -4,
  "info": "用户不存在",
  "status": 403
}
```
- 无效的json格式
```json
{
  "code": -1,
  "info": "Invalid JSON format",
  "status": 400
}
```
- 缺少或错误类型的 fromUserId
```json
{
  "code": -2,
  "info": "Missing or error type of [fromUserId]",
  "status": 400
}
```
- 群组用户中不存在添加者
```json
{
  "code": -4,
  "info": "群组用户中不存在添加者",
  "status": 403
}
```
- 群组用户注销或不存在
```json
{
  "code": -4,
  "info": "群组用户 {id} 注销或不存在",
  "status": 403
}
```

> 返回后端创建好的 Chat


## GET `/chats/search`

该 API 用于搜索聊天

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求参数

```json
{
    "searchText": "xxx",
}
```

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  "code": 0,
  "info": "Succeed",
  "chats": [
    {
      "chatId": "string",
      "createdAt": "datetime",
      "chatType": "Private|Group",
      "chatName": "string",
      "chatAvatarUrl": "string",
      "chatSettings": {
        "isMuted": false,
        "isPinned": false
      },
      "messageListId": "string",
      "participantListId": "string",
      "notificationListId": "string",
      "joinRequestListId": "string"
    }
  ]
}
```




## PATCH `/chats/{chatId}`

该 API 用于用户更改某一个聊天的信息

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求体

```json
{
    "fromUserId": "<string>",
    "chat": "<Chat>"
}
```

> 只有群聊可以修改名称和头像，单聊的名称和头像应该由后端固定给出（名称是聊天对象的名称，头像也是对方的头像。即，给 A 发 B 的名称和头像，给 B 发 A 的名称和头像）。
>
> 注意返回错误情况：
>
> - 修改了并非自己参与的聊天
> - 修改了单聊的名称和头像
> - 其他可能的错误
>
> `ChatSettings` 类型的定义见前端的 definition.ts。definition 可能随后续的实现有改变，注意看后端仓库实时更新

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
  	"chat": "<Chat>"
}

```

> 返回的 Chat 对象是后端修改后的对象，即后端修改数据库，再将数据库中的 Chat 对象读出来，返回给前端（不要直接修改程序中的数据返回给前端）
>
> Chat 对象的定义见前端 definition.ts1

### 错误响应
- 未找到用户
```json
{
  "code": -4,
  "info": "用户不存在",
  "status": 403
}
```
- 修改了并非自己参与的聊天
```json
{
  "code": -4,
  "info": "修改了并非自己参与的聊天",
  "status": 403
}
```
- 修改了单聊的名称和头像
```json
{
  "code": -4,
  "info": "修改了单聊的名称和头像",
  "status": 403
}
```
- 群聊成员无权限修改群聊公有信息
```json
{
  "code": -4,
  "info": "群聊成员无权限修改群聊公有信息",
  "status": 403
}
```
- 缺失令牌
```json
{
  "code": -4,
  "info": "缺失令牌",
  "status": 403
}
```
- 无效的 JSON 格式
```json
{
  "code": -1,
  "info": "Invalid JSON format",
  "status": 400
}
```
- 错误的请求方法
```json
{
  "code": -3,
  "info": "Bad method",
  "status": 405
}
```
- 聊天不存在
```json
{
  "code": -4,
  "info": "聊天不存在",
  "status": 403
}
```


## DELETE `/chats/{chatId}`

该 API 用于群主解散群聊。

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求体

```json
{
    "fromUserId": "<string>",
}
```

- `fromUserId`：删除者的 userId

> 注意可能有两种情况：
>
> - 判断是不是群主
>
> - 判断是不是这个群的群主
>
>

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
}
```

### 错误响应
- 未找到用户
```json
{
  "code": -4,
  "info": "用户不存在",
  "status": 403
}
```
- 非群主
```json
{
  "code": -4,
  "info": "非群主",
  "status": 403
}
```
- 缺失令牌
```json
{
  "code": -4,
  "info": "缺失令牌",
  "status": 403
}
```
- 无效的 JSON 格式
```json
{
  "code": -1,
  "info": "Invalid JSON format",
  "status": 400
}
```
- 错误的请求方法
```json
{
  "code": -3,
  "info": "Bad method",
  "status": 405
}
```
- 聊天不存在
```json
{
  "code": -4,
  "info": "聊天不存在",
  "status": 403
}
```


## GET `/messages/{messageListId}`

该 API 用于获取某个聊天的记录。

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求参数

```json
{
	"fromUserId": "<string>",
}
```

- `fromUserId`：查询者的 userId

> 该 userId 用来检查查询者是否有权限获取这些聊天记录
>
> 显然，只能获取自己参与的聊天的聊天记录，后端错误处理要体现这一点

### 请求体

无

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
  	"chatMessageList": "<ChatMessageList>"
}
```

> ChatMessageList 定义参见前端数据类型定义，注意**不要**返回 id 字段

## POST `/messages/{messageListId}`

该 API 用于在某个MessageList中添加一个消息

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求体

```json
{
	"fromUserId": "<string>",
    "chatMessageContents": ["<ChatMessageContent>", "<ChatMessageContent>",,,]
}
```

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0
  	"info": "Succeed",
  	// "chatMessages": ["<ChatMessage>","<ChatMessage>"]
}
```

> 返回后端构建的 ChatMessage 对象。
>
> **注意，在这个 ChatMessage 对象对象中， ChatMessageMeta 的已读字段应当包含发送者。**即，发送者第一时间已读消息
>
> 此外，后端还要更新这个 chatMessage 所在的 chatMessageList 的：
>
> （1）回复和被回复的关系
>
> （2）该消息可能会影响到的任何其他消息的状态

## PATCH `/messages/{messageListId}`

该 API 用于修改某一个ChatMessage的状态。

当前应该需要修改的应该只有 ChatMessageMeta 

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求体

```json
{
	"fromUserId": "<string>",
    "chatMessage": "<ChatMessage>"
}
```

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
  	"chatMessage": "<ChatMessage>"
}
```

> 返回后端构建的 ChatMessage 对象
>
> 此外，后端还要更新这个 chatMessage 所在的 chatMessageList 的：
>
> （1）回复和被回复的关系
>
> （2）该消息可能会影响到的任何其他消息的状态
>
> 在 socket 中做出这些变更的通知

## GET `/participants/{participantListId}`

该 API 用于获取某个 ChatParticipantList

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求体

```json
{
	"fromUserId": "<string>",
}
```

- `fromUserId`：查询者的 userId

> 该 userId 用来检查查询者是否有权限获取这些成员信息
>
> 显然，只能获取自己参与的聊天的成员信息，后端错误处理要体现这一点

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
  	// "chatParticipantList": "<ChatParticipantList>"
}
```

> ChatParticipantList 定义参见前端数据类型定义，注意**不要**返回 id 字段

## PATCH `/participants/{participantListId}/`

该 API 用于更新群聊用户的身份

如增删管理员，转让群主

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求体

```json
{
	"fromUserId": "<string>",
    "chatParticipant": "<ChatParticipant>"
}
```

- `fromUserId`：操作者的 userId
- `chatParticipant`：操作者修改后的 ChatParticipant 数据

>ChatParticipant 类型定义见前端 definition.ts
>注意处理：
>
>- 该用户是否有权限更改这个 ChatParticipantList
>- 更改后的chatParticipant是否合法（比如，chatParticipant 的 userId 不属于当前的 ChatParticipantList，就说明这是瞎改的，应当报错）

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
  	// "chatParticipant": "<ChatParticipant>"
}
```

> 返回后端更新后的 ChatParticipant 对象
>

## GET `/notifications/{notificationListId}`

该 API 用于查询某个聊天的 ChatNotificationList

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请参数

```json
{
	"fromUserId": "<string>",
}
```

- `fromUserId`：查询者的 userId

>该 userId 用来检查查询者是否有权限获取这些通知记录
>
>显然，只能获取自己参与的聊天的通知记录，后端错误处理要体现这一点

### 请求体

无

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
  	"chatNotificationList" : "<ChatNotificationList>",
}
```

> ChatNotificationList 定义参见前端数据类型定义，不要返回 id 字段

## POST `/notifications/{notificationListId}`

该 API 用于在某个ChatNotificationList中添加ChatNotification

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求体

```json
{
	"fromUserId": "<string>",
    "notification": "<string>"
}
```

- `fromUserId`：通知者的 userId
- `notification`: 该用户要通知的消息

>注意
>
>- 检查用户是否在对应的 ChatNotificationList 中
>- 检查用户是否有权限发表通知，只有群主、管理员可以发通知

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
  	// "chatNotification" : "<ChatNotification>",
}
```

> 返回添加好的 ChatNotification

## GET `/joinRequests/{joinRequestListId}`

该 API 用于获取某个 ChatJoinRequestList（入群请求）

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求参数

```json
{
	"fromUserId": "<string>",
}
```

- `fromUserId`：查询者的 userId

>注意，检查是否有权限查询：
>
>- 应当只有管理员和群主可以查询该群的 ChatJoinRequestList，并且可以通过入群请求

### 请求体

无

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
  	"chatJoinRequestList" : "<ChatJoinRequestList>",
}
```

> ChatJoinRequestList 定义参见前端数据类型定义，不要返回 id 字段

## POST `/joinRequests/{joinRequestListId}`

该 API 用于在某个 ChatJoinRequestList 中添加 ChatJoinRequest（入群请求）

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求体

```json
{
    “fromUserId”: "<string>",
	"userId": "<string>",
    "toChatId": "<string>",
}
```

- `UserId`：入群者的userId
- `fromUserId`：操作者的 userId
- `toChatId`：群聊的chat id

>注意：
>
>- 所有群成员都可以添加入群请求
>- 但是要检查入群请求是否合法，不能是人已经在群里还要入群

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
}
```

> 返回后端创建好的 ChatJoinRequest，ChatJoinRequest 的定义见前端 definition.ts
>
> 错误响应挑出来 重复申请1

```json
{
  	"code": 1,
  	"info": "重复申请"
}
```





## PATCH `/joinRequests/{joinRequestListId}`

该 API 用于更改 ChatJoinRequestStatus。即，群主或管理员同意是否入群。

### 请求头

请求头需要将 `Authorization` 字段设置为 JWT 令牌。

### 请求体

```json
{
	“fromUserId”: "<string>",
    "chatJoinRequest": "<ChatJoinRequest>"
}
```

- `fromUserId`：操作者的 userId

>注意：
>
>- 只有群主和管理员可以通过/拒绝请求
>- 但是要检查入群请求是否合法，不能是人已经在群里

### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
  	"chatJoinRequest": "<ChatJoinRequest>"
}
```

> 返回后端修改好的 ChatJoinRequest，ChatJoinRequest 的定义见前端 definition.ts



# WebSocket API

>客户端：前端
>
>服务器：后端

## EVENT `friend_request_receive`

### 描述

服务器通知客户端一个新的好友请求

### 服务器到客户端的数据

```json
{
    "friendRequest": {
        "requestId": "<string>",
        "createdAt": "<string>",
    	"fromUserId": "<string>",
    	"toUserId": "<string>",
    	"status": "<FriendRequestStatus>"
    }
}
```



## EVENT `friend_request_update`

### 描述

这个 API 用来更新某个好友请求的状态

>比如：
>
>- 用户取消了申请，报 Canceled
>- 对方突然注销了（？）或者其他服务器错误，报 Failed
>

### 服务器到客户端的数据

```json
{
    "friendRequest": {
        "requestId": "<string>",
        "createdAt": "<string>",
    	"fromUserId": "<string>",
    	"toUserId": "<string>",
    	"status": "<FriendRequestStatus>"
    }
}
```



## EVENT `friend_added`

### 描述

服务器提醒客户端某个好友被成功添加了。

> 注意：
>
> - `friend_request_update`：只用来通知好友请求的状态更新
> - 如果更新 friend_request 时发现某个人将状态置为 Accepted，就可以在服务端的数据库添加好友关系
> - 数据库中添加好友关系后，再进行 `friend_added` 通知，告知**请求的两方**好友关系
> - 这样可以避免耦合，也方便设计 socket handler

### 服务器到客户端的数据

```json
{
    "user": {
        "userId": "<string>",
        "userName": "<string>",
        "phoneNumber": "<string>",
        "emailAddress": "<string>",
        "avatarUrl": "<string>"
    }
}
```

> 注意：如果是 A 和 B 之间的好友请求，则应该给 A 发 userB，给 B 发 userA



## EVENT `friend_removed`

### 描述

服务器提醒客户端好友关系被删除了。

> 注意：
>
> - HTTP api 中，有 `/friends/{friendId}` 的 DELETE 方法。该方法显然只能用于单向删除好友。
> - 如果想要双向删除好友，则可以用该 API 从 socket 中通知两人。
> - 至于只做单向删除还是做双向删除，可以由后端自行决定。前端只设置对应的 handler。
> - 总之，删除好友的 HTTP api 和 socket api 都要实现，然后按需处理业务逻辑即可。

### 服务器到客户端的数据

```json
{
    "user": {
        "userId": "<string>",
        "userName": "<string>",
        "phoneNumber": "<string>",
        "emailAddress": "<string>",
        "avatarUrl": "<string>"
    }
}
```

> 注意：如果是 A 和 B 之间的好友关系解除，则应该给 A 发 userB，给 B 发 userA



## EVENT `user_profile_update`

### 描述

服务器提醒客户端某个用户的信息更新了。

### 服务器到客户端的数据

```json
{
    "user": {
        "userId": "<string>",
        "userName": "<string>",
        "phoneNumber": "<string>",
        "emailAddress": "<string>",
        "avatarUrl": "<string>"
    }
}
```



## EVENT `user_unregister`

### 描述

服务器提醒客户端某个用户注销了。

### 服务器到客户端的数据

```json
{
    "user": {
        "userId": "<string>",
        "userName": "<string>",
        "phoneNumber": "<string>",
        "emailAddress": "<string>",
        "avatarUrl": "<string>"
    }
}
```



## EVENT `chat_update`

### 描述

服务器提醒客户端某个 Chat 对象的信息更新了。

### 服务器到客户端的数据

```json
{
    "chat": "<Chat>"
}
```



## EVENT `chat_deleted`

### 描述

服务器提醒客户端某个 Chat 对象被删除了。

### 服务器到客户端的数据

```json
{
    "chatId": "<Chat>"
}
```



## EVENT `message_list_update`

### 描述

服务器提醒客户端某个 ChatMessageList 对象的信息更新了。

### 服务器到客户端的数据

```json
{
    "chatMessageList": "<ChatMessageList>"
}
```



## EVENT `message_list_deleted`

### 描述

服务器提醒客户端某个 ChatMessageList对象被删除了。

### 服务器到客户端的数据

```json
{
    "chatMessageList": "<ChatMessageList>"
}
```



## EVENT `participant_list_update`

### 描述

服务器提醒客户端某个 ChatParticipantList 对象的信息更新了。

### 服务器到客户端的数据

```json
{
    "chatParticipantList": "<ChatParticipantList>"
}
```



## EVENT `participant_list_deleted`

### 描述

服务器提醒客户端某个 ChatParticipantList 对象被删除了。

### 服务器到客户端的数据

```json
{
    "chatParticipantList": "<ChatParticipantList>"
}
```



## EVENT `notification_list_update`

### 描述

服务器提醒客户端某个 ChatNotificationList 对象的信息更新了。

### 服务器到客户端的数据

```json
{
    "chatNotificationList": "<ChatNotificationList>"
}
```



## EVENT `notification_list_deleted`

### 描述

服务器提醒客户端某个 ChatNotificationList 对象被删除了。

### 服务器到客户端的数据

```json
{
    "chatNotificationList": "<ChatNotificationList>"
}
```



## EVENT `join_request_list_update`

### 描述

服务器提醒客户端某个 ChatJoinRequestList 对象的信息更新了。

### 服务器到客户端的数据

```json
{
    "chatJoinRequestList": "<ChatJoinRequestList>"
}
```

> 群主，管理员有权限获得该通知



## EVENT `join_request_list_deleted`

### 描述

服务器提醒客户端某个 ChatJoinRequestList 对象被删除了。

### 服务器到客户端的数据

```json
{
    "chatJoinRequestList": "<ChatJoinRequestList>"
}
```



# Appendix

这里提供前端的数据类型定义，我尽量使之和 API 中的传输类型适配，可以用作参考：
```typescript
// Definition of types and constants used in the application


// User
export type User = {
    id?: number;
    userId: string;
    userName: string;
    emailAddress: string;
    phoneNumber: string;
    avatarUrl: string;
};

export const INITIAL_USER: User = {
    userId: "",
    userName: "",
    emailAddress: "",
    phoneNumber: "",
    avatarUrl: "",
};

export type Users = User[];


// FriendGroup
export type ResponseFriendGroup = {
    id?: number;
    groupId: string;
    groupName: string;
    friends: Users;
};

export type ResponseFriendGroups = ResponseFriendGroup[];

export type FriendGroup = {
    id?: number;
    groupId: string;
    groupName: string;
    friends: string[]; // userIds
};

export type FriendGroups = FriendGroup[];

export const DEFAULT_FRIEND_GROUP_NAME = "My Friends";

// FriendRequest
export enum FriendRequestStatus {
    // Only for sent requests
    Pending = 'Pending',
    Canceled = 'Canceled',
    // Only for received requests
    Accepted = 'Accepted',
    Rejected = 'Rejected',
    // Error status
    Failed = 'Failed'
}

export type FriendRequest = {
    id?: number;
    requestId: string;
    createdAt: string;
    fromUserId: string;
    toUserId: string;
    status: FriendRequestStatus;
}

export type FriendRequests = FriendRequest[];


// ChatMessage
export enum ChatMessageContentType {
    Text = 'Text',
    Image = 'Image',
    Video = 'Video',
    Audio = 'Audio',
    File = 'File',
    Forwarded = 'Forwarded'
}

export type ChatMessageContent = {
    contentType: ChatMessageContentType;
    contentPayload: string;
}

export type ChatMessageMeta = {
    withdrawn: boolean;
    deleted: boolean;
    readBy: string[]; // userIds
    repliedBy: string[]; // messageIds
    replyMessageId?: string;
}

export const DEFAULT_CHAT_MESSAGE_META: ChatMessageMeta = {
    withdrawn: false,
    deleted: false,
    readBy: [],
    repliedBy: [],
};

export type ChatMessage = {
    messageId: string;
    createdAt: string;
    fromUserId: string;
    content: ChatMessageContent;
    meta: ChatMessageMeta;
}

export type ChatMessages = ChatMessage[];

export type ChatMessageList = {
    id?: number;
    messageListId: string;
    messages: ChatMessages;
};


// ChatParticipant
export enum ChatParticipantType {
    Owner = 'Owner',
    Admin = 'Admin',
    Member = 'Member'
};

export type ChatParticipant = {
    userId: string;
    type: ChatParticipantType;
};

export type ChatParticipants = ChatParticipant[];

export type ChatParticipantList = {
    id?: number;
    participantListId: string;
    participants: ChatParticipants;
};


// ChatNotification
export type ChatNotification = {
    notificationId: string;
    fromUserId: string;
    createdAt: string;
    notification: string;
};

export type ChatNotifications = ChatNotification[];

export type ChatNotificationList = {
    id?: number;
    notificationListId: string;
    notifications: ChatNotifications;
};


// ChatJoinRequest
export enum ChatJoinRequestStatus {
    Pending = 'Pending',
    Accepted = 'Accepted',
    Rejected = 'Rejected',
    Canceled = 'Canceled',
    Failed = 'Failed'
}

export type ChatJoinRequest = {
    joinRequestId: string;
    fromUserId: string;
    toChatId: string;
    createdAt: string; #
    status: ChatJoinRequetStatus; #
};

export type ChatJoinRequests = ChatJoinRequest[];

export type ChatJoinRequestList = {
    id?: number;
    joinRequestListId: string;
    joinRequests: ChatJoinRequests;
};


// Chat
export enum ChatType {
    Private = 'Private',
    Group = 'Group'
}

export type ChatSettings = {
    isMuted: boolean;
    isPinned: boolean;
};

export const DEFAULT_CHAT_SETTINGS: ChatSettings = {
    isMuted: false,
    isPinned: false,
};

export type Chat = {
    id?: number;
    chatId: string;
    createdAt: string;
    chatType: ChatType;
    chatName: string;
    chatAvatarUrl: string;
    chatSettings: ChatSettings;

    messageListId: string;
    participantListId: string;
    notificationListId: string;
    joinRequestListId: string;
};

```

