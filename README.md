# Twake-Chat Matrix 扩展服务器

<br />
<div align="center">
  <a href="https://github.com/linagora/twake-on-matrix">
    <img src="https://github.com/artembru/ToM-server/assets/146178981/4a5da817-466f-4d4a-8804-3881b672bc42">
  </a>

  <p align="center">
    <a href="https://twake-chat.com">网站</a>
    •
    <a href="https://beta.twake.app/web/#/rooms">查看演示</a>
    •
    <a href="https://github.com/linagora/twake-on-matrix/issues">报告错误</a>
    •
    <a href="https://hosted.weblate.org/projects/linagora/twake-matrix/#repository">翻译 Twake</a>
</p>
</div>

---

这个仓库是一个多包仓库。详情请参见 [模块](#模块)。

**ToM 服务器** 为 [Matrix Synapse 服务器](https://github.com/element-hq/synapse) 增强了以下功能:
 * 首先，**Tom** 是一个 [Matrix 身份服务器](https://spec.matrix.org/latest/identity-service-api/)，但具有额外功能：
   * 在组织内部，它添加了一些搜索 API，允许像邮件客户端一样查找内部用户，例如用于自动补全
   * 它还扩展了 [Matrix 身份服务](https://spec.matrix.org/latest/identity-service-api/) 搜索响应，添加了非活跃用户
 * 它还提供了一个"应用服务"，允许管理员创建自动加入的频道
 * 它还实现了 [联邦身份机制](https://github.com/matrix-org/matrix-spec-proposals/pull/4004)，扩展了
   [Matrix 身份服务](https://spec.matrix.org/latest/identity-service-api/) 以连接 Matrix 身份服务，提供更好的搜索

这是架构原理：

![架构原理](./docs/arch.png)

REST API 端点文档可在 https://linagora.github.io/ToM-server/ 查看

## 亲自试用

- [运行我们的 Docker](./docker.md)
- [使用 compose 本地部署](./docker.md#docker-compose)

## 模块

* [@twake/matrix-identity-server](./packages/matrix-identity-server):
  Node.js 的 [Matrix 身份服务](https://spec.matrix.org/v1.6/identity-service-api/) 实现
* [@twake/matrix-client-server](./packages/matrix-client-server/):
  Node.js 的 [Matrix 客户端-服务器](https://spec.matrix.org/v1.11/client-server-api/) 实现
* [@twake/matrix-invite](./packages/matrix-invite): matrix 邀请网络应用
* [@twake/server](./packages/tom-server): 主 Twake 聊天服务器，扩展 [@twake/matrix-identity-server](./packages/matrix-identity-server)
* [@twake/federated-identity-service](./packages/federated-identity-service): Twake 联邦身份服务
* [@twake/config-parser](./packages/config-parser): 使用环境变量的简单文件解析器
* [@twake/crypto](./packages/crypto): Twake 聊天的加密方法
* [@twake/logger](./packages/logger): Twake 的日志记录器
* [@twake/utils](./packages/utils): Twake 聊天的实用方法
* [@twake/matrix-application-server](./packages/matrix-application-server): 实现
  [Matrix 应用服务 API](https://spec.matrix.org/v1.6/application-service-api/)
* [matrix-resolve](./packages/matrix-resolve): 根据
  [Matrix 规范](https://spec.matrix.org/latest/server-server-api/#server-discovery) 将 Matrix "服务器名称" 解析为基本 URL
* [@twake/retry-promise](./packages/retry-promise): 使用重试策略扩展 JavaScript Promise 的简单模块

## 系统要求

- [ ] Node >=18

## 命令

* `npm run build`: 构建所有包
* `npm run test`: 测试所有包
* `node ./server.mjs`: 运行服务器

## 开发环境设置

按照以下步骤在开发模式下启动项目：

### 1. 复制环境文件

基于提供的示例创建本地 `.env` 文件：

```bash
cp .env.example .env
```

您可以根据需要调整 `.env` 中的任何变量（例如数据库凭证、API 密钥等）。

### 2. 启动所需服务

使用提供的 Docker Compose 文件启动本地依赖项（PostgreSQL、LDAP 等）：

```bash
docker compose -f .compose/examples/dev.pgsql+ldap.yml up -d
```

这将在后台运行所有必要的后端服务。
稍后停止它们：

```bash
docker compose -f .compose/examples/dev.pgsql+ldap.yml down
```

### 3. 运行开发环境

启动本地开发环境（监听器 + 服务器自动重载）：

```bash
npm run dev
```

这将：

* 自动监听和重建所有包 (`lerna run watch`)
* 通过 `nodemon` 启动后端服务器
* 自动从 `.env` 加载环境变量

### 4. 访问和调试

启动后：

* 服务器应在控制台打印的 URL 上运行（例如 `http://localhost:3000`）
* `packages/` 中的任何代码更改都将触发自动重建和服务器重启

## 版权和许可证

Copyright (c) 2023-present Linagora <https://linagora.com>

许可证: [GNU AFFERO GENERAL PUBLIC LICENSE](./LICENSE)