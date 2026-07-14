# Pi — Агentic Coding Platform

Pi — это автономная платформа для agentic (агентного) программирования с поддержкой MCP-инструментов, подпедентов и LLM. Платформа позволяет создавать сложные многошаговые задачи, делегируя их специализированным агентам.

---

## 📚 Документация

Полная документация находится в папке `/home/slabak/.pi/agent/docs/`:

| Файл | Описание |
|------|----------|
| [`MCP_OVERVIEW.md`](docs/MCP_OVERVIEW.md) | **Что такое MCP** — обзор протокола, архитектура проекта, конфигурация серверов (chrome-devtools, github, ssh-mcp), типы вызовов инструментов |
| [`SETTINGS.md`](docs/SETTINGS.md) | **Настройки проекта** — `settings.json`, `mcp.json`, конфигурация API, таймауты, подпеденты |
| [`SUBAGENTS.md`](docs/SUBAGENTS.md) | **Подпеденты (Subagents)** — как использовать subagent_supervisor, chain/parallel/single вызовы, асинхронные задачи |
| [`QUICKSTART.md`](docs/QUICKSTART.md) | **Быстрый старт** — установка, проверка инструментов, работа с файлами и терминалом |

---

## 🚀 Быстрый старт

### 1. Запустите pi
```bash
cd /home/slabak/.pi && pi -r
```

### 2. Проверьте MCP-инструменты
```javascript
// Chrome DevTools
chrome_devtools_list_pages()

// GitHub
server.github_search_repositories(query: "test")

// SSH
ssh_mcp_exec(command: "ls /home/slabak/.pi")
```

### 3. Используйте подпедентов
```json
{"agent": "documenter", "task": "Напиши документацию по функции get_weather()"}
```

---

## 🔧 MCP (Model Context Protocol)

MCP — это стандартный протокол, позволяющий LLM получать доступ к внешним инструментам: файловой системе, браузеру, API и т.д.

### Подключённые сервера
| Сервер | Команда | directTools |
|--------|---------|-------------|
| `chrome-devtools` | `/home/slabak/Desktop/start-chrome-devtools.sh` | ✅ true |
| `github` | `npx @modelcontextprotocol/server-github` | 🔹 2 инструмента |
| `ssh-mcp` | `npx ssh-mcp ...` | ✅ true (открытый SSH) |

### Типы инструментов
- **tools** — функции с входными/выходными параметрами
- **resources** — данные из внешних источников
- **prompts** — интерактивные шаблоны для пользователя

---

## 🤖 Подпеденты (Subagents)

Подпедент — это автономный агент, работающий внутри основного агента. Каждый подпедент:
- Имеет свой контекст `{previous}` (предыдущий ответ родителя)
- Может использовать те же инструменты и навыки
- Обменивается сообщениями через `intercom`

### Управление
```bash
// Список доступных агентов
subagent_supervisor({ action: "list" })

// Отображение деталей
subagent_supervisor({ action: "get", agent: "documenter" })

// Отправка сообщения
subagent_supervisor({ action: "send", to: "documenter", message: "..." })

// Ответ подпеденту
subagent_supervisor({ action: "reply", replyTo: "msg12345", message: "..." })
```

### Типы вызовов
- **SINGLE** — один агент, простая задача
- **CHAIN** — цепочка агентов (результат одного → входной для следующего)
- **PARALLEL** — несколько агентов работают одновременно
- **ASYNC** — долгая задача на фоне с отслеживанием через supervisor

---

## ⚙️ Настройки проекта

### settings.json
| Параметр | Значение | Описание |
|----------|----------|----------|
| `defaultModel` | `qwopus3.5-4b-coder` | LLM для генерации ответов |
| `defaultProvider` | `lmstudio` | Провайдер (llama.cpp) |
| `api.timeout` | `210000` | Общий таймаут API в мс |
| `subagents.defaultTimeoutMs` | `210000` | Таймаут подпедента |
| `subagents.defaultModel` | `qwen3.6-27b-ud` | Модель для подпедентов |

### mcp.json
Настройка MCP-серверов:
- `toolPrefix` — префикс инструментов (server.github.)
- `idleTimeout` — время неактивности до закрытия соединения
- `requestTimeoutMs` — таймаут на запрос к серверу
- `directTools` — прямой доступ без префикса
- `autoAuth` — автоматическая авторизация при подключении

---

## 📂 Структура проекта

```
/home/slabak/.pi/
├── README.md                 # Эта документация
│
├── settings.json             # Основные настройки (LLM, API, таймауты)
│   └── subagents/            # Настройки подпедентов (модели, timeouts)
│
├── prompts/                  # Системные промпты
│   └── AGENTS.md            # Промпт для агента
│
├── docs/                     # 📚 Документация проекта
│   ├── MCP_OVERVIEW.md      # Обзор MCP и конфигурация серверов
│   ├── SETTINGS.md          # Настройки проекта (settings.json, mcp.json)
│   ├── SUBAGENTS.md         # Руководство по подпедентам
│   └── QUICKSTART.md        # Быстрый старт
│
├── agent/                    # Конфигурация агентов
│   ├── mcp.json              # Настройка MCP-серверов
│   ├── models.json           # Конфиг моделей LLM
│   ├── mcp-cache.json        # Кэш MCP-серверов
│   └── auth.json             # Авторизация (зашифровано)
│
├── npm/                      # NPM пакеты проекта
│   └── node_modules/         # Пакеты: pi-mcp-adapter, pi-subagents
│
├── sessions/                 # История сессий подпедентов
└── auth.json                 # Авторизация (зашифровано)
```

---

## 🔐 Безопасность

⚠️ **Важно**: SSH-сервер подключён через `ssh-mcp` с паролем. Никогда не передавайте пароли в чистом виде — используйте `-p` параметр.

### Проверка конфигурации
```bash
cat /home/slabak/.pi/agent/mcp.json   # Проверить настройки серверов
cat /home/slabak/.pi/agent/settings.json  # Проверить таймауты и модели
```

---

## 🆘 Troubleshooting

| Проблема | Решение |
|----------|---------|
| Сервер не подключен | Проверьте `mcp.json`, перезапустите pi (`pi -r`) |
| Ошибка авторизации GitHub | Убедитесь, что токен в `.env` (если требуется) |
| Ошибка SSH | Проверьте сеть, таймауты в `ssh-mcp` конфиге |
| Таймаут LLM | Увеличьте `api.timeout` в settings.json |

---

*Документация обновлена: 2026-07-14* | *Версия: v1.0*

---
