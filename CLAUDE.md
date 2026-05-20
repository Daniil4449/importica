# Importica — Project Context for Claude

## Що це за проєкт

Сайт логістичної компанії **Importica** (importica.com.ua).
Засновник: Danylo | Email: danylo@importica.com.ua
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
| Telegram бот | @importica_bot |
| Make.com | Пересилає повідомлення бота в групу "Importica - Заявки" + автовідповідь |
| Search Console | Підключено, sitemap відправлено 17.05.2026 |

**Контакти на сайті:**
- Телефон: `+38 050 231 36 52` — **ТИМЧАСОВИЙ**, замінити в 3 місцях: topbar, contacts-strip, footer
- Email: info@importica.com.ua
- Telegram: https://t.me/importica_bot

---

## Секції index.html (порядок на сторінці)

1. **Topbar** — телефон, email, @importica (telegram)
2. **Header** — SVG лого, nav, кнопка Telegram
3. **Hero** — доставка авто з Клайпеди від $850, маршрут, ціна, кнопки
4. **SEO-блок** — текстовий блок для пошукових систем
5. **Services** — 6 карток послуг
6. **Invoice** — оплата інвойсів: проблема → кроки → валюти → гарантії
7. **Customs** — оцінка митних ризиків + форма заявки (EmailJS)
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

## EmailJS — форма митних ризиків (Customs секція)

SDK підключено: `cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js`

**Форма збирає:** тип вантажу, країна відправки, вартість за інвойсом, контакт (telegram/email), деталі.
**При успіху:** email на info@importica.com.ua + success-повідомлення клієнту.
**При помилці:** fallback — відкриває Telegram з готовим текстом.

**Що замінити в index.html (3 placeholder значення):**
```js
emailjs.init('YOUR_PUBLIC_KEY');     // emailjs.com → Account → Public Key
emailjs.send('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', params)
```

**Змінні шаблону EmailJS:**
- `{{cargo_type}}` — тип вантажу
- `{{from_country}}` — країна відправки
- `{{invoice_value}}` — вартість за інвойсом
- `{{client_contact}}` — контакт клієнта
- `{{details}}` — деталі

**Кроки налаштування:**
1. emailjs.com → Sign Up → Add New Service → Gmail → підключити info@importica.com.ua
2. Email Templates → Create → вставити змінні → зберегти → скопіювати Template ID
3. Account → API Keys → скопіювати Public Key
4. Вставити всі 3 значення в index.html → commit → push

---

## Google Analytics GA4

В `<head>` index.html є закоментований блок. Потрібно:
1. analytics.google.com → створити акаунт → Property → Web → importica.com.ua
2. Скопіювати Measurement ID (G-XXXXXXXXXX)
3. Розкоментувати блок в index.html і замінити G-XXXXXXXXXX

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
```

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

- [x] **EmailJS** — ЗРОБЛЕНО 20.05.2026 (Public Key: sM063G4NxBe08BwhI, Service: service_4jt8l1t, Template: template_uapgy9w)
- [x] **Видалити main.html** — ЗРОБЛЕНО 19.05.2026
- [ ] **GA4** — зареєструватись, вставити Measurement ID
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
