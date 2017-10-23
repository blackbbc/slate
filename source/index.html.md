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

# Response

当请求到资源时，HTTP状态码为2XX，否则为4XX

当HTTP状态码为2XX时，所请求的资源在即在json串里

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
游客     | 10    |     ✓    |          |          |          |          |
注册用户 | 20    |     ✓    |     ✓    |          |          |          |
高级用户 | 30    |     ✓    |     ✓    |    ✓     |    ✓     |          |
管理员   | 40    |     ✓    |     ✓    |    ✓     |    ✓     |    ✓     |


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
    "passwordHash": "pbkdf2:sha256:50000$2igfGrPk$b0b92bf7363e1a12a70d08701ea52ad5c10ffb1552e2d1c68e1c76400790f0e7",
    "role": 20,
    "username": "foo"
}
```


### HTTP Request

`POST /api/register`

### Query Parameters

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
  "remember": True
}
```

> 示例返回值


```json
{
  "_id": {
    "$oid": "59ecc6efe549a24e4947b42f"
  },
    "email": "123456@qq.com",
    "passwordHash": "pbkdf2:sha256:50000$2igfGrPk$b0b92bf7363e1a12a70d08701ea52ad5c10ffb1552e2d1c68e1c76400790f0e7",
    "role": 20,
    "username": "foo"
}
```

### HTTP Request

`POST /api/login`

### Query Parameters

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
    "passwordHash": "pbkdf2:sha256:50000$2igfGrPk$b0b92bf7363e1a12a70d08701ea52ad5c10ffb1552e2d1c68e1c76400790f0e7",
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

## Update User

更新一个用户信息


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

### Query Parameters

参数       | 类型     | 可选 | 描述     |
---------- | -------- | :--: | -------- |
title      | string   |      | 歌曲名称 |
alias      | [string] |  ✓   | 别名     |
author     | string   |  ✓   | 歌曲作者 |
album      | string   |  ✓   | 专辑     |
tags       | [string] |  ✓   | 标签     |
content    | string   |      | 曲谱内容 |
references | [object] |  ✓   | 相关链接，格式为`{"name": string, "url": string}` |

## Update Music

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
page  |  ✓   |   1    | 页标     |
size  |  ✓   |   10   | 每页数量 |
sort  |  ✓   |   date | 排序依据, `date`按发布时间, `views`按查看数 |
order |  ✓   |   desc | 排序规则, `asc`升序, `desc`降序 |

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
page |  ✓   |  1     | 页标     |
size |  ✓   |  10    | 每页数量 |
