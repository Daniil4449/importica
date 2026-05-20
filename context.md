# Importica — Project Knowledge

**Оновлено:** 20.05.2026

---

## Компанія

**Importica** — міжнародна логістична компанія.
- Сайт: https://importica.com.ua
- Засновник: Danylo Denisyuk
- Email: danylo@importica.com.ua / info@importica.com.ua
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

| Сервіс | Деталі |
|--------|--------|
| Сайт | Один файл: index.html (весь HTML+CSS+JS вбудований) |
| Хостинг | GitHub Pages — github.com/Daniil4449/importica |
| Deploy | git push main → GitHub Actions → автодеплой |
| Домен | importica.com.ua (реєстратор: Imena.ua) |
| DNS | Cloudflare |
| SSL | Автоматично (GitHub Pages + Cloudflare) |
| Email | Google Workspace → info@importica.com.ua |
| Telegram бот | @importica_bot (токен є у власника) |
| Make.com | Автопересилання → група "Importica - Заявки" + автовідповідь клієнту |
| Search Console | Підключено, sitemap відправлено 17.05.2026 |
| Analytics | GA4 — НЕ ПІДКЛЮЧЕНО (placeholder є в коді) |
| EmailJS | SDK підключено, ключі НЕ ВСТАВЛЕНО (placeholder є в коді) |

---

## Структура сайту (секції index.html)

```
1.  Topbar        — телефон | email | telegram
2.  Header        — SVG лого | навігація | кнопка Telegram
3.  Hero          — доставка авто з Клайпеди від $850
4.  SEO-блок      — текст для пошукових систем
5.  Services      — 6 карток послуг
6.  Invoice       — оплата інвойсів за кордоном
7.  Customs       — оцінка митних ризиків + ФОРМА (EmailJS)
8.  How it works  — 4 кроки доставки авто
9.  Cities        — Київ, Львів, Харків, Дніпро, Одеса + ваше місто
10. Pricing       — ринок $1000-1100 vs Importica $850
11. Calculator    — JS калькулятор вартості доставки
12. FAQ           — 6 питань і відповідей
13. CTA Final     — заклик до дії
14. Contacts      — всі контакти
15. Footer        — лого | посилання | © 2026
16. Sticky CTA    — мобільна фіксована панель
```

---

## Дизайн-система

```
Фон основний:   #0d0d0d
Акцент:         #e8ff00  (жовто-зелений)
Текст:          #f5f5f0
Другорядний:    #555 / #666 / #777
Карточки:       фон #1a1a1a | бордер #2a2a2a
Telegram:       #229ED9
Шрифт заголовки: Unbounded (900)
Шрифт текст:    DM Sans
```

**Правила при редагуванні:**
- Зберігати всі SEO meta-теги (title, description, OG, Schema.org)
- Нові секції — в тому ж стилі (темний фон, жовтий акцент)
- Після змін: `git add index.html` → `git commit` → `git push origin main`

---

## EmailJS — форма митних ризиків

**Статус:** SDK підключено, ключі відсутні (форма не відправляє email).

**Що потрібно зробити:**
1. emailjs.com → Sign Up → Add Email Service → Gmail → info@importica.com.ua
2. Email Templates → Create New Template
3. Вставити в шаблон змінні: `{{cargo_type}}`, `{{from_country}}`, `{{invoice_value}}`, `{{client_contact}}`, `{{details}}`
4. Замінити в index.html:
   - `YOUR_PUBLIC_KEY` → emailjs.com → Account → API Keys
   - `YOUR_SERVICE_ID` → Email Services → Service ID
   - `YOUR_TEMPLATE_ID` → Email Templates → Template ID

**Логіка форми:** успіх → email + success UI | помилка → fallback Telegram

---

## Google Analytics GA4

**Статус:** закоментований placeholder в `<head>` index.html.

**Що потрібно:**
1. analytics.google.com → New Property → Web → importica.com.ua
2. Скопіювати Measurement ID (G-XXXXXXXXXX)
3. Розкоментувати блок в `<head>` і замінити G-XXXXXXXXXX

---

## Що заховано в git (можна повернути)

| Елемент | Коли повертати |
|---------|----------------|
| Секція "Про нас" | Є реальні цифри: 5 років досвіду, 100+ доставок |
| Секція відгуків (3 картки) | Є реальні відгуки від клієнтів |
| Instagram кнопки | Створено Instagram акаунт |

---

## Пріоритети — що робити далі

### Термінові:
- [ ] **EmailJS** — зареєструватись і вставити 3 ключі в index.html
- [ ] **GA4** — зареєструватись і вставити Measurement ID
- [ ] **Постійний телефон** — замінити +380502313652 у topbar, contacts, footer

### Середні:
- [ ] **Секція "Про нас"** — повернути з реальними даними (5 років, 100+)
- [ ] **Відгуки** — повернути коли будуть реальні від клієнтів

### Майбутнє:
- [ ] Instagram акаунт → підключити до сайту
- [ ] og:image (1200×630px) для соцмереж
- [ ] English версія сайту

---

## Файли проєкту

```
importica/
├── index.html       ← ЄДИНИЙ файл сайту
├── robots.txt       ← SEO
├── sitemap.xml      ← SEO
├── CNAME            ← GitHub Pages домен
├── CLAUDE.md        ← Контекст для Claude Code CLI
├── context.md       ← Цей файл (Project Knowledge)
├── README.md        ← Документація
├── DEPLOY-GUIDE.md  ← Гайд по деплою
└── .github/workflows/deploy.yml  ← CI/CD
```

---

## Типові задачі

**Замінити телефон:**
Шукати `+38 050 231 36 52` — є в 3 місцях: topbar, contacts-strip, footer.

**Додати нову секцію:**
Вставити між існуючими секціями в index.html. Клас `fade-up` дає анімацію появи при скролі.

**Оновити сайт:**
```bash
git add index.html
git commit -m "опис змін"
git push origin main
```
Деплой автоматичний через GitHub Actions (~1-2 хвилини).
