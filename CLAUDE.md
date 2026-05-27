# Importica — Project Context for Claude

## Що це за проєкт

Сайт логістичної компанії **Importica** (importica.com.ua).
Засновник: Danylo | Email: info@importica.com.ua (alias для danylo@importica.com.ua)
Бізнес: 5 років досвіду, 100+ доставок до появи сайту.

**Послуги:**
- Доставка авто з Клайпеди (Литва) в Україну від $850
- Міжнародна логістика (США, Корея, Китай, Європа)
- Оплата інвойсів постачальникам за кордоном
- Безкоштовна оцінка митних ризиків
- Консолідація вантажів

---

## Файлова структура

```
importica/
├── index.html          ← ЄДИНИЙ файл сайту (весь HTML, CSS, JS всередині)
├── robots.txt          ← SEO
├── sitemap.xml         ← SEO (відправлено в Google Search Console)
├── CNAME               ← GitHub Pages домен: importica.com.ua
├── CLAUDE.md           ← Контекст для Claude Code CLI
├── context.md          ← Контекст для Claude.ai Project Knowledge
├── README.md           ← Документація проєкту
├── DEPLOY-GUIDE.md     ← Гайд по деплою
└── .github/
    └── workflows/
        └── deploy.yml  ← GitHub Actions CI/CD (auto-deploy при push)
```

> Немає окремих папок css/ або js/ — все вбудовано в index.html.

---

## Інфраструктура

| Що | Де / Як |
|----|---------|
| Хостинг | GitHub Pages — github.com/Daniil4449/importica |
| Домен | importica.com.ua (реєстратор: Imena.ua) |
| DNS | Cloudflare (неймсервери переведені з Imena) |
| SSL | Автоматично через GitHub Pages + Cloudflare |
| Email | Google Workspace → info@importica.com.ua (alias → danylo@importica.com.ua) |
| Gmail Send-as | info@ як sender за умовчанням (підтверджено Workspace alias) |
| Telegram бот | @importica_bot |
| Make.com | Пересилає повідомлення бота в групу "Importica - Заявки" + автовідповідь |
| Search Console | Підключено, sitemap відправлено 17.05.2026 |
| Форма (Customs) | Formsubmit + fetch/iframe (iOS fix 27.05.2026) |
| Google Analytics | GA4 підключено 27.05.2026 (G-GQBVZQ0LDF) |
| Google Sheets | "Importica — Ліди" → Apps Script webhook (27.05.2026) |
| Apps Script | Web App webhook (daniilka4449@gmail.com) |
| Bot команди | /start, /help — через BotFather (27.05.2026) |
| Make.com сценарій | 3 шляхи: /start → привітання, /help → контакти, інше → група + автовідповідь |
| UTM трекінг | localStorage + referrer auto-detect (Google/Telegram/Facebook/Instagram) |
| og:image | og-image.svg 1200×630px брендований (27.05.2026) |

**Контакти на сайті:**
- Телефон: `+38 050 231 36 52` — **ТИМЧАСОВИЙ**, замінити в 3 місцях: topbar, contacts-strip, footer
- Email: info@importica.com.ua
- Telegram: https://t.me/importica_bot

---

## Секції index.html (порядок на сторінці)

1. **Topbar** — телефон, email, @importica (білий текст #f5f5f0, hover жовтий #e8ff00)
2. **Header** — SVG лого, nav (жовті лінки #e8ff00), кнопка Telegram
3. **Hero** — доставка авто з Клайпеди від $850, маршрут, ціна, кнопки
4. **SEO-блок** — текстовий блок для пошукових систем
5. **Services** — 6 карток послуг
6. **Invoice** — оплата інвойсів: проблема → кроки → валюти → гарантії
7. **Customs** — оцінка митних ризиків + форма (Formsubmit + iframe ✅)
8. **How it works** — 4 кроки доставки авто
9. **Cities** — міста доставки з цінами (Київ, Львів, Харків, Дніпро, Одеса)
10. **Pricing** — порівняння ринок ($1000-1100) vs Importica ($850)
11. **Calculator** — JS калькулятор орієнтовної вартості доставки
12. **FAQ** — 6 питань і відповідей (accordion)
13. **CTA Final** — заклик написати
14. **Contacts strip** — телефон, email, telegram, сайт, локація
15. **Footer** — SVG лого, посилання, копірайт 2026
16. **Sticky CTA** — фіксована панель на мобільних

---

## Formsubmit — форма митних ризиків (Customs секція)

**Статус: ✅ повністю працює end-to-end (21.05.2026)**

- Сервіс: formsubmit.co (безкоштовний, без реєстрації)
- Endpoint: `https://formsubmit.co/56ca8e43c291e01a9138572f3c12ca69` (активований hash для danylo@importica.com.ua)
- Метод: реальний HTML form POST у **прихованому iframe** (AJAX endpoint не підтримує файли)
- Отримувач: danylo@importica.com.ua (sender: submissions@formsubmit.co)

**Технічна реалізація:**
```html
<form action="https://formsubmit.co/56ca8e43..." method="POST"
      enctype="multipart/form-data" target="formsubmit-frame">...</form>
<iframe name="formsubmit-frame" style="display:none;"></iframe>
```

**Кілька файлів:** Formsubmit обробляє лише один `name="attachment"`. Для додаткових файлів JS динамічно створює `attachment2`, `attachment3`... через DataTransfer API.

**Hidden fields:**
- `_subject` — `[НОВИЙ]` / `[ПОВТОРНИЙ]` + контакт
- `_template=table` — табличне форматування email
- `_captcha=false` — без капчі
- `_replyto` — email клієнта (натискання "Відповісти" в Gmail → авто-адресує клієнту)
- Дані: Статус_клієнта, Email_клієнта, Телефон, Вантаж, Країна, Вартість, Контакт, Деталі

**Повторні клієнти:** `localStorage.importica_customs_done='1'` → заголовок `[ПОВТОРНИЙ] — виставити рахунок $25-40`.

**При успіху** (iframe.onload): success UI на сторінці + збереження контактів у localStorage.
**При помилці/таймауті 15с:** toast + fallback відкриває Telegram.

**Примітки:**
- EmailJS видалено — несумісний з Edge Tracking Prevention
- Web3Forms замінено на Formsubmit 21.05.2026 (стабільніше, підтримка файлів)

---

## Gmail Send-as — info@importica.com.ua як sender

**Статус: ✅ налаштовано (21.05.2026)**

**Що зроблено:**
1. У Workspace Admin: `info@importica.com.ua` створено як alias облікового запису `danylo@importica.com.ua` (НЕ Group — групи блокували Gmail verification).
2. У Gmail Settings → Облікові записи → "Надсилати пошту від імені" → додано `Importica <info@importica.com.ua>` як alias (Google auto-verified без verification email).
3. Встановлено `info@importica.com.ua` за умовчанням.
4. Опція "Завжди відповідати з моєї адреси за умовчанням".

**Підводний камінь (вирішено):** Спочатку info@ був Google Group → блокував зовнішні verification email. Видалено і пересторено як справжній user alias.

---

## Google Analytics GA4

**Статус: ✅ підключено (27.05.2026)**
- Measurement ID: `G-GQBVZQ0LDF`
- Акаунт: daniilka4449@gmail.com → Property: Importica → Stream: Importica Website
- Код активовано в `<head>` index.html

---

## Google Sheets webhook — таблиця лідів

**Статус: ✅ працює (27.05.2026)**

- **Таблиця:** "Importica — Ліди" (Sheet ID: `1F5WF-leQPvqwnA-XTCPxRrSPc-amLa0KXzh_kX79Wvk`)
- **Колонки:** Дата | Час | Вантаж | Країна | Вартість | Email | Телефон | Контакт | Деталі | Джерело | Статус
- **Webhook URL:** `https://script.google.com/macros/s/AKfycbyosLZwguJ319pOKBigD7xXS5e5f7-c21z6o8zU2bWvuQKdqZmD8Qg9w9V_zxkPaPtq6g/exec`
- **Доступ:** Anyone (без авторизації) → Apps Script виконує doPost() → пише рядок у Sheet

**В index.html в onSuccess() після Formsubmit:**
```javascript
fetch('https://script.google.com/macros/s/.../exec', {
  method:'POST', mode:'no-cors',
  body: JSON.stringify({cargo, country, value, email, phone, contact, notes, source})
});
```

**Apps Script код:** doPost(e) → SpreadsheetApp.openById().appendRow([...])

---

## UTM-трекінг джерела лідів

**Статус: ✅ працює (27.05.2026)**

JavaScript `detectLeadSource()` запускається при завантаженні сторінки:
1. Якщо в URL є `?utm_source=...` → використовує його
2. Інакше якщо `document.referrer` → визначає: Google / Facebook / Instagram / Telegram / domain
3. Інакше → "Прямий" (first-touch attribution через localStorage)

**Зберігається:** `localStorage.importica_lead_source`
**Передається в:** email (через Formsubmit), Google Sheets (через webhook)

**UTM-посилання для соцмереж:**
```
importica.com.ua?utm_source=telegram&utm_medium=social
importica.com.ua?utm_source=facebook&utm_medium=social
importica.com.ua?utm_source=instagram&utm_medium=social
```

---

## Telegram бот — повна архітектура

**@importica_bot** (керує @BotFather, акаунт daniilka4449@gmail.com)

### Команди (BotFather → Commands):
- `/start` — 🚀 Новий запит — написати нам
- `/help` — Контакти та інформація

### Опис бота (BotFather → Edit Info → "What can this bot do?"):
```
🚗 Доставка авто з Клайпеди від $850
🌍 Міжнародна логістика зі США, Кореї, Китаю
⚖️ Оцінка митних ризиків
📋 Для нового запиту — напишіть /start
```

### Make.com сценарій "Інтеграція Telegram-бота"

**Структура (3 шляхи через Router):**

| Шлях | Умова | Дія |
|------|-------|-----|
| 1-й `/start` | Message.Text = /start | Bot 3: Привітання + перелік послуг |
| 2-й Не /start | Text ≠ /start AND Text ≠ /help | Bot 4 (forward в групу "Заявки") → Bot 5 (автовідповідь "Дякуємо...") |
| 3-й `/help` | Text = /help | Bot 6: Контакти Importica |

**Telegram Bot 6 (новий, 27.05.2026):** надсилає повідомлення з контактами в Chat ID клієнта (`Message: Chat: ID`):
```
📞 Контакти Importica
🌐 Сайт: importica.com.ua
📧 Email: info@importica.com.ua
💬 Telegram: @importica_bot
☎️ Телефон: +38 050 231 36 52
⏰ Працюємо щодня з 9:00 до 20:00
📋 Щоб залишити заявку — напишіть /start
```

---

## Дизайн-система (важливо зберігати!)

```
Фон:       #0d0d0d (майже чорний)
Акцент:    #e8ff00 (жовто-зелений)
Текст:     #f5f5f0 (майже білий)
Другорядний: #555, #666, #777
Карточки:  #1a1a1a фон, #2a2a2a бордер
Telegram:  #229ED9
Шрифти:    Unbounded (заголовки, 900) + DM Sans (текст)
Фонова сітка: крапкова, radial-gradient rgba(232,255,0,0.10) 1px, 28px × 28px на body
```

**Стилі навігації (оновлено 21.05.2026):**
- `nav a` — `#e8ff00` (брендовий жовтий), hover → `#f5f5f0`
- `.topbar a` — `#f5f5f0` (білий), hover → `#e8ff00`

**Топбар (оновлено 21.05.2026):**
- Емодзі 🚗 в топбарі перевернуто через `transform:scaleX(-1)` (їде вліво)

**Митниця (оновлено 21.05.2026):**
- Емодзі митниці скрізь: `⚖️` (замінено з `🛃` — не підтримувався на Windows)
- Правильна назва: "Допомога з митницею" (не "митнею")

**Правила:**
- Не змінювати існуючі стилі
- Нові секції додавати органічно в той самий стиль
- Зберігати всі SEO meta-теги при будь-яких змінах

---

## Що зарезервовано в git (прибрано, легко повернути)

| Що | Коли повертати |
|----|----------------|
| Секція "Про нас" | Коли готові реальні цифри (5 років, 100+ доставок, кількість країн) |
| Секція відгуків | Коли з'являться реальні відгуки від клієнтів |
| Instagram кнопки | Коли буде створено Instagram акаунт |

---

## Пріоритети (наступні кроки по черзі)

- [x] **Web3Forms** — ЗРОБЛЕНО 20.05.2026
- [x] **Видалити main.html** — ЗРОБЛЕНО 19.05.2026
- [x] **Formsubmit + iframe + multiple files** — ЗРОБЛЕНО 21.05.2026 (заміна Web3Forms, підтримка файлів)
- [x] **Gmail Send-as info@** — ЗРОБЛЕНО 21.05.2026 (Workspace alias, за умовчанням)
- [x] **Reply-To на клієнта** — ЗРОБЛЕНО 21.05.2026
- [x] **Стилі nav/topbar** — ЗРОБЛЕНО 21.05.2026 (жовті лінки, білий topbar)
- [x] **Тест end-to-end** — ЗРОБЛЕНО 21.05.2026 (4 файли + відповідь дійшла клієнту)
- [x] **Крапкова сітка фону** — ЗРОБЛЕНО 21.05.2026 (premium dot grid, жовтий 10% opacity, 28px)
- [x] **Виправлено 🛃 → ⚖️** — ЗРОБЛЕНО 21.05.2026 (у всіх місцях де митниця)
- [x] **Виправлено "митнею" → "митницею"** — ЗРОБЛЕНО 21.05.2026
- [x] **GA4** — підключено 27.05.2026 (G-GQBVZQ0LDF)
- [x] **og:image** — зроблено 27.05.2026 (og-image.svg 1200×630)
- [x] **iOS Safari fix** — 27.05.2026 (fetch для text-only)
- [x] **Калькулятор митниці** — 27.05.2026 (офіційні ставки UA, авто+товари)
- [x] **Валюти інвойсу** — 27.05.2026 (USD/EUR/UAH/USDT)
- [x] **UTM-трекінг джерела** — 27.05.2026
- [x] **Google Sheets таблиця лідів** — 27.05.2026
- [x] **Apps Script webhook** — 27.05.2026
- [x] **Bot меню команд** — 27.05.2026 (/start, /help)
- [x] **Bot опис** — 27.05.2026 (інструкції для повторних клієнтів)
- [x] **Make.com 3-й шлях /help** — 27.05.2026 (контакти)
- [ ] **Make.com → Google Sheets для бота** — додати модуль Google Sheets на Path 2 (НА ЗАВТРА 28.05.2026)
- [ ] **Постійний телефон** — замінити тимчасовий +380502313652 (3 місця в index.html)
- [ ] **Секція "Про нас"** — повернути з реальними даними
- [ ] **Відгуки** — додати реальні (перші клієнти через сайт)
- [ ] **Instagram** — створити акаунт → підключити кнопки на сайті
- [ ] **English версія** — довгострокова ціль

---

## Git / Deploy

```bash
# Стандартний workflow:
git add index.html
git commit -m "опис змін"
git push origin main
# → GitHub Actions автоматично деплоїть на importica.com.ua
```

Бранч: `main` — єдиний, прямо в продакшн.
