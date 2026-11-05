---
name: setup-sales-bot
description: Set up a new AI sales bot for a business with the Onswer tools. Use when the user wants a bot that answers customers in WhatsApp or Telegram, gives a website, Instagram, TikTok or 2GIS link and asks to build a bot from it, or asks to connect, test or fix a Onswer bot's knowledge base and channels.
---

# Set up a sales bot for a business

Answer the user in the language they write in (Russian, Kazakh, English or another). Write the bot's texts (instructions, knowledge base entries) in the language of the business's customers, and say so if you chose it.

You work through the Onswer tools (the `onswer` MCP server). There are no tools that delete bots or data; do not promise deletion.

## 1. Get the facts

Ask for what is missing, in one short message:
- a link to the business: website, Instagram, TikTok or 2GIS page;
- what the bot should do: `sell` (help buy), `book` (book appointments) or `consult` (answer and collect a lead). If the user is unsure, omit `goal` and let the reader decide;
- the language of the owner's texts (`ru`, `kk` or `en`).

Call `list_agents` first. If the business already has a bot, ask whether to improve it (go to step 3) or build another one (this counts against the plan's bot limit).

## 2. Build the bot from the link

Call `create_agent_from_link` with the `url`, and `goal`, `language` and `description` when you have them. Put extra facts the user told you (prices, hours, rules) into `description`. It takes 20 to 90 seconds; tell the user to wait.

Limits to know and to tell the user plainly if they are hit:
- the Free plan builds at most 3 bots from a link per day;
- reading links is capped at 10 per hour per account;
- the plan's bot limit applies. The error message names the plan; do not push the user to pay, just explain what is limiting.

If it fails because a page cannot be read, do not retry in a loop. Ask the user to paste the information and use `add_knowledge_text` instead.

Read the result and report it: the bot's name, how many services and knowledge entries were found, `missing` (facts the reader could not find) and `services_without_price`. Do not hide the gaps.

## 3. Review the instructions

Call `get_agent` with the `agent_id`. Read `instructions` (the system prompt), `ai_tasks`, `schedule`, `services` and `timezone`. Check against what the user told you:
- does the bot say what the business really sells, in the right tone and language;
- are the working hours and timezone right (IANA, for example `Asia/Almaty`);
- does it hand over to a human when it should.

If something should change, tell the user what and why, then call `update_agent`. Important: `instructions` REPLACES the old prompt. Send the complete new text, based on the one you just read, and change only what is needed. Send only the other fields you want to change. The old prompt is kept in the cabinet's history.

## 4. Fill the knowledge base

Call `list_knowledge` to see what exists. Then close the gaps from step 2:
- prices, delivery terms, payment, returns, address and hours, frequent questions: use `add_knowledge_text`, one topic per entry, with a short `title`, written as plain facts;
- never invent a price, a discount or a policy. If the user did not give it and the site does not have it, ask. Write only what the owner confirmed;
- set `shareable: true` only for entries the bot may send to a client as a message (for example a price list the owner is happy to share);
- a page with more detail on the same site: `add_knowledge_url` (up to 5 pages per call; counts toward the hourly link limit).

## 5. Test it

Use `send_test_message` as if you were a client. It keeps the conversation between calls, and every test message counts toward the plan's message limit, so do not spam it. Run a handful of realistic scenarios:
- a price question for a real service;
- a question that is NOT in the knowledge base (the bot should not invent an answer);
- a booking or order request;
- a rude or off-topic message;
- the same in the customers' main language.

Show the user the exact replies and your verdict. If a reply is wrong, fix the knowledge base or the instructions (steps 3 and 4) and test the same message again. If `replies` is empty, the bot is paused, the plan's message limit is reached or the account has no active plan: check `get_agent` (`bot_enabled`) and tell the user.

## 6. Connect a channel

Ask which channel the user wants first.

Telegram:
1. The owner creates a Telegram bot in @BotFather (`/newbot`) and gets a token that looks like `123456:ABC...`.
2. The token is a secret. Tell the user that pasting it into this chat gives it to the AI assistant; if they prefer, they can connect Telegram themselves in the cabinet instead. Only if they choose to paste it, call `connect_telegram` with `agent_id` and `bot_token`.
3. Never repeat the token in your messages, and do not store it anywhere.
4. Report the bot's username that the tool returns.

WhatsApp:
1. WhatsApp is linked by scanning a QR code with the business phone, which works only in the cabinet. Call `get_whatsapp_connect_link` and give the user the link.
2. Tell them: open the link, choose WhatsApp, scan the QR in WhatsApp > Linked devices.

Then call `get_channel_status` and confirm what is connected. If WhatsApp shows `QRCODE`, the scan is still pending.

## 7. Finish

Summarize in a few lines: what the bot knows, what the user still has to provide (the `missing` list), which channels are live, and that they can come back later to review analytics and real chats and improve the bot.

## Rules

- Never print or repeat API keys or bot tokens.
- Do not claim a result you did not see in a tool reply.
- Do not change a bot's settings without telling the user what you are changing.
- If a tool returns an error, show its message in plain words and say what the user can do.
