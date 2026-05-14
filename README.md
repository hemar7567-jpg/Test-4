# CryptoMiner Pro

Тап-кликер крипто-майнер для Telegram WebApp. По геймплею — в духе Hamster Kombat.

**Стек:** чистый HTML / CSS / JS, без сборки. Один файл = одна игра.

## ✨ Возможности

- Классический тап-геймплей с энергией и комбо-системой
- 8 апгрейдов тапа/энергии/мультипликатора и 7 пассивных майнеров
- 6 бустов (×2 тап, авто-тап, золотой дождь, и т.п.)
- 14 квестов (постоянных и дневных)
- 6 скинов монет, разблокируемых по уровню
- 10 уровней с накоплением «всего заработано»
- 8 ачивок
- Ежедневный бонус на 14 дней с прогрессией наград
- Лаки-монета — мини-ивент раз в 1–3 минуты
- Офлайн-доход (до 8 часов)
- **Реальный TonConnect** — подключение Tonkeeper, MyTonWallet и др.
- Реферальная система через `?start=ref_<id>` Telegram-ссылок
- Лидерборд (фейковые имена + ты)
- Тёмная премиальная тема, animated-фон, частицы

## 📁 Структура

```
.
├── index.html                    # вся игра в одном файле
├── tonconnect-manifest.json      # манифест TonConnect — публичный URL обязателен
├── icon-192.png                  # иконка для манифеста (192×192)
├── icon-512.png                  # иконка для манифеста (512×512, опционально)
├── icon.svg                      # исходник иконки в SVG
├── README.md
├── LICENSE                       # MIT
└── .gitignore
```

## 🚀 Быстрый старт (GitHub Pages)

### 1. Создай GitHub-репозиторий

Например `cryptominer-game`. **Публичный** (приватные не работают на бесплатном Pages).

### 2. Залей файлы из этой папки в корень репозитория

```bash
git init
git add .
git commit -m "Initial CryptoMiner Pro"
git branch -M main
git remote add origin https://github.com/USERNAME/cryptominer-game.git
git push -u origin main
```

### 3. Включи GitHub Pages

В репозитории: **Settings → Pages**

- **Source:** `Deploy from a branch`
- **Branch:** `main` / `(root)`
- **Save**

Через 1–2 минуты сайт будет доступен по адресу:

```
https://USERNAME.github.io/cryptominer-game/
```

### 4. Подмени URL в трёх местах

#### `tonconnect-manifest.json`

```json
{
  "url": "https://USERNAME.github.io/cryptominer-game",
  "name": "CryptoMiner Pro",
  "iconUrl": "https://USERNAME.github.io/cryptominer-game/icon-192.png"
}
```

#### `index.html` — найди эти константы:

```js
const TON_MANIFEST_URL = 'https://USERNAME.github.io/cryptominer-game/tonconnect-manifest.json';
const BOT_USERNAME     = 'твой_бот_без_собаки';
```

Закоммить и запушь снова.

### 5. Создай Telegram-бота

1. Открой [@BotFather](https://t.me/BotFather) в Telegram
2. `/newbot` → задай имя и username (запиши username — он нужен в `BOT_USERNAME`)
3. Получи токен (пригодится для бэка позже)
4. `/setmenubutton` → выбери бота → введи URL `https://USERNAME.github.io/cryptominer-game/` и текст кнопки (например, «⛏️ Играть»)
5. (опционально) `/setdomain` → укажи `USERNAME.github.io`

Готово. Открывай бота в Telegram и нажимай на кнопку — игра запустится.

## 🎮 Управление

- **Тап по монете** — добыча монет (расход энергии 1/тап)
- **Длительный спам** — поднимает комбо (до ×2.9 при 20+ комбо)
- **8% шанс крита** — ×3 к одному тапу
- Меню снизу: майнить / задания / скины / топ / TON

## 💎 Что работает с TonConnect

- Реальное подключение TON-кошелька через popup TonConnect SDK
- Адрес сохраняется в localStorage и подставляется в верхний бар
- Заявка на вывод формируется и копится в `S.ton.pending`

> Реальная **отправка TON** при выводе требует серверной части с проектным TON-кошельком (см. ниже).

## 👥 Что работает с рефералами

- Парсинг `start_param` формата `ref_<telegram_id>` из `Telegram.WebApp.initDataUnsafe`
- Стартовый бонус +2000 монет новичку (один раз, защита от повтора)
- Запрет приглашать самого себя
- Реф-ссылка с реальным TG-ID игрока: `t.me/BOT_USERNAME?start=ref_<id>`
- Шеринг ссылки через нативный Telegram-share

> Начисление **+5000 монет пригласившему** требует бэкенда — иначе любой игрок через DevTools раскрутит счётчик в localStorage. До бэка `S.refs` и `S.refEarn` остаются локальной декорацией.

## 📊 Сохранения

Состояние хранится в `localStorage` под ключом `cm_v4`. Структура — см. функцию `defaultState()` в `index.html`.

Очистить прогресс:
```js
localStorage.removeItem('cm_v4'); location.reload();
```

## 🛠 Разработка

Открой `index.html` напрямую в браузере (Chrome / Edge / Firefox). Mobile-режим DevTools 430×900 — самое близкое к Telegram WebApp.

Для тестирования внутри Telegram нужен HTTPS — открывай через GitHub Pages.

## 🔮 Этап 2 — бэкенд (когда дойдёт)

Чего не закроет статичный фронт:

| Фича | Нужен бэк? | Почему |
|---|---|---|
| Подключение TON | ❌ | TonConnect работает с клиента |
| Парсинг `start_param` | ❌ | Делаем на клиенте |
| Реальная отправка TON при выводе | ✅ | Транзакция от проектного кошелька |
| Защита от накруток (master state) | ✅ | localStorage редактируется |
| Учёт рефералов с +5000 пригласившему | ✅ | Иначе любой накрутит |
| Лидерборд с реальными игроками | ✅ | Нужна общая БД |

Под бэкенд бесплатно подходят:

- **Render.com** — 750 ч/мес для веб-сервисов
- **Railway.app** — $5 кредита/мес
- **Fly.io** — 3 машины free
- **Vercel** — Python serverless functions

Рекомендую: Python + FastAPI + SQLite + aiogram (для бота) на Render.

## 📜 Лицензия

[MIT](./LICENSE)
