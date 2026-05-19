# Importica — importica.com.ua

Міжнародна логістика та доставка вантажів. Доставка авто з Клайпеди від $850.

---

## Структура проєкту

```
importica/
├── index.html              ← ЄДИНА актуальна сторінка (весь сайт тут)
├── main.html               ← ЗАСТАРІЛО — можна видалити
├── robots.txt              ← SEO
├── sitemap.xml             ← SEO
├── CNAME                   ← GitHub Pages: importica.com.ua
├── DEPLOY-GUIDE.md         ← Гайд по деплою
└── .github/
    └── workflows/
        └── deploy.yml      ← GitHub Actions CI/CD
```

> CSS та JS — вбудовані в index.html. Окремих папок css/ та js/ немає.

---

## Інфраструктура

| Сервіс | Деталі |
|--------|--------|
| Хостинг | GitHub Pages (безкоштовно) |
| Домен | importica.com.ua (Imena.ua) |
| DNS | Cloudflare |
| Email | Google Workspace → info@importica.com.ua |
| Telegram | @importica_bot (Make.com автовідповідь) |
| Search Console | Підключено, sitemap відправлено |
| Analytics | GA4 — не налаштовано (placeholder в head) |
| Форма | EmailJS — ключі не вставлено (YOUR_PUBLIC_KEY і т.д.) |

---

## Секції сайту (index.html)

1. Topbar — телефон, email, telegram
2. Header — SVG лого, навігація, кнопка Telegram
3. Hero — доставка авто з Клайпеди від $850
4. SEO-блок
5. Services — 6 послуг
6. Invoice — оплата інвойсів
7. Customs — оцінка митних ризиків + форма (EmailJS)
8. How it works — 4 кроки
9. Cities — міста доставки
10. Pricing — порівняння цін
11. Calculator — JS калькулятор
12. FAQ — 6 питань
13. CTA Final
14. Contacts
15. Footer
16. Sticky CTA (мобільна)

---

## EmailJS — що вставити

Файл: `index.html`

```js
emailjs.init('YOUR_PUBLIC_KEY');       // → emailjs.com → Account
emailjs.send('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', params)
```

Змінні шаблону: `{{cargo_type}}`, `{{from_country}}`, `{{invoice_value}}`, `{{client_contact}}`, `{{details}}`

---

## Google Analytics GA4

В `<head>` є закоментований блок. Розкоментувати і замінити `G-XXXXXXXXXX`.

---

## Що зарезервовано в git (прибрано, можна повернути)

- Секція "Про нас" — 5 років досвіду, 100+ доставок
- Секція відгуків — 3 картки
- Instagram кнопки

---

## Roadmap

- [x] Одно-сторінковий сайт з усіма секціями
- [x] SEO (meta, OG, Schema.org, sitemap, robots.txt)
- [x] GitHub Pages + Cloudflare DNS + HTTPS
- [x] Google Search Console
- [x] Telegram бот + Make.com автовідповідь
- [x] SVG логотип
- [x] EmailJS SDK підключено
- [ ] EmailJS ключі вставити (реєстрація на emailjs.com)
- [ ] Google Analytics GA4 активувати
- [ ] Замінити тимчасовий телефон
- [ ] Секція "Про нас" з реальними даними
- [ ] Відгуки реальних клієнтів
- [ ] Instagram акаунт + підключення
- [ ] og:image для соцмереж
- [ ] English версія сайту
