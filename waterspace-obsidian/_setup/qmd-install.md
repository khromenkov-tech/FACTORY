# qmd — семантический поиск по wiki

qmd — это MCP-сервер для семантического поиска через embeddings. С ним любой агент Фабрики может найти нужную заметку по смыслу, а не по точным словам.

**Рекомендуется, не обязательно.** Без qmd работает fallback на `grep` по содержимому файлов.

---

## Что даёт

- Запрос «как мы решили работать с возражениями» → находит `[[decision_objections-flow]]` и `[[feedback_objection-tone]]`, даже если этих слов в файлах нет.
- Запрос «голос для здорового образа жизни» → находит `[[feedback_voice-health]]`, `[[project_anna-fitness]]`, `[[reference_health-style-guide]]`.
- Связи между заметками через смысл, а не только wikilinks.

---

## Установка (macOS + Linux)

### 1. Проверь Python 3.10+

```bash
python3 --version
```

Если меньше — поставь через `brew install python@3.11` (Mac) или пакетный менеджер (Linux).

### 2. Установи qmd

```bash
pip install --user qmd-mcp
```

Или через pipx:
```bash
pipx install qmd-mcp
```

### 3. Подключи как MCP сервер в Claude Code

Открой `~/.claude/settings.json` (или `~/.config/claude/settings.json` на Linux) и добавь:

```json
{
  "mcpServers": {
    "qmd": {
      "command": "qmd-mcp",
      "args": [
        "--root", "/абсолютный/путь/к/корню/Фабрики",
        "--collection", "wiki"
      ]
    }
  }
}
```

Замени путь на абсолютный путь к корню установки (где лежат `wiki/`, `projects/`, `.claude/`).

### 4. Первый индекс

```bash
cd /путь/к/корню/Фабрики
qmd init
qmd embed
```

`init` создаёт служебную папку `.qmd/`, `embed` строит embeddings для всех файлов wiki.

### 5. Проверь

В новой сессии Claude спроси: «найди мне записи про голос бренда». Если qmd подключён — модель будет искать через MCP-tool `qmd_search` или `qmd_vsearch`.

---

## Auto-update

Добавь Stop hook, чтобы qmd индекс обновлялся после каждой сессии:

`.claude/hooks/stop.sh`:
```bash
#!/bin/bash
cd "$CLAUDE_PROJECT_DIR" || exit 0
qmd update --collection wiki --quiet
qmd embed --collection wiki --quiet --changed-only
```

Не забудь `chmod +x .claude/hooks/stop.sh`.

---

## Затраты

- **Storage:** ~5 MB на 100 заметок (embeddings).
- **CPU:** первичный `embed` — 10-30 сек на 100 заметок. Инкрементальный — секунды.
- **Без сети:** работает локально через `sentence-transformers`. API-ключи не нужны.

Если хочешь outsource на API (Voyage, OpenAI) — есть флаг `--provider voyage`, но локальная модель `all-MiniLM-L6-v2` справляется с wiki из тысяч заметок.

---

## Fallback без qmd

Если qmd не ставится (Windows без WSL, корпоративная политика) — `wiki-keeper` использует `grep -ri` по содержимому. Это медленнее и менее точно, но рабоче.

Конвенции (префиксы `feedback_`, `decision_`) и обязательные wikilinks делают grep эффективным.

---

## Troubleshooting

**`qmd: command not found`** → проверь `~/.local/bin` в `PATH` (или путь от pipx).

**MCP не появляется в Claude** → перезапусти Claude Code, проверь `claude mcp list`.

**Поиск не находит свежие заметки** → запусти `qmd update` вручную, или проверь Stop hook.

**Большие файлы тормозят** → разбей на несколько заметок по теме. Wiki не любит файлы >2000 строк.
