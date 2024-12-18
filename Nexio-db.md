# Nexio 数据库文档
## 表的用途与表之间的关系

### User

用来存储用户

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储用户的id |
| name | str | 用来存储用户的用户名 |
| password | str | 用来存储用户的密码 |
| created_time | float | 用来存储创建时间，是一个浮点数 |
| sid | str | 用来存储当前端客户端连接到socket服务器时的标识名 |

### Userdata

用来存储用户数据

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储用户数据的id |
| user | User(外键) | 用来存储对应的用户 |
| emails | str | 用来存储用户的邮箱 |
| phonenumber | str | 用来存储用户的电话号码 |
| avatarUrl | file | 用来存储头像文件 |

### Friendship

用来存储好友关系

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储好友关系的id |
| userId | str | 用来存储对应的用户id |
| friendId | str | 用来存储对应好友的id |
| status | bool | 用来存储是否是好友 |

### FriendshipRequest

用来存储好友请求

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储好友请求的id |
| senderId | str | 用来存储发送者id |
| receiverId | str | 用来存储接收者id |
| message | str | 用来存储附加信息 |
| status | str | 用来存储好友请求的状态 |
| createdAt | float | 用来存储创建时间 |

### FriendGroup

用来存储好友分组

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储好友分组的id |
| user | User(外键) | 用来存储对应用户 |
| name | str | 用来存储组名 |
| createdAt | float | 用来存储创建时间 |

### GroupMembership

用来储存哪些好友在指定的组里

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储对应的id |
| group | FriendGroup(外键) | 用来存储对应的组 |
| friendship | Friendship(外键) | 用来存储在组内的好友 |
| createdAt | float | 用来存储创建时间，即进组时间 |

### Chat

用来储存某个聊天

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储聊天的id |
| chatName | str | 用来存储聊天的名字 |
| chatType | str | 用来存储聊天的类型(私聊还是群聊) |
| chatAvatarUrl | file | 用来存储聊天头像 |
| createdAt | float | 用来存储创建时间 |

### MessageinChat

用来存储聊天中的信息

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储聊天信息的id |
| chat | Chat(外键) | 用来存储对应的聊天 |
| user | User(外键) | 用来存储发送消息的用户 |
| content | str | 用来存储信息内容 |
| contentType | str | 用来存储信息类型 |
| createdAt | float | 用来存储创建时间 |
| withdraw | bool | 用来存储信息是否撤回 |

### MessageStatus

用来存储对于某个用户的某条消息的状态

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储id |
| message | MessageinChat(外键) | 用来存储对应的消息 |
| recipient | User(外键) | 用来存储对应此状态的用户 |
| is_read | bool | 用来存储消息是否被此用户阅读 |
| is_replied | bool | 用来存储消息是否被此用户回复 |
| is_deleted | bool | 用来存储消息是否被此用户删除 |

### MessageContent

用来存储可能的多媒体消息内容

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储id |
| message | MessageinChat(外键) | 用来存储对应的消息 |
| image | file | 用来存储发送的图片与表情 |
| file | file | 用来存储发送的文件，视频与音频 |

### repliedMessage

用来存储回复与被回复消息间的关系

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储id |
| message | MessageinChat(外键) | 用来存储对应的消息 |
| recipient | User(外键) | 用来存储发送回复消息的用户 |
| repliedmessageid | str | 用来存储被回复的消息的id |

### ParticipantinChat

用来存储在聊天中的用户

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储id |
| chat | Chat(外键) | 用来存储对应的聊天 |
| user | User(外键) | 用来存储在此聊天中的用户 |
| ChatParticipantType | str | 用来存储用户在聊天中的权限 |
| isMuted | bool | 用来存储聊天是否被该用户静音 |
| isPinned | bool | 用来存储聊天是否被该用户置顶 |
| createdAt | float | 用来存储创建时间，即加入聊天的时间 |

### NotificationinChat

用来存储群公告

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储id |
| chat | Chat(外键) | 用来存储对应的聊天 |
| user | User(外键) | 用来存储发布群公告的用户 |
| content | str | 用来存储群公告内容 |
| createdAt | float | 用来存储创建时间 |

### JoinRequestinChat

用来存储入群申请

| 数据名称 | 数据类型 | 用途 |
| --- | --- | --- |
| id | int | 用来存储id |
| chat | Chat(外键) | 用来存储对应的聊天 |
| fromUserId | str | 用来存储申请加入群聊的用户id |
| toChatId | str | 用来存储想要加入的群聊id |
| status | str | 用来存储请求状态 |
| createdAt | float | 用来存储创建时间 |

#### 表之间关系如图

![db](./db.png)
