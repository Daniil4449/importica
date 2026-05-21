# Importica — Project Knowledge

**Оновлено:** 21.05.2026

---

## Компанія

**Importica** — міжнародна логістична компанія.
- Сайт: https://importica.com.ua
- Засновник: Danylo Denisyuk
- Email: info@importica.com.ua (alias для danylo@importica.com.ua)
- Telegram: @importica_bot
- Телефон: +38 050 231 36 52 (**ТИМЧАСОВИЙ** — буде замінений)
- Досвід: 5 років у логістиці
- Доставок до сайту: 100+

**Послуги:**
1. Доставка авто з Клайпеди (Литва) в Україну — від $850 (власні автовози, регулярні рейси)
2. Морська доставка зі США, Кореї, Китаю (FCL/LCL контейнери)
3. Наземна доставка з Європи
4. Оплата інвойсів постачальникам (USD, EUR, KRW, CNY, GBP)
5. Безкоштовна оцінка митних ризиків
6. Консолідація вантажів

---

## Технічна інфраструктура

| Сервіс | Статус | Деталі |
|--------|--------|--------|
| Хостинг | ✅ | GitHub Pages — github.com/Daniil4449/importica |
| Домен | ✅ | importica.com.ua (Imena.ua) |
| DNS | ✅ | Cloudflare |
| SSL | ✅ | Автоматично (GitHub Pages + Cloudflare) |
| Deploy | ✅ | git push main → GitHub Actions → автодеплой |
| Email (Workspace) | ✅ | Google Workspace, info@ як alias для danylo@ |
| Gmail Send-as | ✅ | info@importica.com.ua — за умовчанням як відправник |
| Telegram бот | ✅ | @importica_bot |
| Make.com | ✅ | Автопересилання в групу "Importica - Заявки" + автовідповідь |
| Search Console | ✅ | Підключено, sitemap відправлено 17.05.2026 |
| Formsubmit | ✅ | Підключено 21.05.2026 — форма митних ризиків (замінив Web3Forms) |
| Google Analytics GA4 | ❌ | Не підключено (placeholder є в коді) |

---

## Formsubmit — деталі (форма митних ризиків)

**Статус: ✅ повністю працює end-to-end (21.05.2026)**

- Сервіс: formsubmit.co (безкоштовний, без реєстрації)
- Endpoint: `https://formsubmit.co/56ca8e43c291e01a9138572f3c12ca69` (активований hash для danylo@importica.com.ua)
- Метод: реальний HTML form POST у **прихованому iframe** (не fetch/AJAX, бо AJAX endpoint не підтримує файли)
- Отримувач: danylo@importica.com.ua → бачить email як від `submissions@formsubmit.co`

**Чому iframe, а не fetch:**
- AJAX endpoint Formsubmit (`/ajax/`) **не підтримує** вкладені файли
- Звичайний form POST перезавантажує сторінку — треба iframe як target

**Як це працює технічно:**
```html
<form target="formsubmit-frame" enctype="multipart/form-data">...</form>
<iframe name="formsubmit-frame" style="display:none;"></iframe>
```
JS встановлює `frame.onload` → коли iframe завантажує відповідь Formsubmit, показуємо success UI. Таймаут 15 сек на випадок мережевої помилки.

**Підтримка кількох файлів:**
Formsubmit обробляє лише ОДИН `name="attachment"`. Для кожного додаткового файлу динамічно створюються hidden inputs `attachment2`, `attachment3` тощо через DataTransfer API.

**Поля форми:**
- `_subject` — динамічний (`[НОВИЙ]` або `[ПОВТОРНИЙ]` + контакт)
- `_template=table` — гарне табличне форматування в email
- `_captcha=false` — без капчі
- `_replyto` — email клієнта (натискання "Відповісти" в Gmail → автоматично адресує клієнту)
- Дані: Статус_клієнта, Email_клієнта, Телефон, Вантаж, Країна, Вартість, Контакт, Деталі

**Розпізнавання повторних клієнтів:**
- `localStorage.importica_customs_done = '1'` встановлюється після першої успішної відправки
- При наступному запиті заголовок змінюється на `[ПОВТОРНИЙ] — виставити рахунок $25-40`
- Контакти нормалізуються (toLowerCase, без пробілів) перед порівнянням

**Логіка:** успіх (iframe.onload) → success UI + збереження контакту в localStorage | помилка/таймаут → toast + fallback на Telegram

**Примітки:**
- EmailJS не використовується — несумісний з Edge Tracking Prevention
- Web3Forms видалено 21.05.2026 — замінено на Formsubmit (стабільніший, з підтримкою файлів)

---

## Email: Gmail Send-as через Workspace alias

**Статус: ✅ повністю налаштовано (21.05.2026)**

**Як налаштовано:**
1. У Google Workspace Admin: `info@importica.com.ua` створено як alias для облікового запису `danylo@importica.com.ua` (НЕ як Group — групи блокували Gmail verification).
2. У Gmail → Settings → Облікові записи → "Надсилати пошту від імені" → додано `Importica <info@importica.com.ua>` як alias (Google auto-verified без verification email).
3. `info@importica.com.ua` встановлено **за умовчанням**.
4. Опція "Завжди відповідати з моєї адреси за умовчанням" увімкнена.

**Що це дає на практиці:**
- Клієнт заповнює форму → Formsubmit шле лист на `danylo@` з `Reply-To: <email клієнта>`
- Danylo тисне "Відповісти" в Gmail → лист іде клієнту **від `info@importica.com.ua`**
- У полі "Кому" автоматично — email клієнта
- Перевірено end-to-end: лист доходить у Вхідні (не в спам)

**Підводний камінь, який вирішено:**
- Спочатку `info@importica.com.ua` був Google Group — він блокував зовнішні verification email від Gmail. Видалено і пересторено як справжній user alias. Тільки після цього Gmail auto-verified.

---

## Google Analytics GA4

**Статус: ❌ не підключено.**

В `<head>` index.html є закоментований блок. Щоб підключити:
1. analytics.google.com → New Property → Web → importica.com.ua
2. Скопіювати Measurement ID (формат: G-XXXXXXXXXX)
3. Розкоментувати блок в `<head>` і замінити G-XXXXXXXXXX → commit → push

---

## Структура сайту (секції index.html по порядку)

```
1.  Topbar        — телефон | email | telegram (білий текст, жовтий hover)
2.  Header        — SVG лого | навігація (жовті лінки) | кнопка Telegram
3.  Hero          — доставка авто з Клайпеди від $850
4.  SEO-блок      — текст для пошукових систем
5.  Services      — 6 карток послуг
6.  Invoice       — оплата інвойсів за кордоном
7.  Customs       — оцінка митних ризиків + форма (Formsubmit + iframe ✅)
8.  How it works  — 4 кроки доставки авто
9.  Cities        — Київ, Львів, Харків, Дніпро, Одеса + ваше місто
10. Pricing       — ринок $1000-1100 vs Importica $850
11. Calculator    — JS калькулятор вартості доставки
12. FAQ           — 6 питань і відповідей (accordion)
13. CTA Final     — заклик до дії
14. Contacts      — всі контакти
15. Footer        — лого | посилання | © 2026
16. Sticky CTA    — мобільна фіксована панель
```

---

## Дизайн-система (не змінювати!)

```
Фон:            #0d0d0d
Акцент:         #e8ff00 (жовто-зелений)
Текст:          #f5f5f0
Другорядний:    #555 / #666 / #777
Карточки:       фон #1a1a1a | бордер #2a2a2a
Telegram синій: #229ED9
Шрифт заголовки: Unbounded (вага 900)
Шрифт текст:    DM Sans
```

**Стилі навігації (оновлено 21.05.2026):**
- `nav a` — колір `#e8ff00` (брендовий жовтий), hover → `#f5f5f0`
- `.topbar a` — колір `#f5f5f0` (білий), hover → `#e8ff00`

**Правила:**
- Зберігати всі SEO meta-теги при будь-яких змінах
- Нові секції — в тому ж стилі (темний фон, жовтий акцент)
- Клас `fade-up` на секції → анімація появи при скролі
- Після змін: `git add index.html` → `git commit` → `git push origin main`

---

## Що заховано в git (прибрано, легко повернути)

| Елемент | Коли повертати |
|---------|----------------|
| Секція "Про нас" | Коли є реальні цифри (5 років, 100+ доставок, кількість країн) |
| Секція відгуків | Коли є реальні відгуки від клієнтів |
| Instagram кнопки | Коли створено Instagram акаунт |

---

## Пріоритети — що далі

### Зроблено ✅
- [x] Сайт запущено на importica.com.ua
- [x] SEO (meta, OG, Schema.org, sitemap, robots.txt)
- [x] GitHub Pages + Cloudflare DNS + HTTPS
- [x] Google Search Console
- [x] Telegram бот + Make.com автовідповідь
- [x] SVG логотип
- [x] **Formsubmit + iframe + multiple files** (21.05.2026) — повна заміна Web3Forms, підтримка вкладень
- [x] **Gmail Send-as info@importica.com.ua** (21.05.2026) — Workspace alias, за умовчанням
- [x] **Reply-To на email клієнта** (21.05.2026) — натискання "Відповісти" автоматично адресує клієнту
- [x] **Стилізація nav + topbar** (21.05.2026) — жовті лінки nav, білий topbar з жовтим hover
- [x] **Тест end-to-end** (21.05.2026) — форма → email з 4 файлами → відповідь від info@ → клієнт отримав у Вхідні
- [x] Безпека сайту перевірена — все OK

### Термінові ⚡
- [ ] **GA4** — зареєструватись на analytics.google.com, вставити Measurement ID
- [ ] **Постійний телефон** — замінити +380502313652 у 3 місцях (topbar, contacts, footer)

### Середні 📋
- [ ] **Секція "Про нас"** — повернути з реальними даними (5 років, 100+ доставок)
- [ ] **Відгуки** — повернути коли є реальні від клієнтів

### Майбутнє 🔮
- [ ] Instagram акаунт → підключити до сайту
- [ ] og:image (1200×630px) для соцмереж
- [ ] English версія сайту

---

## Файли проєкту

```
importica/
├── index.html       ← ЄДИНИЙ файл сайту (HTML + CSS + JS всередині)
├── robots.txt       ← SEO
├── sitemap.xml      ← SEO
├── CNAME            ← GitHub Pages: importica.com.ua
├── CLAUDE.md        ← Контекст для Claude Code CLI (авто-читається)
├── context.md       ← Цей файл (для Claude.ai Project Knowledge)
├── README.md        ← Документація проєкту
├── DEPLOY-GUIDE.md  ← Гайд по деплою
└── .github/workflows/deploy.yml  ← GitHub Actions CI/CD
```

---

## Типові задачі

**Замінити телефон:**
Шукати `+38 050 231 36 52` — є в 3 місцях: topbar, contacts-strip, footer.

**Додати нову секцію:**
Вставити між існуючими секціями в index.html з класом `fade-up`.

**Оновити сайт:**
```bash
git add index.html
git commit -m "опис змін"
git push origin main
# Деплой автоматичний ~1-2 хвилини
```

**Якщо Formsubmit перестане працювати:**
- Перевірити hash в action формы: `formsubmit.co/56ca8e43c291e01a9138572f3c12ca69`
- Перевірити що iframe `formsubmit-frame` існує перед `</body>`
- Перевірити Gmail спам — може потрапити туди при зміні sender
