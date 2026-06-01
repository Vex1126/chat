# 分布式聊天系统

这是一个基于 Qt、C++ 服务端和 Node.js 验证服务实现的聊天系统示例项目。项目包含桌面客户端、网关服务、状态服务、聊天服务集群以及邮箱验证码服务，整体用于演示用户注册登录、好友关系、消息通信、服务发现与验证码校验等功能。

## 项目结构

```text
chat-main/
├── chat-client/     # Qt 桌面聊天客户端
├── GateSever/       # 网关服务，负责 HTTP 接入、登录注册及服务转发
├── StatusSever/     # 状态服务，负责分配可用聊天服务器
├── ChatSever1/      # 聊天服务实例 1
├── ChatSever2/      # 聊天服务实例 2
└── VaifyServer/     # Node.js 邮箱验证码 gRPC 服务
```

> 目录名中保留了当前项目原有拼写，例如 `GateSever`、`VaifyServer`。

## 技术栈

- 客户端：Qt Widgets、C++17
- 服务端：C++、Boost.Asio、gRPC、MySQL、Redis
- 验证服务：Node.js、gRPC、Redis、Nodemailer
- 数据存储：MySQL
- 缓存与验证码：Redis

## 功能概览

- 用户注册、登录、重置密码
- 邮箱验证码发送与校验
- 好友搜索、添加好友、好友申请处理
- 聊天列表、联系人列表、聊天窗口
- 文本消息收发
- 网关服务统一接入
- 状态服务分配聊天服务器
- 多聊天服务器实例之间的 RPC 通信

## 环境准备

### 客户端

需要安装：

- Qt 5 或 Qt 6
- 支持 C++17 的编译器
- Qt Creator 或 qmake 构建环境

客户端工程文件位于：

```text
chat-client/chat.pro
```

### C++ 服务端

需要准备：

- Visual Studio C++ 构建环境
- Boost
- gRPC / Protobuf
- MySQL Connector/C++
- Redis 服务
- MySQL 服务

各服务端目录下包含对应的 `.sln`、`.vcxproj` 和配置文件。

### Node 验证服务

进入 `VaifyServer` 后安装依赖：

```bash
npm install
```

启动验证码服务：

```bash
npm run server
```

默认监听：

```text
0.0.0.0:50051
```

## 配置说明

客户端配置：

```text
chat-client/config.ini
```

默认连接网关：

```ini
[GateSever]
host=localhost
port=8080
```

C++ 服务端配置主要位于各服务目录的 `config.ini` 中，包括：

- 网关服务端口
- 验证服务地址
- Redis 地址、端口和密码
- MySQL 地址、端口、账号和数据库
- 状态服务地址
- 当前聊天服务器实例信息
- 其他聊天服务器 RPC 地址

Node 验证服务配置：

```text
VaifyServer/config.json
```

其中包含邮箱授权码、MySQL 和 Redis 连接信息。正式使用或提交公开仓库前，请替换为自己的配置，并避免上传真实敏感信息。

## 推荐启动顺序

1. 启动 MySQL 和 Redis。
2. 启动 `VaifyServer` 验证码服务。
3. 启动 `StatusSever` 状态服务。
4. 启动 `ChatSever1` 和 `ChatSever2` 聊天服务。
5. 启动 `GateSever` 网关服务。
6. 使用 Qt Creator 打开并运行 `chat-client/chat.pro`。

## 默认端口

| 服务 | 默认地址或端口 |
| --- | --- |
| GateSever | `127.0.0.1:8080` |
| VaifyServer | `127.0.0.1:50051` |
| StatusSever | `127.0.0.1:50052` |
| ChatSever1 TCP | `127.0.0.1:8090` |
| ChatSever1 RPC | `127.0.0.1:50055` |
| ChatSever2 TCP | `127.0.0.1:8091` |
| ChatSever2 RPC | `127.0.0.1:50056` |
| Redis | `127.0.0.1:6380` |
| MySQL | `127.0.0.1:3306` |

## 开发提示

- 如果修改了 `.proto` 文件，需要重新生成对应的 gRPC / Protobuf 代码。
- Qt 客户端构建后会将 `config.ini` 和 `static` 静态资源复制到输出目录。
- 本项目包含多个服务，调试时建议先确认 Redis、MySQL 和各服务端口是否正常可用。
- 如果本地端口被占用，需要同步修改对应服务配置和客户端配置。

## 说明

该项目适合作为即时通信系统、Qt 客户端开发、C++ 网络服务、gRPC 服务通信和 Redis/MySQL 集成的学习示例。实际部署时建议补充更完善的异常处理、日志系统、配置隔离、密钥管理和自动化构建流程。
