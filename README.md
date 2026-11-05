# Onswer Sales Bot for Claude Code

English | [Русский](#русский)

Onswer is an AI sales assistant for small and medium businesses. It answers customers in WhatsApp and Telegram, books appointments, collects leads into amoCRM and Bitrix24, follows up, and handles voice calls. This plugin connects Claude Code to your Onswer account and teaches Claude how to set a bot up and improve it.

## What it does

- Connects Claude to the Onswer MCP server (`https://api.onswer.ai/mcp`, Streamable HTTP) with 13 tools: list and read bots, create a bot from a website, Instagram, TikTok or 2GIS link, update settings, manage the knowledge base, send test messages, connect Telegram, link WhatsApp, check channels, read analytics and recent chats.
- Adds two skills:
  - `/onswer-sales-bot:setup-sales-bot`: read the business link, review the bot's instructions, add prices and FAQ to the knowledge base, test with test messages, then connect Telegram or WhatsApp.
  - `/onswer-sales-bot:review-bot-performance`: read analytics and recent chats, find knowledge gaps and weak replies, fix them and verify.
- Claude answers in the language you write in.
- There are no tools that delete bots or data.

## Install

```bash
claude plugin marketplace add AlanC8/onswer
claude plugin install onswer-sales-bot@onswer-marketplace
```

Or inside Claude Code: `/plugin install onswer-sales-bot --marketplace AlanC8/onswer`.

## Sign in

You need a Onswer account. The Free plan can build up to 3 bots from a link per day.

**Browser sign-in (OAuth).** After installing, run `/mcp`, choose `onswer` and **Authenticate** (or `claude mcp login onswer`). The Onswer page opens: sign in or create an account, then press **Allow**. No key to copy. Connected apps are listed, and can be disconnected, in the dashboard under Profile > AI assistants.

**API key instead** (CI, shared machines): create a key like `wv_sk_...` in Profile > AI assistants, then either use `variants/env-var.mcp.json` as `.mcp.json` with `export ONSWER_API_KEY="wv_sk_..."`, or skip the plugin:

```bash
claude mcp add --transport http onswer https://api.onswer.ai/mcp --header "Authorization: Bearer $ONSWER_API_KEY"
```

## Other apps

The server is a standard remote MCP server with OAuth sign-in: `https://api.onswer.ai/mcp`.

- **claude.ai / Claude Desktop:** Settings > Connectors > Add custom connector, URL above, then Connect.
- **ChatGPT:** Settings > Apps (developer mode) > Create, URL above, authentication OAuth.
- **Cursor:** add to `~/.cursor/mcp.json`: `{"mcpServers": {"onswer": {"url": "https://api.onswer.ai/mcp"}}}`.
- **Codex CLI:** `codex mcp add onswer --url https://api.onswer.ai/mcp`, then `codex mcp login onswer`.

Each one opens the Onswer sign-in page in the browser the first time.

## Use

Ask Claude, for example:

- "Set up a sales bot for my coffee shop: https://example.com, connect Telegram."
- "How did my bot do over the last 30 days? What did it fail to answer?"

## Privacy and support

The plugin itself stores nothing. Claude reads and changes your Onswer account through the tools above; recent chats include your customers' messages. Privacy policy: https://onswer.ai/privacy. Terms: https://onswer.ai/terms. Docs: https://onswer.ai/docs/developers/mcp. Support: support@maxx.kz.

License: MIT.

---

## Русский

Onswer: ИИ-помощник по продажам для малого и среднего бизнеса. Отвечает клиентам в WhatsApp и Telegram, записывает на приём, собирает заявки в amoCRM и Bitrix24, делает дожим и принимает звонки. Этот плагин подключает Claude Code к вашему аккаунту Onswer и учит Claude настраивать бота и улучшать его.

### Что делает

- Подключает Claude к MCP-серверу Onswer (`https://api.onswer.ai/mcp`, Streamable HTTP), 13 инструментов: список и чтение ботов, создание бота по ссылке на сайт, Instagram, TikTok или 2ГИС, изменение настроек, база знаний, тестовые сообщения, подключение Telegram, ссылка для WhatsApp, статус каналов, аналитика и последние чаты.
- Добавляет два навыка:
  - `/onswer-sales-bot:setup-sales-bot`: прочитать ссылку на бизнес, проверить инструкции бота, добавить цены и частые вопросы в базу знаний, проверить тестовыми сообщениями, затем подключить Telegram или WhatsApp.
  - `/onswer-sales-bot:review-bot-performance`: разобрать аналитику и последние чаты, найти пробелы в знаниях и слабые ответы, исправить и проверить.
- Claude отвечает на том языке, на котором вы пишете.
- Инструментов для удаления ботов и данных нет.

### Установка

```bash
claude plugin marketplace add AlanC8/onswer
claude plugin install onswer-sales-bot@onswer-marketplace
```

Или внутри Claude Code: `/plugin install onswer-sales-bot --marketplace AlanC8/onswer`.

### Вход

Нужен аккаунт Onswer. На тарифе Free можно создать не больше 3 ботов по ссылке в сутки.

**Вход через браузер (OAuth).** После установки наберите `/mcp`, выберите `onswer` и **Authenticate** (или `claude mcp login onswer`). Откроется страница Onswer: войдите или зарегистрируйтесь и нажмите **«Разрешить»**. Копировать ключ не нужно. Подключённые приложения видны и отключаются в кабинете: «Профиль → ИИ-помощники».

**Вместо этого API-ключ** (CI, общие машины): создайте ключ вида `wv_sk_...` в «Профиль → ИИ-помощники», затем либо возьмите `variants/env-var.mcp.json` как `.mcp.json` и выполните `export ONSWER_API_KEY="wv_sk_..."`, либо обойдитесь без плагина:

```bash
claude mcp add --transport http onswer https://api.onswer.ai/mcp --header "Authorization: Bearer $ONSWER_API_KEY"
```

### Другие приложения

Сервер обычный удалённый MCP с входом через OAuth: `https://api.onswer.ai/mcp`.

- **claude.ai / Claude Desktop:** «Настройки → Коннекторы → Добавить свой коннектор», адрес выше, «Подключить».
- **ChatGPT:** «Настройки → Приложения» (режим разработчика) → «Создать», адрес выше, авторизация OAuth.
- **Cursor:** в `~/.cursor/mcp.json`: `{"mcpServers": {"onswer": {"url": "https://api.onswer.ai/mcp"}}}`.
- **Codex CLI:** `codex mcp add onswer --url https://api.onswer.ai/mcp`, затем `codex mcp login onswer`.

При первом подключении каждое из них откроет в браузере страницу входа Onswer.

### Примеры запросов

- «Настрой бота для моей кофейни: https://example.com, подключи Telegram».
- «Как работал мой бот за 30 дней? На что он не смог ответить?»

### Данные и поддержка

Сам плагин ничего не хранит. Claude читает и меняет ваш аккаунт Onswer через перечисленные инструменты; последние чаты содержат сообщения ваших клиентов. Политика конфиденциальности: https://onswer.ai/privacy. Условия: https://onswer.ai/terms. Документация: https://onswer.ai/docs/developers/mcp. Поддержка: support@maxx.kz.

Лицензия: MIT.
