# Change Plan 变更小试验

[![Build Status](https://cloud.drone.io/api/badges/tsingson/goim/status.svg)](https://cloud.drone.io/tsingson/goim)  [![GoDoc](https://godoc.org/github.com/tsingson/goim?status.svg)](https://godoc.org/github.com/tsingson/goim)

>
>
>
>  有个 slack 频道, 不少朋友在交流 goim , 欢迎加入[slack #goim](https://join.slack.com/t/reading-go/shared_invite/enQtMjgwNTU5MTE5NjgxLTA5NDQwYzE4NGNhNDI3N2E0ZmYwOGM2MWNjMDUyNjczY2I0OThiNzA5ZTk0MTc1MGYyYzk0NTA0MjM4OTZhYWE)
>
>
>


有几位朋友私信沟通闲聊, 想要一个同时支持 kafka / nats , 以便 merge 原有代码, 我 fork 了一个 repo 来尝试实现这个想法, 这里 https://github.com/tsingson/goim, 在将来几天内处理完成



### 计划变更如下:
  - [x] 在 internal/logic/conf 与 internal/job/conf 中增加 nats 的连接配置项, 与 选择 kafka ( 默认) 或 nats 的开关配置项
  - [x] 把 internal/logic/dao 抽象为 interface , 同时支持 kafka / nats ( 仅是 nats )
  - [ ] ~~把 internal/job 中 func (j *Job) Consume() 函数拆分为  func (j *Job) Consume() 支持 kafka / func (j *Job) ConsumeNats()  支持 nats~~
  - [x] 把 internal/job 中 func (j *Job) Consume() 抽取为 interface 支持 nats
  - [x] 修改 job / logic 配置项,  从 toml 文件中读取 Nats 开关项与连接配置
  - [x] 抽离 job / comet 中有关 bilibili/discovery 相关调用代码, 以便更换
  - [ ] ~~计划增加聊天消息存储接口, 提供消息离线存储与聊天机器人对接用~~
  - [ ] ~~计划处理离线消息独立存储( 或加离线标识, 以便用户上线时重新获取)~~
  - [ ] 测试, 测试, 测试

除以上变更外, 所有代码尽量保持不变



### 架构与定制建议

![goim-architecture-0](/docs/goim-architecture-0.png)

见这里 [https://github.com/tsingson/goim/wiki/Customization-(chinese-version)](https://github.com/tsingson/goim/wiki/Customization-(chinese-version))


以上, 祝愉快.

----------------

Some friends ask to [https://github.com/Terry-Mao/goim](https://github.com/Terry-Mao/goim) support the kafka / nats. 
I forked a repo to try to implement this idea, here https://github.com/tsingson/goim, Completed in few days



The plan  are:

   - [x] Add nats connection configuration in internal/logic/conf and internal/job/conf, and switch configuration for kafka (default) or nats
   - [x] Abstract internal/logic/dao as interface to support kafka / nats (only nats, no liftbridge )

   - [ ] ~~Split the func (j *Job) Consume() function in internal/job into func (j *Job) Consume() Support kafka ( default) / func (j *Job) ConsumeNats() Support nats~~
  - [x] Split the  func (j *Job) Consume() function in internal/job into interface , to support kafka / nats 
  - [x] add nats switch setting and nats client connect setting , modify  job / logic read configuration from toml file 
  - [ ] testing, more testing 

it's all.

ps, wish you happiness.





goim v2.0
==============
[![Build Status](https://travis-ci.org/Terry-Mao/goim.svg?branch=master)](https://travis-ci.org/Terry-Mao/goim) 
[![Go Report Card](https://goreportcard.com/badge/github.com/Terry-Mao/goim)](https://goreportcard.com/report/github.com/Terry-Mao/goim)
[![codecov](https://codecov.io/gh/Terry-Mao/goim/branch/master/graph/badge.svg)](https://codecov.io/gh/Terry-Mao/goim)

goim is a im server writen by golang.

## Features
 * Light weight
 * High performance
 * Pure Golang
 * Supports single push, multiple push and broadcasting
 * Supports one key to multiple subscribers (Configurable maximum subscribers count)
 * Supports heartbeats (Application heartbeats, TCP, KeepAlive, HTTP long pulling)
 * Supports authentication (Unauthenticated user can't subscribe)
 * Supports multiple protocols (WebSocket，TCP，HTTP）
 * Scalable architecture (Unlimited dynamic job and logic modules)
 * Asynchronous push notification based on Kafka

## Architecture
![arch](./docs/arch.png)

## Quick Start

### Build
```
    make build
```

### Run
```
    make run
    make stop

    // or
    nohup target/logic -conf=target/logic.toml -region=sh -zone=sh001 -deploy.env=dev -weight=10 2>&1 > target/logic.log &
    nohup target/comet -conf=target/comet.toml -region=sh -zone=sh001 -deploy.env=dev -weight=10 -addrs=127.0.0.1 2>&1 > target/logic.log &
    nohup target/job -conf=target/job.toml -region=sh -zone=sh001 -deploy.env=dev 2>&1 > target/logic.log &

```
### Environment
```
    env:
    export REGION=sh
    export ZONE=sh001
    export DEPLOY_ENV=dev

    supervisor:
    environment=REGION=sh,ZONE=sh001,DEPLOY_ENV=dev

    go flag:
    -region=sh -zone=sh001 deploy.env=dev
```
### Configuration
You can view the comments in target/comet.toml,logic.toml,job.toml to understand the meaning of the config.

### Dependencies
[Discovery](https://github.com/bilibili/discovery)

[Kafka](https://kafka.apache.org/quickstart)

## Document
[Protocol](./docs/protocol.png)

[English](./README_en.md)

[中文](./README_cn.md)

## Examples
Websocket: [Websocket Client Demo](https://github.com/Terry-Mao/goim/tree/master/examples/javascript)

Android: [Android](https://github.com/roamdy/goim-sdk)

iOS: [iOS](https://github.com/roamdy/goim-oc-sdk)

## Benchmark
![benchmark](./docs/benchmark.jpg)

### Benchmark Server
| CPU | Memory | OS | Instance |
| :---- | :---- | :---- | :---- |
| Intel(R) Xeon(R) CPU E5-2630 v2 @ 2.60GHz  | DDR3 32GB | Debian GNU/Linux 8 | 1 |

### Benchmark Case
* Online: 1,000,000
* Duration: 15min
* Push Speed: 40/s (broadcast room)
* Push Message: {"test":1}
* Received calc mode: 1s per times, total 30 times

### Benchmark Resource
* CPU: 2000%~2300%
* Memory: 14GB
* GC Pause: 504ms
* Network: Incoming(450MBit/s), Outgoing(4.39GBit/s)

### Benchmark Result
* Received: 35,900,000/s

[中文](./docs/benchmark_cn.md)

[English](./docs/benchmark_en.md)

## LICENSE
goim is is distributed under the terms of the MIT License.
