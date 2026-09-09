# Запуск AI-виджета на prostranstvo.pw

Виджет использует бекенд **agent_system** (тот же, что на d-art.space).

---

## Что уже подготовлено локально

### Сайт (`prostranstvo.pw/`)

| Путь | Назначение |
|------|------------|
| `widget/contact-widget.js` | Виджет чата |
| `widget/contact-widget.css` | Стили виджета |
| `widget/widget-theme.css` | Золото/фиолет под стиль «Пространства» |
| `widget/backend-client/` | Копия промпта для загрузки на сервер бекенда |
| `index.html` | Виджет подключён |
| `Legal/*.html` | Виджет подключён (пересобрать: `python build_legal.py`) |

### Бекенд (`d-art-s/agent_system/`)

| Путь | Назначение |
|------|------------|
| `backend/clients/prostranstvo/system.md` | Строгий промпт консультанта |
| `backend/clients/prostranstvo/knowledge.json` | Тарифы, инструменты, entry_points MAX/TG |
| `backend/clients/prostranstvo/contacts.json` | Бот поддержки Telegram |
| `backend/clients/prostranstvo/meta.json` | Имя агента |
| `backend/clients/prostranstvo/NOTIFY.txt` | Куда слать уведомления в Telegram |
| `../agent_system/DEPLOY-SERVER.md` | Полная инструкция деплоя бекенда на сервер |

**agentId виджета:** `prostranstvo`

---

## Шаг 1 — Бекенд на сервере

1. Скопировать папку клиента на сервер:
   ```
   agent_system/backend/clients/prostranstvo/
   ```
   (можно взять из `widget/backend-client/`)

2. **Проверить ссылку на бот поддержки** в `contacts.json`:
   ```json
   {
     "phone": "",
     "telegram": "https://t.me/Ai1_consultant_bot"
   }
   ```
   Замените на реальный URL вашего саппорт-бота, если другой.

3. В `.env` бекенда добавить домен в CORS:
   ```
   CORS_ORIGIN=...,https://prostranstvo.pw,https://www.prostranstvo.pw
   ```

4. Перезапустить бекенд:
   ```bash
   sudo systemctl restart d-art-backend
   ```

5. Проверка:
   ```bash
   curl https://d-art.space/health
   ```

6. *(Опционально)* Уведомления о лидах: `notify.json` или `TELEGRAM_NOTIFY_BY_AGENT`.

---

## Шаг 2 — Файлы сайта на сервер

Залить на хостинг prostranstvo.pw:

```
widget/contact-widget.js
widget/contact-widget.css
widget/widget-theme.css
index.html
Legal/*.html
```

**Важно:** заливайте CSS и JS, не только HTML.

---

## Шаг 3 — Проверка

1. Открыть https://prostranstvo.pw
2. Кнопка чата в правом нижнем углу
3. Спросить: «Какие тарифы?», «Как начать?», «Как связаться с поддержкой?»
4. В Network: `POST https://d-art.space/api/chat` → **200**

---

## Настройки виджета

- `data-api-url="https://d-art.space/api"` — бекенд (nginx проксирует `/api/` → Node)
- `data-agent-id="prostranstvo"` — клиент
- `data-cw-theme="dark"` — тёмная тема
- `data-cw-no-auto-open="true"` — без авто-открытия

Подробнее — атрибуты в `index.html` и `build_legal.py`.
