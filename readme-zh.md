# rss to bot

[![send feeds to bot](https://github.com/hyoban/rss-to-bot/actions/workflows/main.yml/badge.svg)](https://github.com/hyoban/rss-to-bot/actions/workflows/main.yml)

使用 Github Action 来将你的 RSS 订阅发送到 Telegram 的机器人上.

## 如何使用

1. 点击 `Use this template` 生成一份你自己的仓库
2. 复制替换 `feeds.opml` 文件中的 RSS 订阅, 你可以从其他阅读器导出订阅
3. `Settings->Secrets->Actions->New repository secrets` 分别创建:
    1. Telegram 的机器人 token `TG_TOKEN`
    2. 你与该机器人对话的 id `TG_CHAT_ID`
    3. 你所处的时区 `TIMEZONE`. 比如 `Asia/Shanghai`
    4. 用于存储发送记录的 `GIST_ID`
    5. `GIST_TOKEN` 用于更新发送记录的 gist，[生成一个](https://github.com/settings/tokens/new?scopes=gist)

## 常见问题

### 如何获取 `TG_CHAT_ID`

可以在本地运行如下代码, 然后发送 `/start` 给机器人.

```js
const TelegramBot = require('node-telegram-bot-api')

const token = 'xxxxx'
const bot = new TelegramBot(token, { polling: true })
let chatId = null

bot.onText(/\/start/, (msg) => {
  chatId = msg.chat.id
  console.log('chatId:', chatId)
  bot.sendMessage(chatId, chatId)
})
```

### 如何修改发送订阅的频率

编辑 `.github/workflows/main.yml` 文件中 `- cron: '0 7,12,17,22 * * *'` 内容, 默认的含义是每天 7,12,17,22 点会执行.


### 如何管理订阅

手动编辑订阅文件, 或者使用 [opml-file-manager](./packages/opml-file-manager/README.md)
