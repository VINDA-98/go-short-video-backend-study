# Go 短视频后端练习

[English](README.md)

这是一个 Go 练习项目，基于短视频后端 [HammerCloth/tiktok](https://github.com/HammerCloth/tiktok)。可以从 [`router.go`](router.go) 开始，沿着 Gin 路由、JWT 中间件、控制器、服务层和数据访问层查看请求如何处理。

## API 模块

| 模块 | 接口覆盖范围 |
| --- | --- |
| 视频 | 获取 Feed、上传视频、查询用户投稿。[设计说明](https://bytedancecampus1.feishu.cn/docs/doccntmcunjHSMzVUNEhGbxjxJh) |
| 用户 | 注册、登录、查询用户资料。[设计说明](https://bytedancecampus1.feishu.cn/docs/doccn1vusmV9oN1ukTCyLpbJ46f) |
| 点赞与评论 | 点赞、取消点赞、查看点赞列表，以及发表评论、删除评论和查看评论。[点赞设计](https://bytedancecampus1.feishu.cn/docs/doccn13iJgTIAebIPpMiRqb0Hwb) · [评论设计](https://bytedancecampus1.feishu.cn/docs/doccnDqfcZJW4tTD409NGlYfvCb) |
| 关注 | 关注、取关、查询关注列表和粉丝列表。[设计说明](https://bytedancecampus1.feishu.cn/docs/doccnOsdm29SufPJkDfRs7tLHgx) |

## 架构

![短视频后端架构](images/framework.jpg)

Gin 处理 HTTP 请求，GORM 负责访问 MySQL。Redis 用于缓存，RabbitMQ 消费者处理关注、点赞和评论事件。媒体文件通过 FTP 传输，视频截图由 FFmpeg 提取。相关设计见 [MySQL 数据库设计](images/mysql.jpg)、[Redis 设计](images/redis.jpg) 和 [RabbitMQ 设计](images/rabbitmq.jpg)。

## 本地运行

原开发环境使用 Go 1.18.1。先用 [`config/douyin.sql`](config/douyin.sql) 初始化本地 MySQL，再配置 Redis、RabbitMQ，以及提供 Nginx、vsftpd、FFmpeg 和 SSH 的媒体主机。相关配置和服务示例位于 [`config/`](config/)。启动前请配置自己的开发服务地址和凭据。

配置完成后，运行 `go run .`，服务监听 `9099` 端口。原项目的 Linux AMD64 编译命令如下。

```sh
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build .
```

如果要在手机上演示，需要另外准备兼容这套抖音练习 API 的客户端。

## 性能分析记录

仓库保留了通过 `pprof` 采集 120 秒 CPU 数据生成的 [v1.0 火焰图](images/1.0.png) 和 [v1.2 火焰图](images/1.2.png)。原项目记录提到，接入中间件后，相同方法调用耗时缩短。历史对比见火焰图及[原压测报告](https://bytedancecampus1.feishu.cn/docs/doccnoDHHJ84k94G1I2TxHj9Udh)。

许可条款见 [LICENSE](LICENSE)。
