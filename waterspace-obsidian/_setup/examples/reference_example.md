---
name: Telegram Bot API креды
description: Токены и chat_id для рабочего канала и админ-группы Анны
type: reference
status: active
audience: internal
confidence: high
updated: 2026-05-13
tags: [telegram, ops]
---

# Telegram Bot API — credentials

> Образец `reference_*.md`. Удали этот файл при первом запуске или замени своими ссылками/ресурсами.

## Bot

- **Bot username:** `@anna_fitness_bot`
- **Bot name:** Anna Fitness Coach
- **Token:** хранится в env `TG_BOT_TOKEN` (не записывать сюда явно)
- **Webhook:** `https://anna.example.com/tg-webhook`

## Каналы и чаты

| Где | chat_id | Тип | Использование |
|---|---|---|---|
| Публичный канал | `-100xxxxxxxxxx` | channel | основные посты, репосты Reels |
| VIP-группа | `-100yyyyyyyyyy` | supergroup | клиенты годовой программы |
| Админ-чат | `-100zzzzzzzzzz` | group | алерты, уведомления о платежах |
| Личка Анны | `123456789` | user | отправка важных нотификаций |

## Ограничения API

- Posting rate: 20 msg/min на чат, 30/sec глобально.
- Media size: ≤50 MB (фото), ≤2 GB (видео через `sendVideo`).
- Edit window: 48 часов после публикации.

## Связанные сервисы

- Платёжный бот: [[reference_payment-bot]] (отдельный токен)
- Бот рассылок: [[reference_broadcast-bot]] (через Telethon, не Bot API)

## История изменений

- 2026-05-13: создано
- 2026-04-12: добавлен VIP-чат после запуска годовой программы
- 2026-02-01: миграция с long polling на webhook

## Источники

- [Telegram Bot API docs](https://core.telegram.org/bots/api)
- Внутренняя страница: `[[project_anna-fitness]]`
