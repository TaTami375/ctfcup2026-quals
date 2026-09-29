# Кубок CTF России — исторический контекст отборочных этапов

## Область исследования

Документ обобщает публичные репозитории отборочных этапов Кубка CTF России, обнаруженные в GitHub-организации `acisoru` на 29 сентября 2026 года.

Исследованные репозитории:

- https://github.com/acisoru/ctfcup-2020-quals
- https://github.com/acisoru/ctfcup-21-quals
- https://github.com/acisoru/ctfcup22-quals
- https://github.com/acisoru/ctfcup2023-quals
- https://github.com/acisoru/ctfcup2025-quals

Охваченные годы: **2020, 2021, 2022, 2023 и 2025**. В организации `acisoru` не найден публичный репозиторий отборочного task-based этапа 2024 года. Репозитории финалов, школьных соревнований и этапов Attack/Defense исключены.

Обозначения достоверности:

- **ПОДТВЕРЖДЕНО** — техника прямо описана в README, исходном коде, официальном райтапе или solver/exploit-файле.
- **ВЫВЕДЕНО ИЗ ИСХОДНИКОВ** — классификация следует из структуры или кода, но не сформулирована в официальном райтапе.
- **НЕ УСТАНОВЛЕНО** — имеющихся материалов недостаточно для надёжного вывода.

Ограничение: дерево репозитория 2020 года содержит недопустимый для Windows путь `Can you find me?/`. Git-объекты и верхнеуровневый список заданий доступны, но обычный checkout завершился ошибкой. Поэтому задания 2020 года изучены менее глубоко, чем материалы 2021–2025 годов.

## Общая характеристика соревнования

Отборочный этап преимущественно проводится в формате jeopardy CTF. Репозитории обычно содержат описания заданий, исходный код, публичные вложения, конфигурации развёртывания, официальные решения и exploit/solver-скрипты. Для сетевых сервисов регулярно используется Docker Compose.

Основные категории: web, reverse, pwn, crypto и forensics. В отдельные годы присутствуют OSINT, PPC, warmup, партнёрские pentest-задания и beginners-трек.

Наблюдаемое развитие:

- **2020–2022:** много заданий с одной основной техникой, но сложные задачи уже используют ядро Linux, side-channel, необычные архитектуры, особенности браузеров и различия парсеров.
- **2023:** явно выражены многоступенчатые цепочки: SSRF → CRLF → Redis → Celery → RCE, обходы WAF, CSP bypass, custom VM и современные JavaScript runtime.
- **2025:** заметны Rust, kernel pwn, TOCTOU, memory forensics с rootkit, headless browser/PDF, anti-debugging и серии исправленных revenge-задач. Одновременно добавлен большой beginners-трек.

## Карта репозиториев

| Год | Репозиторий | Строк заданий | Основные категории | Материалы решений |
|---|---|---:|---|---|
| 2020 | `acisoru/ctfcup-2020-quals` | 25 | reverse, pwn, forensics, crypto, misc/joy | исходники, отдельные решения и райтапы |
| 2021 | `acisoru/ctfcup-21-quals` | 31, включая fixed-варианты | web, reverse, pwn, forensics, crypto, OSINT | решения в README и exploit-скрипты |
| 2022 | `acisoru/ctfcup22-quals` | 23 | crypto, web, forensic, reverse, pwn, warmup | README и solver-скрипты |
| 2023 | `acisoru/ctfcup2023-quals` | 24 с партнёрскими задачами | forensic, reverse, web, crypto, pwn, misc/PPC, pentest | подробные RU/EN-райтапы и solver-скрипты |
| 2025 | `acisoru/ctfcup2025-quals` | 24 | crypto, reverse, pwn, web, forensics, beginners | source, deploy и solve-каталоги |

Число строк не всегда равно числу уникальных сервисов: в 2021 году исходные и fixed-варианты некоторых crypto-задач используют общие каталоги.

## Обзор категорий

### Web

Повторяются не столько одиночные инъекции, сколько различия парсеров и цепочки эксплуатации:

- SQL injection совместно с обходом nginx/OpenResty WAF;
- SSRF во внутренние Elasticsearch, Redis/Celery, Chrome DevTools и PDF/browser-сервисы;
- CRLF injection, дублированные параметры и неоднозначные HTTP-заголовки;
- XSS, CSP bypass, CSRF, browser bot и различия Firefox/Chromium;
- JWT algorithm confusion и ошибки генерации сессий;
- небезопасная десериализация и доверие к внутренним очередям.

### Crypto

Встречаются XOR и known-plaintext, padding/oracle-подобные атаки, ошибки подписей, эллиптические кривые, нестандартные кодирования, слабые протоколы и математическое восстановление данных. Поздние задачи часто объединяют криптографию с reverse.

### Pwn

Регулярно требуются:

- stack/heap exploitation;
- constrained shellcode;
- утечки libc и обход KASLR;
- side-channel;
- kernel exploitation;
- TOCTOU и out-of-bounds mapping;
- эксплуатация небезопасности типов и lifetime в Rust.

### Reverse

Характерны custom VM, нестандартные кодирования, anti-debugging, многоступенчатые или самомодифицирующиеся бинарники, изменённые системные вызовы, эмуляция нескольких архитектур, crash dump, timing attack и nanomites.

### Forensics и Misc

Используются memory dump, Windows Registry, firmware, сетевые дампы, PDF, custom filesystem, следы malware, скрытые процессы/rootkit, геолокация, OSINT и алгоритмическая автоматизация.

## Каталог заданий

### 2020 — `ctfcup-2020-quals`

| Задание | Категория | Автор | Концепция | Достоверность |
|---|---|---|---|---|
| Agile Lover | reverse | PassKeyRa | Qt/Windows-приложение; доступны исходники и generator | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| SpaceZ | reverse | PassKeyRa | анализ бинарника | НЕ УСТАНОВЛЕНО |
| Randomizer | reverse | PassKeyRa | анализ логики рандомизации | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Path | reverse | PassKeyRa | анализ бинарника | НЕ УСТАНОВЛЕНО |
| exexe | reverse | PassKeyRa | анализ исполняемого файла | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Double | reverse | keltecc | анализ бинарника | НЕ УСТАНОВЛЕНО |
| Baby overwrite | pwn | boggda | базовая перезапись памяти | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Encrypted notes | pwn | boggda | memory corruption в сервисе заметок | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| GOverflow | pwn | boggda | overflow/corruption, связанный с Go | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Integer and floats | pwn | boggda | ошибка числового представления или преобразования | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| NULLAzino777 | pwn | boggda | NULL-related memory exploitation | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Caller | pwn | keltecc | управление потоком исполнения/calling convention | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Excel | forensics | не указан | исследование spreadsheet-артефакта | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Implant | forensics | не указан | анализ implant/malware | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| NPR | forensics | не указан | техника не установлена | НЕ УСТАНОВЛЕНО |
| RDP | forensics | не указан | артефакты Remote Desktop | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| TrueMaster | forensics | не указан | техника не установлена | НЕ УСТАНОВЛЕНО |
| Alone | crypto | keltecc | криптоанализ | НЕ УСТАНОВЛЕНО |
| Diffusion | crypto | keltecc | анализ custom diffusion/cipher | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Numeric | crypto | keltecc | числовая криптография/custom encoding | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Warmer | crypto | keltecc | вводный криптоанализ | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Square | crypto | keltecc | number-theory криптоанализ | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Esoteric | misc | keltecc | esoteric language/encoding | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |
| Can you find me? | joy | Valoriel | поиск по координатам и изображению; присутствуют writeup-файлы | ВЫВЕДЕНО ИЗ ИСХОДНИКОВ |

Не следует считать inferred-классификацию 2020 года официальным описанием решения.

### 2021 — `ctfcup-21-quals`

#### Web

- **Private search** — SQL injection в ClickHouse, обход nginx WAF дублированным параметром и SSRF через URL-функцию. Solver: `tasks/web/search-engine/solution/solve.py`. **ПОДТВЕРЖДЕНО**.
- **Personal Info Site** — HTML injection в `<base>`, перенаправление разрешённого nonce-script на JavaScript атакующего и CSP bypass. **ПОДТВЕРЖДЕНО**.
- **PHP Challenge** — цепочка особенностей PHP завершается инъекцией аргументов PHP CLI и RCE. **ПОДТВЕРЖДЕНО**.
- **PDF me** — обход SSRF-фильтра восьмеричной записью IP, доступ к Chrome DevTools Protocol и чтение `file:///etc/flag`. **ПОДТВЕРЖДЕНО**.
- **Json config** — приложение и Celery используют общую Redis DB; атакующий создаёт Celery task с произвольными параметрами. **ПОДТВЕРЖДЕНО**.
- **Gap** — Go `CVE-2019-9741`, CRLF injection заголовка `Authorization`, доступ к внутреннему Elasticsearch. **ПОДТВЕРЖДЕНО**.

#### Reverse

- **Ultimate Password Protector** — анализ 64-bit Windows crash dump, восстановление heap-пароля и таблиц замены. **ПОДТВЕРЖДЕНО**.
- **Super Secure Linux rev-part** — reverse защищённого Linux-компонента; связан с forensic-частью. **ПОДТВЕРЖДЕНО**.
- **Solid Snake** — поиск патча в Python executable и анализ добавленного обработчика `unlock(...)`. **ПОДТВЕРЖДЕНО**.
- **Simple Protocol** — reverse custom network protocol. **ПОДТВЕРЖДЕНО**.
- **Risky Business**, **Kate** — анализ бинарников и task-specific transformation; присутствуют официальные решения. **ПОДТВЕРЖДЕНО**.

#### Pwn

- **Shell Batya** — shellcode/control-flow exploitation.
- **Secure Notes 1/2** — два уровня memory corruption в notes-сервисе.
- **Random Data Shop** — эксплуатация памяти сервиса случайных данных.
- **My Documents** — вводный binary exploitation.
- **Mnogoetazhka v1/v2** — связанные stack-задачи с растущими ограничениями.

Для всех перечисленных pwn-задач присутствуют официальные README с решениями: **ПОДТВЕРЖДЕНО**.

#### Forensics, Crypto и OSINT

- **Utechka** — анализ артефактов утечки данных.
- **Super Secure Linux for-part** — анализ украденных файлов совместно с reverse-частью.
- **Strange PDF** — исследование структуры и содержимого PDF.
- **Simple Secure File System** — восстановление custom filesystem.
- **Script Kiddie Attack** — расследование артефактов атаки.
- **Signature Server / fixed** — ошибки сервера цифровой подписи и изменённая поверхность атаки fixed-варианта.
- **Операция «Фактор»** — восстановление секретной компоненты через арифметику группы/эллиптической кривой и расшифрование сообщения.
- **x ^= y; y ^= x; x ^= y** — XOR-криптоанализ; README содержит brute-force и аналитический способы.
- **Padding / fixed** — padding-related криптографическая атака и исправленный вариант.
- **Drink long die young** — OSINT-поиск личности/местоположения.

Все пункты этой группы: **ПОДТВЕРЖДЕНО** официальными README.

### 2022 — `ctfcup22-quals`

#### Web

- **warmup** — SQL injection в Go за Lua/nginx WAF; обход основан на различной обработке `;` в URL. **ПОДТВЕРЖДЕНО**.
- **simple** — JWT algorithm confusion `RS256` → `HS256`; публичный TLS-ключ используется как HMAC secret. **ПОДТВЕРЖДЕНО**.
- **onlytweets** — browser bot, CSRF tokens, XSS/CSP и особенности Firefox. **ПОДТВЕРЖДЕНО**.
- **legacy** — неэкранированный username в Perl CGI session generation позволяет получить секрет admin. **ПОДТВЕРЖДЕНО**.

#### Reverse

- **Simple Encoder** — ARM/C++, кодирование Голомба нулевого порядка, написание decoder. **ПОДТВЕРЖДЕНО**.
- **unicorn_plc** — stripped binary запускает проверки для MIPS, PowerPC и SPARC32 через Unicorn Engine. **ПОДТВЕРЖДЕНО**.
- **crackme2077** — VMProtect-бинарник, решаемый timing side-channel. **ПОДТВЕРЖДЕНО**.
- **bebrus** — изменённое ядро Linux; поиск кода проверки во `vfs_read` и восстановление скрытого флага. **ПОДТВЕРЖДЕНО**.

#### Pwn

- **watcher** — утечка libc через `fs`, восстановление стека и побитовый side-channel через монитор целостности страницы. **ПОДТВЕРЖДЕНО**.
- **SHELLyha** — constrained x86 shellcode: XOR-патч `int 0x80`, `read` второго payload и запуск shell. **ПОДТВЕРЖДЕНО**.
- **ozne** — userspace binary exploitation с официальным solver. **ПОДТВЕРЖДЕНО**.
- **babystack** — вводный stack exploitation с официальным solver. **ПОДТВЕРЖДЕНО**.

#### Crypto, Forensics и Warmup

- **Good pad / bad pad**, **Important task**, **Dinosaurs**, **The time has come**, **Stolen blueprints** — padding/construction analysis и task-specific математический криптоанализ. Точная схема должна уточняться по README каждого задания. **ПОДТВЕРЖДЕНО на уровне категории**.
- **malware_autopsy** — static/dynamic malware forensics.
- **fallen_angel** — восстановление forensic-артефактов.
- **french_connection** — network/host forensics.
- **Red firmware I** — извлечение и анализ firmware.
- **classic_matreshka** — вложенные архивы/форматы.
- **gpb_math_pong** — интерактивная математическая автоматизация.

Последние шесть пунктов: **ПОДТВЕРЖДЕНО**.

### 2023 — `ctfcup2023-quals`

#### Web

- **waf** — два заголовка `Content-Type` обходят OpenResty WAF; SQL injection приводит к RCE. **ПОДТВЕРЖДЕНО**.
- **scanner** — SSRF → CRLF injection → запись в Redis → поддельный Celery result/task metadata → небезопасная десериализация → RCE. Solver: `tasks/web/scanner/solve/sploit.py`. **ПОДТВЕРЖДЕНО**.
- **novosti** — stored XSS; framing same-origin страницы 40x без CSP, DOM injection, чтение внутреннего flag service и exfiltration. **ПОДТВЕРЖДЕНО**.
- **mistakes** — особенности `window.name`, prototype pollution и обход фильтра атрибутов через `iframe.srcdoc`. **ПОДТВЕРЖДЕНО**.

#### Reverse

- **monitoring** — deobfuscation JavaScript внутри Bun binary и обход Deno `--allow-read` передачей второго пути после запятой. **ПОДТВЕРЖДЕНО**.
- **time_capsule** — modified MD5 в custom VM с random salt для каждого символа; reverse и brute-force 256 значений либо подмена `getrandom`. **ПОДТВЕРЖДЕНО**.
- **magick_lock** — custom lock/transformation; присутствуют bilingual writeup и solver. **ПОДТВЕРЖДЕНО**.
- **2fac** — reverse custom verification с официальным решением. **ПОДТВЕРЖДЕНО**.

#### Crypto и Pwn

- **murdata** — сетевой crypto service с Docker deployment и solve.
- **geometry** — geometry/number-theory cryptanalysis.
- **lateralus** — анализ custom cryptographic construction.
- **more_heap** — heap exploitation; имеются исходники, Docker и solve.
- **some_storage** — memory corruption в storage service.
- **spy** — binary exploitation с официальным solver.

Все пункты: **ПОДТВЕРЖДЕНО**.

#### Forensics, PPC, Misc и партнёрские задачи

- **gamer1/gamer2** — связанные misc/forensic-этапы вокруг игровых артефактов.
- **zion / zion-revenge** — парные forensic-задачи; revenge содержит RU/EN writeup.
- **Floating in Delirium** — алгоритмическая/precision automation.
- **checkpoint** — сетевой Python PPC-сервис и solver.
- **slons** — Node.js PPC-сервис и solver.
- **Mystery_ldap**, **NFS**, **stealer** — партнёрские pentest-задачи Positive Technologies по LDAP, NFS и endpoint/stealer investigation.

Все пункты: **ПОДТВЕРЖДЕНО на уровне темы задания**.

### 2025 — `ctfcup2025-quals`

#### Web

- **goose / goose-revenge** — связанные web-задачи с Python solver. README краткие, поэтому точную уязвимость следует подтверждать по `tasks/web/*/solve/solve.py`. **ПОДТВЕРЖДЕНО наличие решения; точная техника не установлена**.
- **trustmebroker** — SSRF через headless HTML-to-PDF converter и обход HTML filter; присутствуют Node/Puppeteer-компоненты. **ПОДТВЕРЖДЕНО**.

#### Reverse

- **annabelle** — self-modifying matryoshka: чередование RWX buffers, ARC4-шифрование стадий с ключом из hash кода и обратное восстановление transformations. **ПОДТВЕРЖДЕНО**.
- **nanomachines** — Rust crackme, elliptic-curve exponentiation, nanomites через fork/debugger и `SIGSEGV`; обращение степени по модулю порядка кривой. **ПОДТВЕРЖДЕНО**.
- **Wincrack** — Windows anti-debug: TLS callback заменяет fake verification на real verification; seed зависит от PEB и состояния debugger. **ПОДТВЕРЖДЕНО**.

#### Pwn

- **bad parser** — Linux userspace TOCTOU → OOB `mmap` → libc leak → перезапись TLS/`printf`-состояния → code execution. **ПОДТВЕРЖДЕНО**.
- **play with ker** — Linux kernel heap OOB → file spray → KASLR leak через `cpu_entry_area` → arbitrary write в `modprobe_path`. **ПОДТВЕРЖДЕНО**.
- **safelang / revenge / revenge-revenge** — Rust lifetime extension/type-system unsoundness на основе `cve-rs`; каждый patched-вариант закрывает предыдущие обходы. **ПОДТВЕРЖДЕНО**.

#### Crypto и Forensics

- **miracle** — executable matryoshka/transformation, восстановление флага обратным применением стадий. **ПОДТВЕРЖДЕНО**.
- **cursed** — custom crypto с исходниками и solver; конкретный primitive следует уточнять по `tasks/crypto/cursed/Readme.md`. **ПОДТВЕРЖДЕНО наличие материалов**.
- **BimboIncident** — memory dump с rootkit, скрытым процессом и code injection; требуется memory forensics. **ПОДТВЕРЖДЕНО**.
- **brokilon** — Windows Registry и fileless malware. **ПОДТВЕРЖДЕНО**.
- **Funeral** — firmware с kernel, rootfs и 48-байтовым trailer, содержащим AES key и IV. **ПОДТВЕРЖДЕНО**.

#### Beginners

- **babyrev** — базовый reverse.
- **babypwn** — базовый binary exploitation.
- **babymisc** — базовый анализ артефактов/encoding.
- **babyfor** — вводный forensics с documented solve.
- **babycrypto** — вводный cryptanalysis.
- **babyppc** — geospatial/algorithmic automation; top-level ссылка и имя каталога расходятся.
- **babyosint** — вводный OSINT.
- **babyweb** — вводный web exploitation с solve notes.

Все beginners-задачи: **ПОДТВЕРЖДЕНО на уровне категории и темы**.

## Повторяющиеся классы уязвимостей

1. **Различия парсеров — один из главных признаков CTF Cup.** Повторяются расхождения nginx/backend, duplicate headers, CRLF, PHP CLI parsing и Deno permission parsing.
2. **SSRF обычно является промежуточным шагом.** Целями становятся Elasticsearch, Chrome DevTools, Redis/Celery, internal API или PDF renderer.
3. **Browser-задачи требуют знания политик безопасности.** CSP, `<base>`, iframe, 40x pages, `srcdoc`, Firefox/Chromium и browser bot встречаются многократно.
4. **Внутренние queue/datastore ошибочно считаются доверенными.** Redis и Celery неоднократно становятся частью RCE-chain.
5. **Fixed/revenge-варианты принципиально важны.** Signature/Padding fixed, `zion-revenge`, `goose-revenge` и три версии `safelang` проверяют понимание первопричины.
6. **Side-channel может быть основным решением.** Встречаются timing attack и побитовая утечка через integrity monitor.
7. **Знания ядра нужны в reverse, pwn и forensics.** Изменённые syscall, kernel heap OOB, rootkit, firmware и memory image появляются в разные годы.
8. **Custom transformation и VM регулярно используются в reverse.** Modified MD5 VM, multi-architecture emulation, ARC4 stages и nanomites — характерные примеры.

## Повторяющиеся технологии

- Linux, ELF, C/C++, glibc, kernel modules и QEMU-окружение.
- Python для сервисов, Celery, exploit automation и solver-скриптов.
- Go для web-сервисов и protocol targets.
- JavaScript/Node.js, Puppeteer/headless Chrome, Bun и Deno.
- PHP и Perl CGI.
- Redis, Celery, Elasticsearch, ClickHouse, nginx/OpenResty/Lua.
- Docker и Docker Compose.
- Rust в современных reverse/pwn-задачах.
- Windows internals: PEB, TLS callbacks, crash dump, memory dump и Registry.

## Повторяющиеся инструменты

- **Web:** Burp Suite, `curl`, browser DevTools, собственные Python HTTP-клиенты.
- **Pwn:** Python, pwntools, GDB с pwndbg/GEF, ROP tools, compiler/binutils, QEMU.
- **Reverse:** IDA, Ghidra, Binary Ninja/radare2, debugger, `strace`, `ltrace`, Unicorn Engine.
- **Forensics:** Volatility, Wireshark/tshark, firmware extraction tools, Registry parsers.
- **Crypto/PPC:** Python, SageMath, z3, arbitrary-precision libraries.
- **Infrastructure:** Docker Compose и внимательный анализ nginx/OpenResty configuration.

## Специфические признаки CTF Cup

- Не останавливаться на первой найденной ошибке: web-решения часто состоят из двух–четырёх этапов.
- Сравнивать все парсеры на пути запроса: proxy, WAF, framework, runtime, browser, serializer и datastore protocol.
- Считать deployment-файлы частью задания. Внутренние hostname, port, shared Redis DB и debug port часто прямо указывают путь эксплуатации.
- В reverse ожидать, что очевидная функция проверки может быть fake, runtime-patched, emulated, encrypted или выполняться другим процессом/debugger.
- В pwn готовиться не только к ret2libc, но и к side-channel, kernel, race, Rust unsoundness, sandbox и constrained shellcode.
- Проверять официальный solver по исходникам, а не копировать его без понимания.
- Низкое число решений часто означает необычные знания окружения или длинную цепочку, а не просто большой объём кода.

## Чек-лист подготовки

### Web

- [ ] Анализировать различия nginx/OpenResty/Lua и backend parser.
- [ ] Практиковать duplicate query parameters и duplicate HTTP headers.
- [ ] Строить CRLF payload для HTTP, Redis и внутренних сервисов.
- [ ] Цеплять SSRF с Redis, Celery, Elasticsearch, CDP и internal API.
- [ ] Понимать Celery message/result format и границы десериализации.
- [ ] Практиковать CSP bypass через `<base>`, frame, error page и `srcdoc`.
- [ ] Знать различия Firefox и Chromium browser bot.
- [ ] Повторить JWT algorithm/key confusion.
- [ ] Аудировать headless HTML-to-PDF и sanitizer.

### Crypto

- [ ] Освоить XOR/known-plaintext и custom encoding.
- [ ] Практиковать padding и oracle-подобные атаки.
- [ ] Изучить misuse цифровой подписи и сравнение patched-вариантов.
- [ ] Использовать Sage/Python для elliptic curve order и inverse exponent.
- [ ] Уметь переносить custom hash/cipher из бинарника в solver.

### Pwn

- [ ] Свободно решать stack, heap, libc leak и arbitrary write.
- [ ] Писать constrained и multi-stage shellcode.
- [ ] Практиковать TOCTOU и mapping race.
- [ ] Строить Linux kernel exploit с KASLR bypass и file spray.
- [ ] Понимать TLS, `fs`-relative state и внутренности libc.
- [ ] Изучить Rust lifetime unsoundness и `cve-rs`.
- [ ] Проверять timing/output side-channel, если прямое чтение запрещено.

### Reverse

- [ ] Анализировать Windows TLS callback, PEB anti-debug и crash dump.
- [ ] Реверсить custom VM и писать brute-force/emulation harness.
- [ ] Разбирать self-modifying/staged code и обращать transformations.
- [ ] Инструментировать Unicorn и multi-architecture code.
- [ ] Анализировать kernel syscall/VFS modifications.
- [ ] Извлекать JavaScript из Bun/Deno/Node binary.
- [ ] Понимать nanomite/debugger и signal-driven computation.

### Forensics

- [ ] Использовать Volatility для hidden process, rootkit и injected code.
- [ ] Анализировать Windows Registry и fileless persistence.
- [ ] Извлекать firmware, rootfs, trailer, key и IV.
- [ ] Исследовать PDF, PCAP, RDP artifact и custom filesystem.
- [ ] Связывать host, network и malware evidence.

### Misc, PPC и OSINT

- [ ] Писать устойчивую Python automation для интерактивных протоколов.
- [ ] Работать с floating-point, precision и геоданными.
- [ ] Выполнять воспроизводимый OSINT с фиксацией источников.
- [ ] Исследовать nested archive, esoteric encoding и необычные форматы.

### Инфраструктура

- [ ] Читать Dockerfile и Compose topology до анализа application code.
- [ ] Составлять карту внутренних hostname, port, credentials и datastore.
- [ ] Воспроизводить сервис локально и побайтово сравнивать proxy/backend behavior.
- [ ] Иметь toolchain для C/C++, Go, Python, Node, Rust, ARM и kernel/QEMU.

## Главные выводы

1. **Топология развёртывания часто является частью уязвимости.** Shared queue и internal debug/admin service превращают локальную ошибку в доступ к флагу.
2. **Неоднозначность нужно считать атакуемой.** Если одни байты разбирают два компонента, следует проверять duplicate fields, separator, encoding, scheme, IP form и line endings.
3. **Защитные механизмы служат подсказкой.** CSP, WAF, sanitizer, sandbox, anti-debug или integrity monitor указывают на ожидаемый класс обхода.
4. **Автоматизацию следует начинать рано.** Побайтовое восстановление, side-channel, protocol generation, emulation и повторные browser request требуют скриптов.
5. **Fixed/revenge-задачи нужно сравнивать диффом.** Это показывает security invariant, который проверяет автор.
6. **Нужна широкая системная подготовка.** Соревнование объединяет браузеры, очереди, базы данных, Linux kernel, Windows internals, firmware и современные runtime.

## Источники

Основные репозитории:

- https://github.com/acisoru/ctfcup-2020-quals
- https://github.com/acisoru/ctfcup-21-quals
- https://github.com/acisoru/ctfcup22-quals
- https://github.com/acisoru/ctfcup2023-quals
- https://github.com/acisoru/ctfcup2025-quals

Локальные источники:

- `.research/ctfcup-21-quals/README.md` и `.research/ctfcup-21-quals/tasks/`
- `.research/ctfcup22-quals/README.md` и `.research/ctfcup22-quals/tasks/`
- `.research/ctfcup2023-quals/README.md` и `.research/ctfcup2023-quals/tasks/`
- `.research/ctfcup2025-quals/README.md` и `.research/ctfcup2025-quals/tasks/`
- `.research/ctfcup-2020-quals/.git` — Git object database; ограничение checkout описано выше

Для сравнения с новым заданием сначала следует искать в этом документе класс уязвимости, затем изучать README, исходники, deployment и solver соответствующей исторической задачи.

## Статистика выполнения

- Исследовано репозиториев: **5**.
- Охвачено лет: **5** — 2020, 2021, 2022, 2023, 2025.
- Строк заданий в официальных инвентарях: **127**, включая fixed-варианты и партнёрские задания.
- Основные категории: **web, crypto, pwn, reverse, forensics, misc, PPC, OSINT, warmup, beginners, pentest**.
- Райтапы и solver/exploit-файлы: присутствуют во всех полностью извлечённых репозиториях 2021–2025; точное число файлов не фиксируется, чтобы не смешивать dependency-примеры и дубли RU/EN.
- Не полностью исследован: **репозиторий 2020 года** из-за несовместимого с Windows имени пути.
