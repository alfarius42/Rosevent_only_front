# Rosevent — production static (front only)

Готовый к выкладке статический пакет сайта ROSEVENT (Vite/React SPA). Исходники и сборка — в репозитории [Rosevent](https://github.com/alfarius42/Rosevent).

## Ветки

| Ветка | Назначение |
|-------|------------|
| `develop` | Интеграция: обновления пакета, проверки CI, работа Cursor/PR |
| `main` | Продакшн: только проверенный пакет; деплой на Masterhost по push |

## Быстрый старт

1. Клонировать: `git clone https://github.com/alfarius42/Rosevent_only_front.git`
2. Переключиться на `develop` для правок: `git checkout develop`
3. Деплой-инструкции: [DEPLOY_INSTRUCTIONS.md](./DEPLOY_INSTRUCTIONS.md)

## Обновление пакета из исходников

```powershell
cd C:\Rosevent
npm run build
# скопировать содержимое build/ в корень этого репозитория, затем commit → develop → PR → main
```

## Cursor

См. [AGENTS.md](./AGENTS.md) и [.cursor/rules/](./.cursor/rules/).
