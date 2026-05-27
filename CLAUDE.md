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
| Форма (Customs) | Formsubmit + hidden iframe (підключено 21.05.2026) |

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
- [ ] **Постійний телефон** — замінити тимчасовий +380502313652 (3 місця в index.html)
- [ ] **Секція "Про нас"** — повернути з реальними даними
- [ ] **Відгуки** — додати реальні (перші клієнти через сайт)
- [ ] **Instagram** — створити акаунт → підключити кнопки на сайті
- [ ] **og:image** — зображення 1200×630px для соцмереж
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
