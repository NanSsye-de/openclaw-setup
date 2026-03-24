# xbot 重磅更新：支持微信官方机器人通道（OpenClaw/ilink），WechatAPI 备用方案上线

**作者：老夏的金库**

## 一、背景说明

xbot 机器人一直依赖第三方 WechatAPI 接口实现微信消息的收发。然而此类接口存在不稳定、服务中断等问题，给使用者带来困扰。

从 v2.0 起，xbot 引入了 **OpenClaw/ilink 微信官方机器人通道** 作为过渡方案，在 WechatAPI 不可用时自动切换到微信官方机器人接口，保证服务持续运行。

## 二、全新支持：OpenClaw 过渡通道

### 适用场景

- WechatAPI 服务不可用或被封禁
- 需要更稳定的微信消息收发
- 企业或个人需要合规的微信机器人方案

### 工作原理

系统通过 `ilink/bot/getupdates` 长轮询方式与微信官方服务器通信，所有消息经由 OpenClaw 路由，无需依赖第三方 WechatAPI 接口。

## 三、配置方法（Docker 环境）

### 第一步：修改配置文件

在 `main_config.toml` 中设置：

```toml
[Framework]
type = "wechat"
channel = "openclaw"

[OpenClaw]
base-url = "https://ilinkai.weixin.qq.com"
bot-type = "3"
credentials-file = "resource/openclaw_account.json"
message-mode = "http"
```

### 第二步：启动容器

```bash
docker compose up -d --build
```

### 第三步：扫码授权

首次启动后，后台状态会提示「等待微信扫码登录」。二维码链接会自动写入 `admin/bot_status.json`，用微信扫码确认即可。凭据保存到 `resource/openclaw_account.json`，后续重启自动复用。

## 四、与旧通道对比

| 对比项 | 旧 WechatAPI 通道 | OpenClaw 过渡通道 |
|--------|----------------|----------------|
| 依赖服务 | 第三方 WechatAPI | 微信官方服务器 |
| 稳定性 | 受第三方影响 | 更稳定 |
| 消息能力 | 图片/语音等高级能力 | 优先文本消息，高级能力逐步完善 |
| 配置难度 | 需启动 PAD 服务 | 仅需扫码授权 |

## 五、关于系统通知推送

系统内置了通知推送功能，支持通过企业微信（CP）、微信公众号（Wechat）、短信、邮件等多种渠道发送系统通知（离线提醒、重连通知、重启通知、错误告警等）。

## 六、项目地址

GitHub：https://github.com/NanSsye/xbot

交流群：扫码 GitHub 主页右侧二维码
