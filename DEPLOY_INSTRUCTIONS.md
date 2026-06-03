# Rosevent — развёртывание статики (production)

**Пакет:** `C:\Rosevent_only_front`  
**Собран:** 2026-06-03 из `C:\Rosevent` (`npm run build`)  
**Режим:** только фронт, форма → Unisender, Strapi на хостинге не нужен.

---

## Состав пакета

| Путь | Назначение |
|------|------------|
| `index.html` | Точка входа SPA |
| `assets/` | JS/CSS бандл Vite |
| `.htaccess` | SPA routing (Apache) + заголовок nosniff |
| `fallback-media/` | Изображения и hero-видео без Strapi |
| `trust-logos/` | Логотипы блока «клиенты» |
| `robots.txt`, `sitemap.xml` | SEO (если есть в сборке) |

Проверено в пакете: hero-видео `fallback-media/homepage/Video_ot_Ros_Event_f8e9cd239e.mp4`.

---

## Требования к хостингу

- **Виртуальный хостинг** с Apache (Masterhost Basic/Standard и аналоги).
- **Не нужны:** Node.js, PostgreSQL, Strapi на этом же тарифе.
- **SSL:** Let's Encrypt в панели, редирект HTTP → HTTPS.

---

## Загрузка на сервер (FTP / файловый менеджер)

1. Резервная копия текущего сайта в `public_html` (или `www`).
2. Очистить каталог сайта от **старых** файлов фронта (не трогать служебные каталоги панели).
3. Загрузить **содержимое** этой папки в **корень** сайта — не саму папку `Rosevent_only_front`:
   - `index.html`
   - `assets/`
   - `.htaccess`
   - `fallback-media/`
   - `trust-logos/`
   - остальные файлы из корня пакета
4. Права: файлы **644**, каталоги **755**.
5. Убедиться, что `.htaccess` виден на сервере (в FTP включить показ скрытых файлов).

---

## Проверка после выкладки

- `https://ВАШ-ДОМЕН/` — главная, hero-видео или фон, логотипы клиентов.
- `https://ВАШ-ДОМЕН/portfolio` — страница открывается (не 404 Apache).
- `https://ВАШ-ДОМЕН/privacy-policy` — политика конфиденциальности.
- Форма «Получить предложение» → шаг Unisender → подписка.
- DevTools → Network: **нет** обязательных запросов к `localhost:1337` или недоступному Strapi.
- Прямой URL: `https://ВАШ-ДОМЕН/fallback-media/homepage/Video_ot_Ros_Event_f8e9cd239e.mp4` → **200**.

---

## Обновление сайта

1. В репозитории `C:\Rosevent`: правки → `npm run build`.
2. Снова скопировать `build\*` в `C:\Rosevent_only_front` (или пересобрать пакет скриптом ниже).
3. Залить на хостинг только изменённые файлы или весь каталог (проще — полная замена с бэкапом).

### PowerShell (локально, без изменения репо кроме `build/`)

```powershell
cd C:\Rosevent
npm run build
$dst = "C:\Rosevent_only_front"
if (Test-Path $dst) { Remove-Item $dst -Recurse -Force }
New-Item -ItemType Directory -Path $dst | Out-Null
Copy-Item -Path "build\*" -Destination $dst -Recurse -Force
```

---

## Переменные, вшитые в сборку

Сборка использовала `.env` из `C:\Rosevent`:

- `VITE_FORM_SUBMIT_MODE=unisender`
- `VITE_UNISENDER_SUBSCRIBE_ACTION`, `VITE_UNISENDER_FORM_ID`
- `VITE_UNISENDER_LIST_ID_LEADS`, `VITE_UNISENDER_LIST_ID_MARKETING`

Чтобы сменить форму или ключи — отредактировать `.env` в репо, выполнить `npm run build`, пересобрать пакет в `Rosevent_only_front`.

---

## Nginx (если не Apache)

Эквивалент `.htaccess` для SPA:

```nginx
location / {
  try_files $uri $uri/ /index.html;
}
```

---

## Связанная документация в репозитории

`C:\Rosevent\docs\active\MASTERHOST_STATIC_ONLY_DEPLOY.md`  
`C:\Rosevent\docs\active\UNISENDER_FORM_INTEGRATION.md`
