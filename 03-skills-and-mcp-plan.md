# План skills и MCP-мостов

## Принцип

Skill должен решать одну повторяемую задачу и загружать только нужные инструкции. MCP должен давать доступ к инструменту или данным, которые нельзя надёжно передать обычным файлом. Не подключайте все MCP одновременно: описания инструментов занимают контекст, а лишние разрешения повышают риск ошибочного действия.

Новые project skills хранить в `.devin/skills/<name>/SKILL.md`. Общую логику следует переносить между Cursor и OpenCode через обычные Markdown-инструкции и шаблоны, а client-specific конфигурацию держать отдельно.

## Обязательные skills

### 1. `ctf-task-triage`

Назначение:

- разобрать условие и вложения;
- определить категорию и предполагаемые техники;
- найти аналоги в `context.md`;
- предложить три дешёвые проверки;
- оценить риск и время;
- создать карточку задачи.

Ограничения:

- read-only;
- не запускать неизвестные бинарники;
- не делать сетевые запросы без явного адреса из задания.

### 2. `ctf-web-audit`

Проверяет:

- route, middleware, auth и session;
- proxy/backend parser differences;
- SSRF, CRLF, SQL/NoSQL injection, SSTI, deserialization;
- CSP, browser bot и HTML/PDF renderer;
- Docker Compose topology, internal hostnames и datastore.

Результат: таблица `source → sink → protection → bypass hypothesis → verification`.

### 3. `ctf-reverse-workbench`

Выполняет:

- inventory бинарника;
- определение architecture, format, symbols, packer и mitigations;
- план static/dynamic анализа;
- извлечение constants/strings;
- описание custom VM или transformation;
- генерацию небольших decoder/emulator-скриптов.

Skill не должен считать декомпиляцию доказательством. Он обязан сверять вывод с runtime behavior.

### 4. `ctf-pwn-hypothesis`

Формирует:

- protections и attack surface;
- crash reproduction;
- primitive: leak/read/write/control flow;
- ограничения allocator, sandbox и protocol;
- exploit stages;
- локальную и remote-проверку.

Добавить обязательную самопроверку offset, architecture, endianness, libc и repeatability.

### 5. `ctf-crypto-model`

Skill сначала записывает математическую модель:

- известные и неизвестные значения;
- primitive и параметры;
- oracle;
- повторяющиеся nonce/key/seed;
- требуемая сложность;
- маленький тестовый пример.

Только после этого он пишет solver. Это снижает количество бессмысленных brute-force скриптов.

### 6. `ctf-forensics-pipeline`

Строит воспроизводимый pipeline:

- hash и размер исходного файла;
- определение формата;
- безопасное извлечение;
- timeline и IOC;
- память, Registry, PCAP, firmware или filesystem;
- журнал команд и полученных артефактов.

Оригинальный файл остаётся неизменным. Skill работает с копией.

### 7. `ctf-exploit-review`

Независимая проверка перед сдачей:

- соответствует ли exploit заявленной root cause;
- нет ли hardcoded локальных значений;
- устойчив ли parser/protocol;
- воспроизводится ли решение с чистого запуска;
- минимальны ли зависимости;
- не попали ли flag, token или secret в Git/AI chat.

### 8. `ctf-context-pack`

Сжимает текущую задачу для передачи другой модели или участнику:

```markdown
## Цель
## Файлы и архитектура
## Подтверждённые факты
## Отброшенные гипотезы
## Ключевые фрагменты
## Текущий blocker
## Один точный вопрос
```

Skill должен ограничивать пакет, например 150–250 строками, и ссылаться на файлы вместо копирования всего репозитория.

### 9. `ctf-knowledge-update`

После solve добавляет в базу:

- root cause;
- exploit chain;
- команды;
- solver path;
- признаки похожих задач;
- ошибки команды;
- минимальный regression test.

### 10. `ctf-self-check`

Запускается после ответа другого AI:

1. выделяет все утверждения;
2. помечает факт, предположение и неизвестное;
3. ищет подтверждение в коде или runtime;
4. строит отрицательный тест;
5. проверяет, не придуманы ли API, offsets, CVE или файлы;
6. возвращает список неподтверждённых пунктов.

## MCP-мосты первой очереди

### Filesystem/workspace

Доступ только к каталогу конкретной задачи и общей read-only базе знаний. Не давать доступ к домашнему каталогу, SSH keys, browser profiles и password stores.

### Git

Нужные операции:

- status и diff;
- log/blame для исторических задач;
- отдельные task branches;
- сравнение fixed/revenge-вариантов.

Запретить force push, reset hard и удаление веток через agent tools.

### GitHub

Использовать для:

- поиска публичных исторических репозиториев;
- чтения tree, commit и release;
- поиска похожего кода;
- сохранения командных материалов в приватном репозитории, если это разрешено.

Token должен иметь минимальные scopes. Write tools можно отключить на соревновании.

### Browser/Playwright

Полезен для локальных web-задач и browser bot reproduction:

- DOM и CSP;
- cookies/storage;
- redirect chain;
- screenshot и console/network logs;
- Firefox/Chromium comparison.

Ограничить allowlist адресами задания, `localhost` и локальной Docker-сетью. Не давать модели свободный доступ к произвольным внешним целям.

### HTTP/fetch

Подходит для документации, GitHub raw files и контролируемых запросов к CTF endpoint. Добавить allowlist, timeout, response-size limit и журнал запросов.

## MCP-мосты второй очереди

### Ghidra или Binary Ninja

Нужен reverse-группе для:

- functions, xrefs, strings;
- decompiler output;
- rename/comment;
- controlled patching.

Начать с read-only команд. Изменения проекта и patch binary требуют подтверждения человека.

### GDB/pwndbg

Экспортировать ограниченные действия:

- запустить локальный target;
- breakpoint;
- registers, stack, mappings;
- disassembly и memory read;
- crash trace.

Не предоставлять произвольный shell через MCP. Компиляцию и запуск exploit оставлять под контролем участника.

### Docker

Разрешить `ps`, `logs`, `inspect`, `build` и `compose up` только внутри task project. `docker system prune`, удаление volume и privileged container должны требовать ручного подтверждения.

### Wireshark/tshark

Лучше предоставить read-only wrapper, который:

- показывает protocol hierarchy;
- применяет display filter;
- экспортирует выбранные stream;
- считает endpoints/conversations.

### Volatility

Wrapper для списка plugin, запуска выбранного plugin и сохранения текстового результата. Memory image остаётся локальным.

### SQLite/PostgreSQL/Redis

Подключать только к локальным контейнерам задания. По умолчанию read-only. Для Redis protocol exploitation безопаснее использовать отдельный локальный mock, а не общий инфраструктурный Redis команды.

## Что не подключать постоянно

- unrestricted shell MCP;
- credential manager;
- browser profile с личными сессиями;
- production cloud account;
- email, мессенджеры и платёжные API;
- remote desktop control;
- все reverse/forensic MCP одновременно.

## Профили инструментов

### `triage`

- filesystem read-only;
- Git read-only;
- context search;
- дешёвая модель.

### `web`

- filesystem;
- Git;
- Playwright;
- HTTP allowlist;
- Docker read/logs.

### `reverse-pwn`

- filesystem;
- Git;
- Ghidra/Binary Ninja;
- GDB/pwndbg;
- Docker/QEMU wrapper.

### `forensics`

- filesystem read-only для originals;
- рабочий output-каталог;
- Volatility;
- tshark;
- firmware tools.

### `crypto-ppc`

- filesystem;
- Python/Sage runner с timeout;
- z3;
- без browser и GitHub write.

Участник включает один профиль под текущую задачу. Это экономит контекст и уменьшает число ошибочных tool calls.

## Порядок внедрения

1. Создать `ctf-task-triage`, `ctf-context-pack`, `ctf-self-check` и `ctf-knowledge-update`.
2. Подключить filesystem, Git и Playwright.
3. Провести тренировку и измерить размер контекста, latency и число лишних вызовов.
4. Добавить category skills.
5. Подключать Ghidra, GDB, Volatility и Docker wrappers по одному.
6. Провести threat review каждого MCP: доступные данные, команды, secrets, network, destructive actions.
7. Зафиксировать рабочие версии конфигурации перед CTF.

## Проверка качества skill

Каждый skill тестируется на двух исторических задачах:

- одной простой;
- одной сложной цепочке.

Критерии:

- результат ссылается на реальные файлы;
- факты отделены от гипотез;
- нет выдуманных command/API;
- skill не загружает нерелевантные каталоги;
- следующий участник понимает состояние задачи без исходного чата;
- решение проходит независимую воспроизводимость.

## Документация

- OpenCode MCP: https://opencode.ai/docs/mcp-servers/
- Cursor MCP: https://cursor.com/docs/mcp

Конфигурации Cursor и OpenCode различаются. Сначала утвердите список MCP и permission model, затем создавайте client-specific файлы. Не копируйте конфигурацию с secrets в Git.
