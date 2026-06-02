# Obsidian — визуальный редактор для wiki

Obsidian — бесплатный markdown-редактор с графом связей и wikilinks из коробки. Wiki Фабрики спроектирован как Obsidian-vault — открыл папку, и всё работает.

**Рекомендуется, не обязательно.** Wiki — это обычные `.md` файлы, любой редактор справится. Но Obsidian даёт три суперсилы: граф связей, мгновенный поиск, backlinks.

---

## Установка

1. Скачай Obsidian: [obsidian.md](https://obsidian.md). Бесплатно для личного использования.
2. Открой → «Open folder as vault» → выбери папку `wiki/` корня Фабрики.
3. Готово. Граф связей появится через минуту индексации.

---

## Рекомендуемые настройки

### Файлы и ссылки

`Settings → Files and links`:
- **New link format:** `Shortest path when possible` (короткие wikilinks)
- **Use [[wikilinks]]:** ON (обязательно)
- **Default location for new notes:** `In the root` (плоская структура)
- **Default location for new attachments:** `In the folder specified below` → `_attachments/` (картинки не загрязняют корень)

### Editor

`Settings → Editor`:
- **Default view:** `Source mode` (видишь markdown как есть, без markdown-prettify)
- **Show line numbers:** OFF (мешает)
- **Strict line breaks:** OFF (стандартное markdown-поведение)

### Files

`Settings → Files`:
- **Always update internal links:** ON (при переименовании файла все ссылки автообновятся)

---

## Полезные плагины (Community)

Включи `Settings → Community plugins → Browse`:

1. **Templater** — расширенные шаблоны из `_templates/` с переменными (дата, имя файла, frontmatter).
2. **Dataview** — SQL-подобные запросы к frontmatter. Пример: «покажи все `feedback_*` со `status: active`, сортируй по `updated`».
3. **Tag Wrangler** — управление тегами, переименование, поиск.
4. **Excalidraw** (опционально) — рисованные схемы прямо в заметке. Удобно для диаграмм процесса.
5. **Advanced URI** — открыть конкретную заметку по ссылке из терминала/Claude.

---

## Граф связей

`Settings → Core plugins → Graph view: ON`. Открой через иконку графа (cmd/ctrl+G).

Что увидишь:
- Каждая заметка — узел.
- Каждый `[[wikilink]]` — ребро.
- `MEMORY.md` в центре (на нём больше всего связей).

Что искать:
- **Кластеры** — группы заметок одной темы (бренд, аудитория, продукты).
- **Изолированные узлы** — orphan-файлы без ссылок. Подключи их к чему-то или удали.
- **Хабы** — заметки с большим числом связей. Это твои якорные знания, держи их свежими.

Полезные фильтры в графе:
- `path:wiki/_setup` — скрыть служебные.
- `tag:#archived` — скрыть архив.
- Color groups: разные цвета для `feedback_*`, `decision_*`, `project_*`.

---

## Workflow

### Создание заметки

1. `Cmd+N` (новая) → имя по конвенции: `feedback_objection-style.md`
2. `Cmd+P` → `Templater: Insert template` → выбери `feedback`
3. Заполни frontmatter, напиши содержание
4. Свяжи с другими заметками через `[[...]]`
5. Открой `MEMORY.md`, добавь ссылку в нужную секцию

### Поиск

- **Cmd+O** — открыть файл по имени. Печатай — fuzzy search.
- **Cmd+Shift+F** — поиск по содержимому всех файлов.
- **Cmd+P** → `Quick switcher` — переключение между открытыми.
- **Cmd+G** — открыть граф связей.

### Backlinks

В нижней панели каждой заметки — `Backlinks` секция. Показывает все заметки, которые ссылаются сюда через `[[wikilink]]`. Бесценно для контекста: «где я уже использовал это решение».

---

## Mobile sync

Бесплатно: Obsidian работает с iCloud Drive (Mac/iOS), Syncthing (Linux/Android), любым облаком, которое синкает папку.

Платно: Obsidian Sync ($8/мес) — официальная синхронизация с шифрованием. Не обязательно.

---

## Что НЕ делать в Obsidian

- **Не редактируй `MEMORY.md` напрямую через GUI без понимания структуры.** Используй `wiki-keeper` агента или правки секций.
- **Не создавай папки для типов** (`feedback/`, `projects/`) — конвенция плоская.
- **Не используй мышь для wikilinks.** `[[` → автокомплит → Tab → быстрее.
- **Не игнорируй orphan-warning.** Если файл не упомянут в `MEMORY.md` — он потерян для агентов.
