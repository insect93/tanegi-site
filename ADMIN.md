# Tanegi Site — Admin Guide

## Структура репозитория

```
tanegi-site/
  index.html        — сайт (один файл, всё встроено)
  releases/
    .gitkeep        — чтобы Git трекал пустую папку
    tanegi.zip      — СЮДА кладёшь релизный архив
  ADMIN.md          — этот файл
```

---

## Шаг 1: Первичная настройка GitHub Pages

### 1.1 Создай репозиторий

1. Открой [github.com/new](https://github.com/new)
2. Название репозитория: `tanegi-site` (или любое)
3. Видимость: **Public** (обязательно, иначе Pages не работает на бесплатном аккаунте)
4. Нажми **Create repository**

### 1.2 Загрузи сайт в репозиторий

Открой терминал:

```bash
cd /home/kali/Desktop/tanegi-site

# Инициализируй git
git init
git add .
git commit -m "Initial site"

# Привяжи к GitHub (замени USERNAME на свой ник)
git remote add origin https://github.com/USERNAME/tanegi-site.git
git branch -M main
git push -u origin main
```

### 1.3 Включи GitHub Pages

1. Открой репозиторий на GitHub
2. Перейди **Settings → Pages**
3. В разделе **Source** выбери: **Deploy from a branch**
4. Branch: `main` / `/ (root)`
5. Нажми **Save**

Через 1–2 минуты сайт появится по адресу:
```
https://USERNAME.github.io/tanegi-site/
```

> **Совет**: если хочешь адрес `https://USERNAME.github.io/` (без подпапки) — переименуй репозиторий в `USERNAME.github.io`.

---

## Шаг 2: Загрузка нового релиза tanegi.zip

Каждый раз когда выходит новая версия Tanegi, делай так:

```bash
cd /home/kali/Desktop/tanegi-site

# Положи архив в releases/
cp /путь/к/tanegi.zip releases/tanegi.zip

# Добавь в git и запуши
git add releases/tanegi.zip
git commit -m "Release: tanegi vX.Y.Z"
git push
```

GitHub Pages автоматически обновит сайт. Кнопка **Download** на сайте сразу начнёт отдавать новый файл.

---

## Шаг 3: Обновление версии на сайте (опционально)

В `index.html` найди секцию с версией (около строки 1900):

```html
<div class="dl-meta">
  <span class="dl-version">v1.0.0</span>
  <span class="dl-size">~2.6 MB</span>
  ...
</div>
```

Замени `v1.0.0` на актуальную версию и обнови размер файла:

```bash
ls -lh releases/tanegi.zip   # узнать размер
```

Затем:

```bash
git add index.html
git commit -m "Update version label to vX.Y.Z"
git push
```

---

## Быстрая шпаргалка

| Действие | Команда |
|---|---|
| Первый пуш | `git push -u origin main` |
| Новый релиз | `cp tanegi.zip releases/ && git add releases/tanegi.zip && git commit -m "Release vX" && git push` |
| Обновить сайт | `git add index.html && git commit -m "Update site" && git push` |

---

## Кастомный домен (опционально)

Если хочешь использовать свой домен (например `tanegi.net`):

1. В корне репозитория создай файл `CNAME`:
   ```
   tanegi.net
   ```
2. У своего DNS-провайдера добавь запись:
   - Тип: `CNAME`
   - Имя: `www` (или `@` для apex)
   - Значение: `USERNAME.github.io`
3. В **Settings → Pages → Custom domain** введи `tanegi.net`
4. Включи **Enforce HTTPS**

---

## Если кнопка Download не работает

Кнопка использует `fetch('releases/tanegi.zip', { method: 'HEAD' })` чтобы проверить наличие файла перед скачиванием. Если файл не загружен — показывается предупреждение, скачивание блокируется.

Убедись что:
- Файл находится в `releases/tanegi.zip` (именно это имя!)
- Файл добавлен через `git add` и запушен
- GitHub Pages пересобрал сайт (обычно 1–2 минуты после push)
