---
title: 神社API文档

language_tabs: # must be one of https://git.io/vQNgJ
  - json

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

自由神社后端API文档，所有的请求使用`JSON`格式进行请求

# Request

所有的请求请正确使用`GET`、`POST`、`PUT`、`DELETE`方法，否则会返回`405`错误(Method Not Allowd)

# Response

当请求到资源时，HTTP状态码为2XX，否则为4XX

当HTTP状态码为2XX时，所请求的资源在即在json串里

成功的HTTP响应使用如下状态码：

  - `200`: `GET`、`PUT`、`PATCH`请求成功
  - `201`: `POST`创建成功

当HTTP状态码为4XX时，返回的json串只有一个`message`的键，包含了错误信息

> 示例错误返回

```json
{
    "message": "用户不存在"
}
```

# Role

角色权限

角色     | 权限  | 查看曲谱 | 收藏曲谱 | 上传曲谱 | 修改曲谱 | 审核曲谱 |
-------- | ----- | :------: | :------: | :------: | :------: | :------: |
游客     | 0    |     ✓    |          |          |          |          |
注册用户 | 10    |     ✓    |     ✓    |          |          |          |
高级用户 | 20    |     ✓    |     ✓    |    ✓     |    ✓     |          |
管理员   | 30    |     ✓    |     ✓    |    ✓     |    ✓     |    ✓     |

# Status

曲谱状态

状态   | 取值 |
------ | :--: |
未发布 |  0   |
已发布 |  1   |


# User Module

## Register

注册接口，成功时返回用户信息

> 示例请求值

```json
{
  "username": "foo",
  "email": "12345678@qq.com",
  "password": "bar",
  "password2": "bar"
}
```

> 示例返回值

```json
{
  "_id": {
    "$oid": "59ecc6efe549a24e4947b42f"
  },
    "email": "123456@qq.com",
    "role": 20,
    "username": "foo"
}
```


### HTTP Request

`POST /api/register`

### Payload

参数      | 类型   | 说明     |
--------- | ------ | -------- |
username  | string | 用户名   |
email     | string | 邮箱     |
password  | string | 密码     |
password2 | string | 重复密码 |


## Login

登录接口，成功时返回用户信息

> 示例请求值

```json
{
  "username": "foo",
  "password": "bar",
  "remember": true
}
```

> 示例返回值


```json
{
  "_id": {
    "$oid": "59ecc6efe549a24e4947b42f"
  },
    "email": "123456@qq.com",
    "role": 20,
    "username": "foo"
}
```

### HTTP Request

`POST /api/login`

### Payload

参数      | 类型   | 描述     |
--------- | -----  | -------- |
username  | string | 用户名   |
password  | string | 密码     |
remember  | bool   | 是否记住 |

## Logout

注销

> 示例返回值

```json
{}
```

### HTTP Request

`GET /api/logout`

## Get a Specific User

获取一个用户信息

> 示例请求值

```
/api/users/foo
```

> 示例返回值

```json
{
  "_id": {
    "$oid": "59ecc6efe549a24e4947b42f"
  },
    "email": "123456@qq.com",
    "role": 20,
    "username": "foo"
}
```

### Http Request

`GET /api/users/<username>`

### Url Parameters

参数     | 描述   |
-------- | ------ |
username | 用户名 |

## Get Multiple Users

获取多个用户信息

> 示例请求值

```
/api/users?page=1&size=30
```

> 示例返回值

```json
{
  "total": 1,
  "data": [{
    "_id": {
      "$oid": "59ecc6efe549a24e4947b42f"
    },
      "email": "123456@qq.com",
      "role": 20,
      "username": "foo"
  }]
}
```

### Http Request

`GET /api/users`

### Query Parameters
参数 | 描述     |
---- | -------- |
page | 页码     |
size | 每页数量 |

## Modify User

修改用户信息，限`Admin`用户使用

> 示例请求值

```
/api/users/foo
```

```json
{
  "role": 20
}
```

> 示例返回值

```json

{
  "_id": {
    "$oid": "59ecc6efe549a24e4947b42f"
  },
    "email": "123456@qq.com",
    "role": 20,
    "username": "foo"
}
```

### Http Request

`PATCH /api/users/<username>`

### Url Parameters

参数     | 描述   |
-------- | ------ |
username | 用户名 |

### Payload

参数     | 类型   | 可选 | 描述                     |
-------- | ------ | ---- | ------------------------ |
password | string |  ✓   | 新密码                   |
role     | int    |  ✓   | 角色权限，取值10、20、30 |

## Status

获取当前用户信息，若用户已登录，返回已登录的用户信息，否则返回`401`错误

> 示例请求值

```
/api/status
```

> 示例返回值

```json
{
  "_id": {
    "$oid": "59ecc6efe549a24e4947b42f"
  },
    "email": "123456@qq.com",
    "role": 20,
    "username": "foo"
}
```

### Http Request

`GET /api/status`


# Music Module

## Create Music

创建曲谱，成功时返回曲谱信息

> 示例请求值

```json
{}
```

> 示例返回值

```json
{}
```

### Http Request

`POST /music`

### Payload

参数       | 类型     | 可选 | 描述     |
---------- | -------- | :--: | -------- |
title      | string   |      | 歌曲名称 |
alias      | [string] |  ✓   | 别名     |
author     | string   |  ✓   | 歌曲作者 |
album      | string   |  ✓   | 专辑     |
tags       | [string] |  ✓   | 标签     |
content    | string   |      | 曲谱内容 |
references | [object] |  ✓   | 相关链接，格式为`{"name": string, "url": string}` |

## Modify Music

更新曲谱，成功时返回曲谱信息

> 示例请求值

```json
{}
```

> 示例返回值

```json
{}
```

### Http Request

`PATCH /music/<id>`

### Payload

参数       | 类型     | 可选 | 描述     |
---------- | -------- | :--: | -------- |
title      | string   |  ✓   | 歌曲名称 |
alias      | [string] |  ✓   | 别名     |
author     | string   |  ✓   | 歌曲作者 |
album      | string   |  ✓   | 专辑     |
tags       | [string] |  ✓   | 标签     |
content    | string   |  ✓   | 曲谱内容 |
references | [object] |  ✓   | 相关链接，格式为`{"name": string, "url": string}` |
status     | int      |  ✓   | 状态，取值`0`、`1` |

## Get a Piece of Music

获取一个曲谱，成功时返回曲谱，不存在则返回404错误（参见最后一节）

> 示例请求值

```json
{}
```

> 示例返回值

```json
{}
```

### Http Request

`GET /music/<id>`

### Url Parameters

参数 | 描述     |
---- | -------- |
id   | 曲谱的id |

## Get Multiple Music

获取多个曲谱，成功时返回曲谱列表

> 示例请求值

```json
{}
```

> 示例返回值

```json
{}
```

### Http Request

`GET /music`

### Query Parameters

参数  | 可选 | 默认值 | 描述     |
----- | :--: | :----: | -------- |
page  |  ✓   |   1    | 页码     |
size  |  ✓   |   10   | 每页数量 |
sort  |  ✓   |   date | 排序依据, `date`按发布时间, `views`按查看数 |
order |  ✓   |   desc | 排序规则, `asc`升序, `desc`降序 |

## Star

收藏曲谱

> 示例请求值

```json

```

> 示例返回值

```json

```

### Http Request

### Payload

# Search Module

## Get Search Suggestion

获取搜索推荐

> 示例请求值

```
/suggest?term=极乐
```

> 示例返回值

```json
[{
  "name": "<em class=\"suggest_high_light\">极</em><em class=\"suggest_high_light\">乐</em>净土",
  "value": "极乐净土"
},{
  "name": "<em class=\"suggest_high_light\">极</em><em class=\"suggest_high_light\">乐</em>净土舞蹈教程",
  "value": "极乐净土舞蹈教程"
}]
```

### Http Request

`GET /suggest`

### Query Parameters

参数 | 描述       |
---- | ---------  |
term | 推荐关键词 |


## Search Music

搜索曲谱

> 示例请求值

```json
{}
```

> 示例返回值

```json
{}
```

### Http Request

`GET /search`

### Query Parameters

参数 | 可选 | 默认值 | 描述     |
---- | :--: | :----: | ------   |
q    |  ✓   |  null  | 关键词, `q`和`tag`至少有一个不为空 |
tag  |  ✓   |  null  | 标签, `q`和`tag`至少有一个不为空   |
page |  ✓   |  1     | 页码     |
size |  ✓   |  10    | 每页数量 |

# Donate Module

## Create Donate Record

新增一条捐赠记录

> 示例请求值

```json
{
  "username": "foo",
  "donateDt": 1508747607,
  "amount": 100.0
}
```

> 示例返回值

```json
{}
```

### Http Request

`POST /api/donations`

### Payload

参数     | 类型   | 描述   |
-------- | ------ | ------ |
username | string | 用户名 |
donateDt | long   | 捐赠时间(Unix时间戳，精确到秒) |
amount   | float  | 数量   |

## Get Multiple Donate Records

获取多条捐赠记录

> 示例请求值

```
/api/donations?page=1&&size=10
```

> 示例返回值

```json
{
  "total": 2,
  "data": [{
    "username": "foo",
    "donateDt": 1508747607,
    "amount": 100.0
  }, {
    "username": "foo",
    "donateDt": 1508747307,
    "amount": 100.0
  }]
}
```

### Http Request

`GET /api/donations`

### Payload

参数 | 可选 | 默认值 | 描述     |
---- | :--: | ------ | -------- |
page |  ✓   |  1     | 页码     |
size |  ✓   |  10    | 每页数量 |
