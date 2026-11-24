# Go Short-Video Backend Study

[简体中文](README.zh-CN.md)

A Go short-video backend study based on [HammerCloth/tiktok](https://github.com/HammerCloth/tiktok). Use [`router.go`](router.go) to trace a request from its Gin route through JWT middleware, the controller, service and data-access code.

## API modules

| Module | Endpoints cover |
| --- | --- |
| Video | Feed, upload, and a user's published videos. [Design notes](https://bytedancecampus1.feishu.cn/docs/doccntmcunjHSMzVUNEhGbxjxJh) |
| Users | Registration, sign-in, and profiles. [Design notes](https://bytedancecampus1.feishu.cn/docs/doccn1vusmV9oN1ukTCyLpbJ46f) |
| Likes and comments | Like or unlike a video, list favorites, and add, delete, or list comments. [Likes](https://bytedancecampus1.feishu.cn/docs/doccn13iJgTIAebIPpMiRqb0Hwb) · [Comments](https://bytedancecampus1.feishu.cn/docs/doccnDqfcZJW4tTD409NGlYfvCb) |
| Follows | Follow or unfollow a user and list following accounts and followers. [Design notes](https://bytedancecampus1.feishu.cn/docs/doccnOsdm29SufPJkDfRs7tLHgx) |

## Architecture

![Short-video backend architecture](images/framework.jpg)

Gin handles HTTP requests and GORM connects to MySQL. Redis supports caching, and RabbitMQ consumers process follow, like, and comment events. The media path uses FTP for transfers and FFmpeg for frame extraction. See the [MySQL schema diagram](images/mysql.jpg), [Redis diagram](images/redis.jpg), and [RabbitMQ diagram](images/rabbitmq.jpg).

## Local setup

The original development setup used Go 1.18.1. Initialize a local MySQL instance with [`config/douyin.sql`](config/douyin.sql), configure Redis and RabbitMQ, then provide a media host with Nginx, vsftpd, FFmpeg, and SSH. [`config/`](config/) contains the configuration and service examples. Use your own development service settings before starting the application.

With those dependencies configured, run `go run .` and use port `9099`. The original Linux AMD64 build command is:

```sh
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build .
```

For a mobile demo, use a separate client compatible with the Douyin practice API.

## Profiling notes

The repository retains [v1.0](images/1.0.png) and [v1.2](images/1.2.png) CPU flame graphs from a 120-second `pprof` capture. The original notes report shorter time in the same calls after adding middleware. The graphs and [original load-test report](https://bytedancecampus1.feishu.cn/docs/doccnoDHHJ84k94G1I2TxHj9Udh) provide the historical comparison.

See [LICENSE](LICENSE) for the license.
