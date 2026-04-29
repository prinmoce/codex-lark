# codex-lark

`codex-lark` 是一个本地运行的飞书 / Lark 机器人桥接层，用来把 Codex 接到你的工作流里。

它把消息链路收敛成：

`飞书消息 -> 本机 codex app-server -> 飞书回复`

核心原则是：Codex 操作留在本地，飞书只负责消息交互。

## 项目定位

- 面向 Feishu / Lark 的本地桥接
- 支持线程绑定、线程切换、状态卡片、审批卡片、模型与推理强度管理
- 适合长期运行的个人开发者工作流
- 由 `codex-im` 演进而来，保留了其本地桥接思路，并在稳定性和交互体验上做了多轮修复

## 主要能力

- 飞书长连接机器人
- 普通对话回复
- 卡片回复与流式更新
- 线程绑定与切换
- `/codex bind` 绑定项目
- `/codex where` 查看当前项目 / 线程
- `/codex workspace` 查看当前会话已记录项目和线程
- `/codex remove /绝对路径` 移除会话绑定项目
- `/codex send <相对文件路径>` 发送当前绑定项目内的文件
- `/codex switch <threadId>` 切换线程
- `/codex message` 查看最近几轮消息
- `/codex new` 新建线程
- `/codex stop` 停止当前运行
- `/codex model` / `/codex model update` / `/codex model <modelId>` 管理模型
- `/codex effort` / `/codex effort <low|medium|high|xhigh>` 管理推理强度
- `/codex approve` / `/codex reject` 审批卡片

## 安装

```sh
npm install -g codex-lark
codex-lark feishu-bot
```

开发态运行：

```sh
npm install
npm run feishu-bot
```

## 配置

项目通过 `.env`、`~/.codex-im/.env` 和当前 shell 环境变量加载配置，支持本地运行。

必填环境变量：

- `FEISHU_APP_ID`
- `FEISHU_APP_SECRET`
- `CODEX_IM_DEFAULT_CODEX_MODEL`
- `CODEX_IM_DEFAULT_CODEX_EFFORT`
- `CODEX_IM_DEFAULT_CODEX_ACCESS_MODE`

可选环境变量：

- `CODEX_IM_DEFAULT_WORKSPACE_ID`
- `CODEX_IM_FEISHU_STREAMING_OUTPUT`
- `CODEX_IM_WORKSPACE_ALLOWLIST`
- `CODEX_IM_CODEX_ENDPOINT`
- `CODEX_IM_SESSIONS_FILE`

## 版本说明

### `0.3.0`

- 将仓库正式改名为 `codex-lark`
- 保留 `codex-im` 的主代码基线，并以 `codex-lark` 作为发布名
- 重写 README、仓库元数据和发布说明
- 增加发布前的忽略规则，避免把运行日志、快照和本地配置直接提交到仓库

### `0.2.2`

- 上游 `codex-im` 的稳定版本基础
- 已包含线程绑定、卡片交互、审批、RPC、工作区管理等核心能力

## 开发历程

### 第 1 阶段：从桥接层起步

最初项目目标很明确：把 Codex 连接到飞书，让用户能在消息里直接驱动本地 Codex 会话，而不是频繁切回终端。

### 第 2 阶段：线程与工作区稳定化

随后重点处理线程绑定、线程切换、工作区状态和卡片上下文丢失问题，确保消息能稳定落到同一个线程上。

### 第 3 阶段：运行时恢复与超时处理

再往后，修复重点转向恢复机制和失败提示，包括 RPC 超时、进程重启、阻塞线程识别和可见反馈。

### 第 4 阶段：移动端交互优化

之后优化 Feishu 端的卡片长度、回复样式、线程切换提示和普通消息的展示方式，让手机端使用更顺手。

### 第 5 阶段：MCP / 审批流程适配

最后补上 MCP 工具审批、`getnote` 这类授权流和线程预览改进，让项目更适合真实日常使用。

## 迁移说明

- 这个仓库由 `codex-im` 演进而来
- 公开仓库里不包含真实 `.env`、`sessions.json`、运行日志和脱敏前快照
- 如果你要排查问题，先看 `docs/` 下的时间线和验证记录

## 参考

- 上游思路来源：`codex-im`
- 相关实现参考：Lark / Feishu 机器人、Codex 本地桥接、线程状态与审批流处理
