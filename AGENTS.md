# AGENTS — Rosevent_only_front

## Что это за репозиторий

Production-only статика для **rosevent.ru** (Masterhost, Apache). Нет `package.json` — только собранные файлы (`index.html`, `assets/`, `fallback-media/`, `.htaccess`).

Исходный код: `C:\Rosevent` / https://github.com/alfarius42/Rosevent

## Ветки (обязательно)

- **`develop`** — все изменения агента/разработчика. CI: валидация пакета.
- **`main`** — продакшн-артефакт. Merge только через PR из `develop`. CI: валидация + деплой (если настроены secrets).

Не пушить напрямую в `main`, если не явно запрошено пользователем.

## Типичные задачи

| Задача | Действие |
|--------|----------|
| Обновить сайт после сборки | Скопировать `build/*` из Rosevent → commit в `develop` → PR → `main` |
| Проверить пакет | `DEPLOY_INSTRUCTIONS.md`, workflow `validate-static.yml` |
| Деплой | GitHub Actions `deploy-static.yml` (secrets) или ручная загрузка FTP |

## Файлы, которые нельзя ломать

- `index.html` — entry SPA
- `.htaccess` — SPA routing Apache
- `assets/` — hashed bundles; не переименовывать вручную без пересборки
- `fallback-media/`, `trust-logos/` — медиа без Strapi

## Секреты GitHub (для автодеплоя)

`SSH_PRIVATE_KEY`, `REMOTE_HOST`, `REMOTE_USER`, `FRONTEND_REMOTE_PATH` — или FTP: `FTP_HOST`, `FTP_USERNAME`, `FTP_PASSWORD`, `FTP_FRONTEND_PATH`.

## Стиль работы

См. `.cursor/rules/communication-and-process.mdc` — сначала документация (`DEPLOY_*`, README), потом изменения файлов.
