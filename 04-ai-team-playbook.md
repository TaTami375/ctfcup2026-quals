# База знаний и правила работы команды с AI

## Главный принцип

AI предлагает гипотезы и ускоряет механическую работу. Участник подтверждает вывод кодом, экспериментом или документацией. Ответ модели без проверки не считается прогрессом.

## Структура базы знаний

```text
knowledge/
  index.md
  historical/
    context.md
  techniques/
    web/
    reverse/
    pwn/
    crypto/
    forensics/
    misc/
  tools/
  templates/
  solved/
tasks/
  <task-name>/
    README.md
    evidence.md
    hypotheses.md
    solve/
    artifacts/
```

### `knowledge/index.md`

Короткая карта: категория → техника → документ → исторический пример. Файл не должен превращаться в полный справочник.

### `evidence.md`

Содержит только подтверждённые наблюдения:

```markdown
- [FACT] `app.py:41` передаёт URL в `requests.get`.
- [FACT] container `redis` доступен только из compose network.
- [OBSERVED] duplicate parameter доходит до backend как второе значение.
```

### `hypotheses.md`

```markdown
- [OPEN] SSRF позволяет обратиться к Redis через gopher.
- [REJECTED] DNS rebinding: resolver pinning подтверждено двумя запросами.
- [CONFIRMED] CRLF попадает в raw HTTP request.
```

Модель должна получать оба файла. Так она не повторяет отвергнутые идеи и не смешивает предположения с фактами.

## Цикл работы с задачей

### 1. Собрать минимальный контекст

Перед первым запросом:

- сохранить условие;
- построить дерево файлов глубиной 2–3;
- определить entry points;
- прочитать deploy/config/dependencies;
- найти в `context.md` похожие исторические задачи;
- сформулировать один вопрос.

Плохой запрос: «Реши этот репозиторий».

Хороший запрос:

```text
Это разрешённая локальная CTF-задача. Цель: прочитать /flag из Docker-сервиса.
В nginx параметр проверяется как одно значение, Go backend получает его иначе.
Файлы: nginx.conf, main.go и compose.yml приложены.
Найди различия парсинга, предложи максимум три payload-класса и для каждого
дай локальный тест. Не пиши полный exploit, пока primitive не подтверждён.
```

### 2. Проверить ответ

- Найти каждую названную функцию и строку.
- Запустить минимальный тест.
- Проверить отрицательный случай.
- Записать результат в `evidence.md` или `hypotheses.md`.
- Не продолжать длинный диалог на ложном предположении.

### 3. Сжать контекст

После 10–15 сообщений создать новый context pack:

- цель;
- подтверждённые факты;
- отвергнутые гипотезы;
- текущий exploit primitive;
- один blocker.

Новый чат с кратким пакетом обычно полезнее старого чата с десятками логов.

### 4. Независимая проверка

Перед сдачей передать пакет другой модели или участнику с запросом:

```text
Проведи adversarial review. Найди неподтверждённые предположения,
hardcoded значения, зависимость от локального окружения и случайные совпадения.
Предложи минимальный отрицательный тест.
```

### 5. Обновить знания

После решения сохранить:

- root cause;
- признаки, по которым её можно узнать;
- exploit chain;
- solver и command запуска;
- ошибки и тупиковые гипотезы;
- ссылку на похожую историческую задачу.

## Как экономить токены

1. Ссылаться на файлы и строки вместо вставки целого репозитория.
2. Не отправлять `node_modules`, binaries, build artifacts и большие PCAP/memory dump. Сначала извлекать metadata и выборочные фрагменты.
3. Давать модели дерево каталогов, entry points и зависимости.
4. Разделять исследование и реализацию. Сначала получить проверяемую гипотезу, затем просить код.
5. Просить diff или одну функцию, а не полный файл.
6. Ограничивать число вариантов: «три наиболее вероятные гипотезы».
7. Не просить повторно пересказывать условие.
8. После смены гипотезы начинать новый чат с context pack.
9. Routine-задачи отправлять дешёвой модели; сильную модель подключать к blocker.
10. Сохранять удачные команды и шаблоны в базе, а не генерировать заново.

## Как снизить число отказов на CTF-задачах

- Прямо указывать, что задача относится к разрешённому CTF sandbox.
- Давать локальные файлы, Docker Compose и условие соревнования.
- Называть контролируемую цель: `localhost`, container или адрес организатора.
- Просить анализ root cause, PoC и проверку в разрешённой среде.
- Не маскировать намерение и не просить обход safeguards.
- При отказе сохранить полезную часть ответа и сменить модель. Не тратить запросы на спор.

Любой закрытый провайдер может отказать. Поэтому команда использует два клиента и несколько семейств моделей.

## Правила безопасности

- Не вставлять API keys, cookies, VPN config и личные tokens.
- Проверять правила CTF относительно внешних AI-сервисов.
- Не запускать сгенерированную команду до чтения.
- Не давать AI destructive Docker/Git/filesystem permissions.
- Не запускать неизвестный binary на основной машине. Использовать VM/container без личных данных.
- Не публиковать условия и решения во время соревнования.
- Хранить provider keys в environment/secrets store, а не в репозитории.

## Формат запроса для сложной задачи

```markdown
## Авторизация
Локальная/официальная CTF-задача, тестирование разрешено организатором.

## Цель
Конкретный результат без общих формулировок.

## Окружение
Architecture, OS, container, версии библиотек.

## Файлы
Пути и назначение ключевых файлов.

## Подтверждённые факты
Только наблюдения с доказательствами.

## Отвергнутые гипотезы
Что проверили и почему не сработало.

## Текущая гипотеза
Один предполагаемый primitive.

## Вопрос
Одна проверяемая задача для модели.

## Формат ответа
Гипотеза, доказательство, минимальный тест, критерий успеха.
```

## Персональные рекомендации

### greek

**Роль:** координатор и ведущий web.

Акцент:

- parser differentials между proxy/WAF/backend;
- SSRF-chain к Redis, Celery, Elasticsearch и CDP;
- Docker Compose topology;
- короткие task dossier для сильной модели;
- базовая crypto triage вместе с ℂаnnaβ∫$.

AI использовать для сравнения нескольких parser и adversarial review. Не отдавать модели управление всеми web-задачами сразу. Держать один coordination chat и отдельный чат на каждую задачу.

### DRAK0NN

**Роль:** ведущий forensics.

Акцент:

- Volatility, hidden process, injection и rootkit;
- Windows Registry и fileless malware;
- firmware/rootfs/AES trailer;
- PCAP и timeline automation;
- воспроизводимый forensic pipeline с hash исходников.

AI передавать результаты plugin и metadata, а не полный memory dump. Просить модель строить следующий discriminating test и проверять timeline.

### lldark

**Роль:** web + резерв reverse/pwn.

Акцент:

- browser/CSP/XSS и Playwright;
- Go/Python/Node source audit;
- ELF basics, mitigations, calling convention;
- GDB/pwndbg и pwntools;
- перенос reverse-навыков в поиск pwn primitive.

AI полезен для decompiler cleanup и exploit skeleton. Каждый offset и structure layout проверять в debugger.

### Lera

**Роль:** web, PPC/Misc/OSINT и резерв forensics.

Акцент:

- быстрые warmup/beginners;
- автоматизация HTTP и interactive protocol на Python;
- OSINT с фиксацией источников;
- PDF, archive и простые forensic artifacts;
- независимое воспроизведение web exploit.

AI использовать для boilerplate и преобразования данных. Не отправлять модели большие сырые артефакты до определения формата.

### Gromka

**Роль:** web и хранитель базы знаний.

Акцент:

- поддержание task board, evidence и rejected hypotheses;
- SQLi/WAF, auth/session/JWT;
- сбор context pack;
- Docker logs и service topology;
- проверка, что solved-задача документирована.

AI использовать для summarization только после очистки логов. Проверять, что summary не потерял значения header, byte и timestamp.

### Candy

**Роль:** ведущий reverse и временный владелец pwn.

Акцент:

- custom VM, anti-debug, runtime patching и self-modifying code;
- Windows TLS/PEB;
- Unicorn и multi-architecture code;
- stack/heap basics, leaks и arbitrary write;
- kernel/QEMU workflow;
- decoder/emulator scripts.

Сильную модель использовать после ручного определения architecture, entry point и key routines. Не загружать сырую декомпиляцию целиком. Передавать call graph и несколько функций.

### ℂаnnaβ∫$

**Роль:** web + forensics/reverse, координатор crypto triage.

Акцент:

- математическая формализация crypto-задачи;
- XOR, padding, signatures и ECC basics;
- SageMath/z3;
- связь crypto с reverse;
- помощь DRAK0NN на сложных артефактах;
- независимый review web-chain.

Перед запросом к AI записывать known/unknown, equations, oracle и expected complexity. Не принимать solver, который работает только на одном sample.

## Командные упражнения

1. **Web chain:** `scanner` 2023. Один участник строит topology, второй CRLF/Redis primitive, третий ревьюит Celery deserialization.
2. **Browser:** `novosti` 2023 или `Personal Info Site` 2021. Сравнить ручной анализ и Playwright-assisted workflow.
3. **Reverse:** `time_capsule` 2023. Сделать context pack без полной декомпиляции.
4. **Pwn:** `bad_parser` 2025. Разделить leak, arbitrary write и final execution.
5. **Forensics:** `brokilon` или `BimboIncident` 2025. Сохранить полный evidence log.
6. **Crypto:** `Операция «Фактор»` 2021. Сначала математическая модель, затем Sage solver.

После каждого упражнения измерять:

- время до первой рабочей гипотезы;
- число бесполезных AI-запросов;
- стоимость;
- число неподтверждённых утверждений;
- время передачи задачи другому участнику.

## Критерий готовности команды

Команда готова, если:

- каждый участник умеет создать task card и context pack за 10 минут;
- другой участник воспроизводит solver по базе знаний без исходного AI-чата;
- доступны минимум два model provider;
- spend cap и резерв проверены;
- category tools работают в изолированной среде;
- ни один workflow не требует передачи общего аккаунта или API key через чат;
- координатор умеет остановить задачу и перераспределить людей по измеримому прогрессу.
