---
name: review-bot-performance
description: Review how a Onswer sales bot is doing and improve it. Use when the user asks how their bot performs, wants analytics for the last 7, 30 or 90 days, wants to look at recent customer conversations, find questions the bot could not answer, or fix wrong or weak bot replies.
---

# Review a sales bot's results and improve it

Answer the user in the language they write in (Russian, Kazakh, English or another).

You work through the Onswer tools (the `onswer` MCP server). Recent chats contain real customers' messages and names or phone numbers. Treat them as private: use them to find patterns and fix the bot, quote only the short fragments you need, and do not repeat full phone numbers or personal details back unless the owner asks for a specific customer.

## 1. Pick the bot and the period

Call `list_agents`. If there are several bots, ask which one (or use all of them for the summary). Default period: 30 days. Ask only if the user hinted at another one: the tool accepts 7, 30 or 90.

## 2. Read the numbers

Call `get_analytics_summary` with `days` and, for one bot, `agent_id`. If it says analytics is not included in the plan, say so and go straight to step 3: the recent chats still work.

Report briefly and honestly, comparing with `previous_period_totals`:
- conversations, leads, agreed deals, `conversion_percent`;
- response time and the `quality` block;
- which channels bring the conversations (`channels`);
- `waiting_customers`: customers who are waiting for an answer or a human right now. List them first if there are any, with how long they have waited and the reason. Tell the owner these need a person.

Do not invent causes for a change. If a number moved, say that and look at the chats before explaining why.

## 3. Read real conversations

Call `list_recent_chats` for the bot, with `limit` 10 to 20 and `messages` 4 to 8. Look for:
- questions the bot answered badly, vaguely or wrongly;
- places where the customer left or went silent after a bot reply;
- chats flagged `needs_human` or `paused`;
- repeated questions that the knowledge base should answer;
- tone problems: too long, too pushy, wrong language.

Group what you find into a few concrete findings, each with a short example and what to do about it. Say how many chats you looked at: ten chats are a sample, not proof.

## 4. Find knowledge gaps

The summary's `knowledge_gaps` lists questions the bot could not answer. Call `list_knowledge` for the bot to see what is already there, then for each real gap:
1. Ask the owner for the correct answer if you do not have it. Never make up prices, terms or policies.
2. Add it with `add_knowledge_text`: one topic per entry, a short `title`, facts only.

## 5. Fix, then verify

Propose the changes to the user before making them: new knowledge entries, and changes to the instructions. For instructions call `get_agent` first, then `update_agent` with the COMPLETE new text (it replaces the old prompt; the old one stays in the cabinet's history), changing only what the findings justify.

After each change, replay the problem with `send_test_message` using the customer's real question and show the new reply. Test messages count toward the plan's message limit: replay the important cases, not all of them.

## 6. Report

End with:
- 3 to 5 findings in order of impact;
- what you changed and what you checked;
- what is left for the owner (customers waiting, facts you need from them);
- when to check again (for example in a week, after the new knowledge has met real customers).

## Rules

- No tool deletes anything; do not offer to delete chats or bots.
- Do not report an improvement you have not seen: a good test reply is not a better conversion rate.
- If a tool returns an error, show its message in plain words and say what the user can do.
