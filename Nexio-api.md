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
        "userId": "<string>",
        "userName": "<string>",
        "phoneNumber": "<string>",
        "emailAddress": "<string>",
        "avatarUrl": "<string>"
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

使用 PUT 方法请求该 API 即表示修改 id 为 `userId` 的用户的 profile

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
    "resluts": [
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

> 注意这里：要返回所有认为符合 query 信息的用户。具体那些人符合 query 信息，可以由后端自行定夺



## URL `/friends/groups`

该 API 用于创建好友分组

该 API 仅接受以 POST 和 PATCH 方法的请求。以其他方法请求均应当设置状态码为 405 Method Not Allowed，错误响应格式为：

```json
{
    "code": -3,
    "info": "Bad method"
}
```

### POST

使用 POST 方法新建一个空分组。（即，当前的组刚刚创建，还没有

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

-  `fromGroupName`：原先的组名，必须是现有的组名
-  `toGroupName`：新的组名，必须是现有的组名

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
}
```



## URL `/friends`

该 API 用于获取好友列表（支持分组）

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
    "FriendGroups": [
        {
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
>userId: string;
>userName: string;
>phoneNumber: string;
>emailAddress: string;
>avatarUrl: string;
>};
>
>export type Friends = User[];
>
>export type FriendGroup = {
>groupName: string;
>friends: Friends;
>};
>
>export type FriendGroups = FriendGroup[];
>
>export const DEFAULT_GROUP_NAME = "My Friends";
>
>export const initialFriendGroups: FriendGroups = [
>{
>   groupName: DEFAULT_GROUP_NAME,
>   friends: []
>}
>];
>```
>
>数据结构比较复杂，要小心实现



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

#### 请求体

```json
{
  	"userId": "<string>",
}
```

上述字段的说明为：

-  `userId`：查询者的 userId

#### 成功响应

请求成功时，应当设置状态码为 200 OK，成功响应格式为：

```json
{
  	"code": 0,
  	"info": "Succeed",
    "FriendRequests": [
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
>Pending = 'Pending', // 发送中
>Accepted = 'Accepted', // 同意请求
>Rejected = 'Rejected', // 拒绝请求
>Canceled = 'Canceled', // 撤回请求
>Failed = 'Failed' // 服务器错误
>}
>
>export type FriendRequest = {
>requestId: string;
>createdAt: string;
>fromUserId: string;
>toUserId: string;
>status: FriendRequestStatus;
>};
>```
>
>返回一个 FriendRequest 的列表

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
    "FriendRequest": {
    	requestId: "<string>",
    	createdAt: "<string>",
    	fromUserId: "<string>",
    	toUserId: "<string>",
    	status: "<FriendRequestStatus>"
	}
}
```

### PATCH

PATCH 方法用来更新一个好友请求的状态。

#### 请求头

使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 `Authorization` 字段设置为 JWT 令牌。

#### 请求体

```json
{
    "FriendRequest": {
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
    "FriendRequest": {
    	requestId: "<string>",
    	createdAt: "<string>",
    	fromUserId: "<string>",
    	toUserId: "<string>",
    	status: "<FriendRequestStatus>"
	}
}
```



## URL `/friends/{friendId}`

> friendId：即 friend 的 userId

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



## EVENT `friend_profile_update`

### 描述

服务器提醒客户端某个好友的信息更新了。

> 注意：这个 API 应该用在有好友关系的人之前通知，避免广播

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



## EVENT `friend_unregister`

### 描述

服务器提醒客户端某个好友注销了。

> 关于这个 api：
>
> - 如果使用，则好友注销后从用户列表中消失
> - 如果不使用，则发消息给注销的好友时要显示 Failed
>
> 建议做两手准备，因为 socket 可能无法及时提醒删除好友

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



## EVENT `chat_user_profile_update`

### 描述

服务器提醒客户端某个群友的信息更新了。

> 不论群友是不是好友，都要提醒。也就是说，如果是好友，并且在同一个会话中，就要先发一遍`friend_profile_update`，再给每一个相关的会话发一遍 `chat_user_profile_update`
>
> 因为前端初步打算将 friends 和 chat 中的用户分别存储，只能多次更新。如果之后找到更好的实现方式，可以改动该 API。

### 服务器到客户端的数据

```json
{
    "chatId": "<string>",
    "user": {
        "userId": "<string>",
        "userName": "<string>",
        "phoneNumber": "<string>",
        "emailAddress": "<string>",
        "avatarUrl": "<string>"
    }
}
```



# Appendix

这里提供前端的数据类型定义，我尽量使之和 API 中的传输类型适配，可以用作参考：

```typescript
export type User = {
    userId: string;
    userName: string;
    phoneNumber: string;
    emailAddress: string;
    avatarUrl: string;
};

export type Friends = User[];

export type FriendGroup = {
    groupName: string;
    friends: Friends;
};

export type FriendGroups = FriendGroup[];

export const DEFAULT_GROUP_NAME = "My Friends";

export const initialFriendGroups: FriendGroups = [
    {
        groupName: DEFAULT_GROUP_NAME,
        friends: []
    }
];

export enum FriendRequestStatus {
    Pending = 'Pending', // 发送中
    Accepted = 'Accepted', // 同意请求
    Rejected = 'Rejected', // 拒绝请求
    Canceled = 'Canceled', // 撤回请求
    Failed = 'Failed' // 服务器错误
}

export type FriendRequest = {
    requestId: string;
    createdAt: string;
    fromUserId: string;
    toUserId: string;
    status: FriendRequestStatus;
};
```

