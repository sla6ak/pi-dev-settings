# Настройки проекта — Документация конфигурации

## Файл настроек: `settings.json`

```json
{
  "lastChangelogVersion": "0.80.6",
  "theme": "dark",
  "defaultProvider": "lmstudio",
  "defaultModel": "qwopus3.5-4b-coder",
  "systemPrompt": "../prompts/AGENTS.md",
  "packages": [
    "npm:pi-mcp-adapter",
    "npm:pi-subagents"
  ],
  "api": {
    "timeout": 210000
  },
  "subagents": {
    "defaultTimeoutMs": 210000,
    "defaultModel": "qwen3.6-27b-ud",
    "agentOverrides": {
      "documenter": {
        "model": "qwopus3.5-4b-coder"
      },
      "researcher": {
        "model": "qwopus3.5-4b-coder"
      },
      "junior-coder": {
        "model": "qwopus3.5-4b-coder"
      },
      "tester": {
        "model": "qwopus3.5-4b-coder"
      }
    }
  }
}
```

## Поля конфигурации

### Базовые настройки

| Параметр | Значение | Описание |
|----------|----------|----------|
| `lastChangelogVersion` | `"0.80.6"` | Версия последнего релиза для проверки обновлений |
| `theme` | `"dark"` | Тёмная тема интерфейса (светлая/тёмная) |

### Модель LLM

| Параметр | Значение | Описание |
|----------|----------|----------|
| `defaultProvider` | `"lmstudio"` | Провайдер LLM: lmstudio, ollama и др. |
| `defaultModel` | `"qwopus3.5-4b-coder"` | Дефолтная модель для генерации ответов |

### Система промптов

| Параметр | Значение | Описание |
|----------|----------|----------|
| `systemPrompt` | `"../prompts/AGENTS.md"` | Путь к системному промпту (относительно agent/) |

### API настройки

| Параметр | Значение | Описание |
|----------|----------|----------|
| `api.timeout` | `210000` | Общий таймаут API-запросов в миллисекундах (~3.5 мин) |

### Паки (npm)

```json
"packages": [
  "npm:pi-mcp-adapter",
  "npm:pi-subagents"
]
```
Установленные дополнительные пакеты для расширения функционала.

### Подпеденты (Subagents)

#### Общие настройки подпедентов

| Параметр | Значение | Описание |
|----------|----------|----------|
| `defaultTimeoutMs` | `210000` | Таймаут выполнения подпедента (~3.5 мин) |
| `defaultModel` | `"qwen3.6-27b-ud"` | Модель для подпедентов (отдельная от LLM) |

#### Переоверрайды моделей по агенту

Каждый агент может использовать свою модель:

```json
"agentOverrides": {
  "documenter": { "model": "qwopus3.5-4b-coder" },
  "researcher": { "model": "qwopus3.5-4b-coder" },
  "junior-coder": { "model": "qwopus3.5-4b-coder" },
  "tester": { "model": "qwopus3.5-4b-coder" }
}
```

| Агент | Модель | Назначение |
|-------|--------|------------|
| `documenter` | qwopus3.5-4b-coder | Документирование кода и API |
| `researcher` | qwopus3.5-4b-coder | Исследование и анализ информации |
| `junior-coder` | qwopus3.5-4b-coder | Базовая разработка (синтаксис, простые паттерны) |
| `tester` | qwopus3.5-4b-coder | Тестирование кода |

## Обновление настроек

1. **Измените файл** `/home/slabak/.pi/agent/settings.json`
2. **Перезапустите** pi через терминал: `pi -r`
3. **Или перезагрузите** терминальный интерфейс

## Проверка конфигурации

```bash
# Показать текущую конфигурацию
cat /home/slabak/.pi/agent/settings.json
```

---

*Документация обновлена: 2026-07-14*
