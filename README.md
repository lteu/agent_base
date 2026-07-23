# Agent_biz - ai-cli

本地 AI 命令行工具, debian 版本。

## 环境要求

| 项目 | 版本 |
|------|------|
| Node.js | >= 20.16.0 |
| 系统 | Linux / macOS |

> ⚠️ 服务器自带 Node 16，**必须先装 Node 20**，否则依赖安装失败。

---

## 部署步骤（CentOS 服务器）

### 1. 安装 Node 20

项目 `assets/` 目录已预置安装包，直接解压：

```bash
mkdir -p ~/node20
tar -xJf assets/node-v20.20.1-linux-x64-glibc-217.tar.xz -C ~/node20 --strip-components=1

echo 'export PATH=$HOME/node20/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

node -v  # 应输出 v20.20.1
```

### 2. 若配置开发机，配置 npm 镜像（内网）

```bash
npm config set registry https://npm.api.ops.qihoo.net
npm config set prefer-offline true
npm config set strict-ssl false
```

### 3. 安装依赖

```bash
cd ~/agent_biz
rm -rf node_modules dist   # 清理旧版本残留
npm ci --omit=dev
```

### 4. 构建 & 运行

```bash
npm run build        # 构建产物: dist/cli.js
npm link
ai   # 任意目录直接调用
```


---

## 模型配置

**示例：**

```bash
# GLM
ai --add-model glm model=glm-5.2 baseURL=https://llm-api-endpoint.oceanus.qihoo.net/v1 apiKey=sk-Nm_your_key


# 添加豆包
ai --add-model doubao model=doubao-seed-2.1-turbo baseURL=https://llm-api-endpoint.oceanus.qihoo.net/v1 apiKey=sk-Nm_your_key
```

### 管理模型

```bash
ai --models    # 列出所有预设（别名: --models / --model-list）
ai --use-model <名字>   # 切换到某个预设
ai --rm-model <名字>    # 删除一个预设
```

### 交互式切换

在 AI 对话中直接输入：

```
/models              # 列出预设
/models <序号|名字>   # 切换预设（立即生效）
```
---

## 微信接入

***因权限问题，Oceanus当前不支持***

支持两种方式：个人微信扫码登录（推荐）。

### 方式一：个人微信（扫码登录）

基于微信官方 ilink 机器人协议，无需服务器、无需内网穿透。

```bash
# 1. 扫码绑定个人微信
ai wx-login
# 用微信扫码确认后，机器人账号即登录

# 2. 授权白名单（未授权用户发消息会回显其 user_id）
ai --wx-allow <ilink_user_id>
```

