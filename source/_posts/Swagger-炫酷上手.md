---
title: Swagger 炫酷上手
date: 2017-07-24 15:52:10
categories: Swagger #文章分类
tags: [Swagger] #文章标签
description: 最（hen）流（niu）行（bi）的API表达工具
copyright: true
<!-- top: 20 -->
---


<!--more-->

> 描述API接口文档（OpenAPI规范）

**一个例子**    
>  #之后为我的备注

```
swagger: "2.0" #  OpenAPI规范的版本号

info: # 文档描述信息
  version: v1.0.0
  title: 标题
  description: 这个文档的描述

schemes:  # API的URL
  - http
host: 127.0.0.1:8080
basePath: /MyTest

paths: # API的操作
###########API开始##################
    /player/compare/{playerId}: 
    # http://127.0.0.1:8080/MyTest//player/compare/{playerId}----进行调用api
      get:
        tags: # 这个api的标签分类
          - player
        description: 获取球员场均数据
        parameters:
          - in: path # 路径参数（api的url要带上，名称要一致）
            name: playerId
            description: 球员id
            required: true
            type: string
          - in: query
            name: gender
            description: 性别
            required: false
            type: string
            enum:# 枚举
               - F
               - M
          - in: query  # 请求参数
            name: competitionId
            description: 赛事id
            required: false
            type: string
        #  此外还有一个消息体参数(in: body)
        
        # 引入分页参数，下方的parameters定义
        - $ref: "#/parameters/pageSize"
        - $ref: "#/parameters/page"
        responses:# 响应
          200: # 响应类型（HTTP状态码）
            description: 请求成功
            schema: # 响应内容
                type: object # 对象类型
                properties: 
                    totalCount:
                      type: integer
                      description: 统计数据数量
                    playerData:
                      type: array# 数组类型
                      description: 统计数据总和
                      items:
                        type: object
                        description: 球员统计数据模型
                        $ref: '#/definitions/PlayerDataModel'
          500: # 下文responses定义
              $ref: "#/responses/Standard500ErrorResponse"
          404: # 下文responses定义
              $ref: "#/responses/Standard404ErrorResponse"
        security: # 安全验证
          - access_token: []
##############API结束#################
securityDefinitions:
    access_token:
        type: apiKey
        name: Authorization
        in: header  
definitions: # 自定义模型
    Error:
          required:
            - code
            - message
          properties:
            code:
              type: string
            message:
              type: string
    PlayerDataModel:
        properties:
            teamId:
              type: string
              description: 球队id
            playerId:
              type: string
              description: 球员id
            playerName:
              type: string
              description: 球员名称
responses: # 自定义响应模型
    Standard500ErrorResponse:
        description: 请求失败
        schema:
          $ref: "#/definitions/Error"
    Standard404ErrorResponse:
        description: 页面不存在
parameters: # 自定义参数模型
    pageSize:
        name: pageSize
        in: query
        description: 每页大小
        type: integer
        format: int32
        required: false
    page:
        in: query
        name: page
        description: 页码
        type: integer
        format: int32
        required: false
```

<center>
**[详细解释传送门](https://www.gitbook.com/book/huangwenchao/swagger/details)**
</center>

