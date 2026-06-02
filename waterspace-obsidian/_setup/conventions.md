# Conventions — детальные правила wiki

Правила имён файлов, frontmatter, wikilinks и работы с индексом. Эти конвенции делают wiki работоспособным для агентов, графа и семантического поиска одновременно.

---

## Имена файлов

**Формат:** `<type>_<topic-kebab>.md`

| Префикс | Когда | Примеры |
|---|---|---|
| `feedback_` | Правило, наученное на ошибке/успехе | `feedback_no-emoji-in-bof.md`, `feedback_dash-killer.md` |
| `reference_` | Указатель на внешний ресурс | `reference_notion-templates.md`, `reference_brand-colors.md` |
| `project_` | Состояние проекта/инициативы | `project_anna-fitness.md`, `project_launch-q3.md` |
| `decision_` | Архитектурное решение | `decision_voice-flat.md`, `decision_pricing-tiers.md` |
| `user_` | Профиль клиента | `user_role.md`, `user_workstyle.md` |

**Правила:**
- Только lowercase, дефисы вместо пробелов.
- Кириллица в именах НЕЛЬЗЯ (поломается на некоторых ФС, поисках, экспортах).
- Тема в имени — короткая (2-4 слова), полная — в `name:` frontmatter.
- Один файл — одна тема. Не комбинируй.

**Anti-patterns:**
- ❌ `My Feedback About Voice.md` — пробелы, заглавные, без префикса
- ❌ `feedback_voice.md` (слишком общо — какой voice?)
- ❌ `project_anna.md` (Anna — кто?) → `project_anna-fitness.md`
- ❌ `decision-pricing.md` (тире вместо подчёркивания после префикса)

---

## Frontmatter

### Обязательные поля

```yaml
---
name: Краткое имя на русском
description: Одна строка для индекса MEMORY.md (≤120 знаков)
type: feedback | reference | project | decision | user
status: draft | active | stale | archived
updated: 2026-05-13
---
```

### Опциональные — добавляй по необходимости

```yaml
audience: client | internal
confidence: high | medium | low
project: <slug или general>
tags: [voice, threads]
sources:
  - URL
  - [[wikilink]]
related:
  - [[feedback_other]]
  - [[decision_y]]
```

### Расширения для контентных правил

Если заметка участвует в генерации контента (формула, anti-pattern, swipe):

```yaml
content_intent: thesis | rule | swipe | template | research | objection
audience_segment: <сегмент-аудитории>
funnel_stage: tof | mof | bof | n/a
```

### Что НЕ добавлять

- `doc_type` — дублирует `type` + префикс
- `created_at` — wiki не журнал, нужен только `updated`
- `author` — wiki одного клиента, понятно кто автор
- `version` — wiki не код

---

## Wikilinks

**Обязательны для всех связей внутри wiki.**

```markdown
✅ См. [[decision_voice-flat]] — почему плоская структура
✅ [[feedback_dash-killer|Длинное тире = AI-маркер]]
✅ Аватар описан в [[project_anna-fitness]]

❌ См. [decision_voice-flat](decision_voice-flat.md)
❌ См. файл `decision_voice-flat.md`
❌ См. документ про voice (без ссылки)
```

**Pipe syntax** `[[имя-файла|Отображаемый текст]]` — когда хочешь, чтобы в тексте было читаемое название, а ссылка вела на конкретный файл.

**Ссылки на секции:** `[[MEMORY#Decisions]]` ведёт на якорь.

**Ссылки на блоки:** `[[file#^block-id]]` — продвинутая фича Obsidian, используй редко.

---

## MEMORY.md

Главный индекс. Каждая заметка должна быть упомянута минимум одной ссылкой.

### Структура

```markdown
## Тематический раздел

> Опциональное описание раздела

- [[feedback_topic|Заголовок]] — описание из frontmatter
- [[decision_x|Title]] — короткое summary
```

### Правила

- Сортировка внутри секции — **по релевантности**, не по алфавиту. Важное — выше.
- Описание после `—` — короткое, до 100 знаков.
- Pipe-syntax обязателен для читаемых названий, если имя файла не русское.
- Заметки со `status: archived` — в секции `## Archive`.
- Orphan-файлы (свежие, без ссылки) — в секции `## Inbox (auto-added)`. `wiki-keeper` поднимает их туда автоматически.

### Anti-patterns

- ❌ Длинный плоский список без секций — теряется навигация.
- ❌ Алфавитная сортировка — важное прячется в середине.
- ❌ Markdown-ссылки `[Title](file.md)` — граф связей в Obsidian не построится.
- ❌ Заметка не упомянута в MEMORY.md — она orphan, агенты её не найдут.

---

## Связи между заметками

В теле каждой содержательной заметки полезно дать раздел `## Связи`:

```markdown
## Связи
- Усиливает: [[feedback_no-cta]]
- Конфликтует с: [[decision_old-cta-rules]] (deprecated)
- Применяется в: [[project_anna-fitness]], [[project_max-content]]
```

Это даёт двунаправленные связи для Obsidian backlinks и явное описание для агентов.

---

## Обновление существующих заметок

1. Не меняй имя файла без необходимости — все wikilinks сломаются (Obsidian обновит, но коммиты увидят diff).
2. Меняешь смысл — обнови `updated:` в frontmatter.
3. Заметка устарела, но факты ещё могут пригодиться — `status: stale`. Полностью неактуальна — `status: archived` + перенеси ссылку в раздел `## Archive` в `MEMORY.md`.
4. Удаление — крайняя мера. История полезнее пустоты.

---

## Inbox workflow

`wiki/inbox/` — папка для:
- Заметок из Telegram (auto-import через хук)
- Черновиков, которые ещё не оформлены по конвенции
- Скриншотов и swipe-материала до классификации

Раз в неделю запускай `wiki-keeper` агента (или команду `/inbox`) — он переименует, добавит frontmatter, поднимет в правильную секцию `MEMORY.md`.

Inbox — единственная папка кроме `_setup/`, `_templates/`, `_attachments/`. Не плоди новые.

---

## Размер заметки

- **Норма:** 30-200 строк.
- **Большая:** 200-500 строк. Рассмотри разбиение на несколько связанных.
- **Слишком большая:** >500 строк. Точно разбей. Граф в Obsidian теряет ценность, qmd-индексация хуже.
- **Слишком маленькая:** <10 строк. Объедини с родственной или удали.

---

## Backup

Wiki — это многомесячный труд. Перед массовыми изменениями:

```bash
tar czf ~/wiki-backup-$(date +%Y%m%d).tar.gz -C /путь/к/Фабрике wiki
```

Идеально — git-репо для wiki (даже локальное). Тогда `git log` покажет историю мыслей, а не только текущее состояние.
