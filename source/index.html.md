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

`POST http://localhost:5000/api/register`

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

`POST http://localhost:5000/api/login`

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

`GET http://localhost:5000/api/logout`


# Music Module

## Create Music

## Replace Music

## Update Music

## Get a Piece of Music

## Get multiple music

# Search Module

## Get search suggestion

## Search music
