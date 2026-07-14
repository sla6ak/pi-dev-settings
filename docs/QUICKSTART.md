# Быстрый старт — Руководство для новичков

## Шаг 1: Установка

```bash
# Перейти в директорию проекта
cd /home/slabak/.pi

# Запустить pi (первый запуск)
pi -r
```

## Шаг 2: Проверка подключения к MCP-серверам

МCP-сервера подключаются автоматически при старте. Проверьте работу инструментов:

### Chrome DevTools инструменты

```javascript
// Получить список открытых вкладок
chrome_devtools_list_pages()

// Сделать скриншот текущей вкладки
chrome_devtools_take_screenshot(filePath: "screenshot.png")
```

### GitHub инструменты

```javascript
// Поиск репозиториев
server.github_search_repositories(query: "my-project", page: 1, perPage: 30)

// Получить содержимое файла
server.github_get_file_contents(owner: "username", repo: "repo-name", path: "file.md")
```

### SSH инструменты

```javascript
// Выполнить команду на удалённом сервере
ssh_mcp_exec(command: "ls -la /home/slabak/.pi")
```

## Шаг 3: Использование подпедентов (Subagents)

### Запустить документатора

```json
{ "agent": "documenter", "task": "Напиши документацию по функции get_weather()" }
```

### Запустить исследователя

```json
{ "agent": "researcher", "task": "{task}" }
```

## Шаг 4: Работа с файлами

| Действие | Команда |
|----------|---------|
| Создать файл | `write("/home/slabak/.pi/file.md", content)` |
| Читать файл | `read(path)` — вернёт содержимое (обрезается до 2000 строк) |
| Редактировать | `edit(path, edits[])` — точная замена текста |

## Шаг 5: Выполнение команд в терминале

```bash
# Обычная команда
ls -la /home/slabak/.pi/agent/

# Команда с sudo (удалённый SSH)
ssh_mcp_exec("sudo apt update")

# Запуск bash-команды локально
bash -c "git log --oneline"
```

## Полезные команды

### О проекте
```bash
ls /home/slabak/.pi/           # Список файлов в корне
cat /home/slabak/.pi/settings.json   # Настройки проекта
```

### В agent папке
```bash
ls /home/slabak/.pi/agent/        # Папка с настройками и подпедентами
cat /home/slabak/.pi/agent/mcp.json    # Конфиг MCP-серверов
```

## Частые ошибки

### Ошибка: "Сервер не подключен"
**Причина**: Сервер не запущен или неверный конфиг.  
**Решение**: Проверьте `mcp.json` и перезапустите pi.

### Ошибка: "Нет доступа к файлу"
**Причина**: Неправильные права на файл.  
**Решение**: `chmod +x /path/to/file` или используйте `ssh-mcp`.

### Ошибка: "Таймаут ожидания ответа"
**Причина**: Сервер не отвечает в течение timeoutMs секунд.  
**Решение**: Увеличьте таймаут в настройках или проверьте сеть.

---

*Документация обновлена: 2026-07-14*
