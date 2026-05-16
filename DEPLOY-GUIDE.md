# 🚀 ІНСТРУКЦІЯ: Як викласти сайт на GitHub Pages

## Крок 1 — Встанови Git (якщо ще не встановлений)

Завантаж з: https://git-scm.com/downloads
Встанови з усіма налаштуваннями за замовчуванням.

---

## Крок 2 — Створи акаунт на GitHub

1. Зайди на https://github.com
2. Зареєструйся (якщо ще немає акаунта)
3. Запам'ятай свій USERNAME (він знадобиться)

---

## Крок 3 — Створи репозиторій

1. Натисни зелену кнопку **"New"** або **"+"** → **"New repository"**
2. Repository name: `importica`
3. Зроби **Public** (обов'язково для безкоштовного GitHub Pages)
4. НЕ додавай README (він вже є в нашому проєкті)
5. Натисни **"Create repository"**

---

## Крок 4 — Завантаж файли через термінал

Відкрий термінал (cmd або PowerShell на Windows, Terminal на Mac):

```bash
# Перейди в папку з проєктом
cd шлях/до/папки/importica

# Ініціалізуй Git
git init

# Додай всі файли
git add .

# Перший коміт
git commit -m "Initial commit — Importica website"

# Підключи до GitHub (заміни YOUR-USERNAME на свій нік)
git remote add origin https://github.com/YOUR-USERNAME/importica.git

# Відправ файли
git branch -M main
git push -u origin main
```

---

## Крок 5 — Увімкни GitHub Pages

1. Зайди в репозиторій на GitHub
2. **Settings** (вкладка зверху)
3. Зліва знайди **"Pages"**
4. Source: **"GitHub Actions"** (або Deploy from branch → main → / root)
5. Збережи

Через 1-2 хвилини сайт буде доступний за адресою:
`https://YOUR-USERNAME.github.io/importica`

---

## Крок 6 — Підключи домен importica.com.ua

### 6.1 — Налаштуй DNS у реєстратора домену

Зайди в панель де купував домен і додай такі записи:

| Тип | Ім'я | Значення |
|-----|------|----------|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | YOUR-USERNAME.github.io |

### 6.2 — Додай домен в GitHub Pages

1. Settings → Pages
2. Custom domain → введи `importica.com.ua`
3. Save
4. Увімкни **"Enforce HTTPS"**

⏱️ DNS оновлюється 1-24 години.

---

## Крок 7 — Зареєструй в Google Search Console

1. Зайди: https://search.google.com/search-console
2. Add property → URL prefix → `https://importica.com.ua`
3. Підтверди через HTML тег (вставиш в <head> index.html)
4. Submit sitemap: `https://importica.com.ua/sitemap.xml`

Це прискорить індексацію Google з тижнів до 2-3 днів!

---

## ✅ Чек-лист після публікації

- [ ] Сайт відкривається за https://importica.com.ua
- [ ] HTTPS увімкнений (замочок у браузері)
- [ ] Telegram посилання @importica веде в твій чат
- [ ] Email info@importica.com.ua отримує листи
- [ ] Зареєстрований у Google Search Console
- [ ] Sitemap відправлений в Google

---

## 🔄 Як оновлювати сайт після змін

```bash
# В папці проєкту:
git add .
git commit -m "Опис змін"
git push
```

Сайт оновиться автоматично через ~1 хвилину.

---

## 📞 Якщо щось не виходить

Пиши в цей чат — вирішимо разом покроково! 💪
