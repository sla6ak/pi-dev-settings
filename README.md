# Как привязать Git-репозиторий к GitHub

## Шаг 1. Настройте имя пользователя и email
```bash
git config --global user.name "Ваше Имя"
git config --global user.email "ваш-email@example.com"
```

## Шаг 2. Создайте репозиторий на GitHub
Перейдите на [github.com](https://github.com) → **Settings** → **New repository** и создайте новый публичный репозиторий. Скопируйте URL, например: `https://github.com/username/repo.git`

## Шаг 3. Добавьте файлы в Git
```bash
git add .
git commit -m "Initial commit"
```

## Шаг 4. Подключите к GitHub
```bash
git remote add origin https://github.com/username/repo.git
git push -u origin main
# или для ветки master:
# git push -u origin master
```

## Шаг 5. Настройте удалённые репозитории
```bash
git config --global init.defaultBranch main
```

---
