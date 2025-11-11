
## README.md

```markdown
# DeltaYurt

[![Netlify Status](https://api.netlify.com/api/v1/badges/PROJECT_BADGE/deploy-status)](#)
![Build](https://img.shields.io/badge/build-CI-green)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

**Live:** https://deltayurt.netlify.app

DeltaYurt — живой онлайн‑класс по физике: интерактивные симуляции, многоязычие EN/RU/KY, мгновенная обратная связь.

## Demo
> Вставь GIF/видео 10–20c из `docs/demo.gif`.

## Стек
- React + TypeScript
- Vite (или Next/Vite — уточни)
- Socket.IO (реалтайм)
- i18n JSON (EN/RU/KY)
- Netlify (хостинг)

## Архитектура
```

src/
app/               # провайдеры, роутер
features/          # модули уроков и симов
entities/          # модели (Lesson, User, Sim)
shared/            # ui, lib, hooks, api
i18n/              # en.json, ru.json, ky.json

````

## Метрики (на сегодня)
- 200+ learners
- X уроков/мес
- Avg session: Y минут
- Retention D7: Z%

## Как запустить за 60 секунд
```bash
node -v   # >= 20
npm ci || npm install
npm run dev
````

Открой [http://localhost:5173](http://localhost:5173) (или порт фреймворка).

## Сборка и деплой

```bash
npm run build
```

Netlify собирает из `main`. Для PR — Deploy Previews.

## Локализация

* Все строки — только ключи i18n
* Скрипт проверки «нет пропусков» (todo)

## Безопасность и приватность

См. [SECURITY.md](SECURITY.md). Нет трекинга PII. Web Vitals только агрегировано.

## Дорожная карта

* [ ] Тесты: vitest + @testing-library/react
* [ ] e2e: Playwright
* [ ] Sentry для фронта
* [ ] PWA (оффлайн‑кэш основных симуляций)

## Лицензия

MIT — см. [LICENSE](LICENSE).

````

---

## LICENSE (MIT)
```text
MIT License

Copyright (c) 2025 kabylovtl

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
````

---

## netlify.toml

```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/_/*"
  to = "/index.html"
  status = 200
  force = true

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

# Security headers
[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-Content-Type-Options = "nosniff"
    Referrer-Policy = "strict-origin-when-cross-origin"
    Strict-Transport-Security = "max-age=31536000; includeSubDomains; preload"
    Content-Security-Policy = "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; font-src 'self' data:; connect-src 'self' https:; frame-ancestors 'none';"

# Caching
[[headers]]
  for = "/assets/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/index.html"
  [headers.values]
    Cache-Control = "no-cache"
```

---

## .github/workflows/ci.yml

```yaml
name: ci
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
      - name: Install
        run: |
          npm ci || npm install
      - name: Lint
        run: |
          if npm run -s | grep -q '^lint$'; then npm run lint; else echo 'no lint'; fi
      - name: Typecheck
        run: |
          if npm run -s | grep -q '^typecheck$'; then npm run typecheck; else echo 'no typecheck'; fi
      - name: Test
        run: |
          if npm run -s | grep -q '^test$'; then npm test -- --coverage; else echo 'no tests'; fi
      - name: Build
        run: |
          if npm run -s | grep -q '^build$'; then npm run build; else echo 'no build'; fi
```

---

## CODE_OF_CONDUCT.md

```markdown
# Code of Conduct

Мы придерживаемся [Contributor Covenant](https://www.contributor-covenant.org/), версия 2.1.

Оскорбления, дискриминация и домогательства неприемлемы. Нарушения сообщайте на `maintainer@deltayurt.example`.
```

---

## SECURITY.md

```markdown
# Security Policy

## Supported Versions
Текущее `main`.

## Reporting a Vulnerability
Пишите на `security@deltayurt.example` с шагами воспроизведения. Мы ответим в течение 7 дней. Не публикуйте детали до фикса.
```

---

## CONTRIBUTING.md

```markdown
# Contributing

1. Форк → ветка `feature/<short>`
2. `npm ci` → `npm run dev`
3. Тесты: `npm test` (если есть)
4. PR в `main` с описанием и скрином/видео

Стиль: TypeScript strict, ESLint + Prettier.
```

---

## .github/ISSUE_TEMPLATE/bug_report.md

```markdown
---
name: Bug report
about: Report a problem
labels: bug
---

**Describe**

**Steps**
1.
2.
3.

**Expected**

**Actual**

**Env**
- OS/Browser
- Commit/Version
```

---

## .github/ISSUE_TEMPLATE/feature_request.md

```markdown
---
name: Feature request
about: Suggest an idea
labels: enhancement
---

**Problem**

**Proposal**

**Alternatives**

**Additional context**
```

---

## PULL_REQUEST_TEMPLATE.md

```markdown
## Summary

## Changes

## Screenshots / Video

## Checklist
- [ ] Lint/Typecheck
- [ ] Tests (если применимо)
- [ ] Docs/README обновлены
```

## README.md — Full replacement

````markdown
# DeltaYurt — Live Classroom Platform

[![Netlify Status](https://api.netlify.com/api/v1/badges/PROJECT_BADGE/deploy-status)](#)
![Build](https://img.shields.io/badge/build-CI-green)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

**Live:** https://deltayurt.netlify.app
**Repo:** https://github.com/kabylovtl-dotcom/DeltaYurt

Интерактивные живые уроки, симуляции и задания. Локализация: EN / RU / KY.

---

## 🚀 Быстрый старт

```bash
# Требования
node -v    # >= 20

# Установка
npm ci || npm install
cd server && npm ci || npm install && cd ..

# Запуск (два терминала)
cd server && npm run dev         # API  http://localhost:3005
# во втором терминале
npm run dev:frontend             # FE   http://localhost:8081

# Альтернатива (если есть скрипт)
npm run dev:both
````

### Переменные окружения

Создай `.env` и `server/.env` при необходимости.

```ini
# server/.env
PORT=3005
NODE_ENV=development
```

---

## 🧭 Структура проекта

```
src/
  components/{classroom,teacher,student,ui}
  store/            # Zustand
  types/
  pages/
server/
  index.ts          # Express + Socket.IO
  seed.ts           # тестовые данные
public/locales/{en,ru,ky}/{common,teacher,student,sim}.json
```

---

## 🌍 i18n

* Переключатель языка в UI. Выбор сохраняется в localStorage.
* Добавить новый язык:

```bash
mkdir -p public/locales/{lang}/{common,teacher,student,sim}
cp public/locales/en/*.json public/locales/{lang}/
```

Обнови `src/i18n.ts` и `src/components/ui/LanguageSwitcher.tsx`.

---

## ✨ Основные функции

* Управление классами, коды подключения, статистика.
* Живая комната: чат, «поднять руку», управление симуляцией, очередь выступлений.
* Календарь уроков/дедлайнов с фильтрами.
* Профили, достижения, лидерборд.
* Темная/светлая тема, системный режим.

**Стек:** React + TypeScript + Vite + Tailwind + shadcn/ui + Zustand + Framer Motion + Recharts; Realtime: Socket.IO; Backend: Express + TS.

---

## 📡 API и события (кратко)

REST:

```
GET /api/classes/:code
POST /api/homeworks/:homeworkId/grade
```

Socket.IO события: `register_user`, `join_class`, `teacher_start_lesson`, `teacher_present_simulation`, `chat_message`, `raise_hand`, `grade_submission`, и др.

---

## 🧪 Демо-аккаунты (локально)

Учитель: `teacher@deltayurt.test` / `password123`
Студенты: `student1@deltayurt.test` / `password123`
Код класса: `DY-TEST1`

> На проде не публикуй реальные пароли. Для демо используй `server/seed.ts`.

---

## 🔐 Безопасность и приватность

* Нет PII‑трекинга; Web Vitals только агрегировано.
* Политики и контакты в `SECURITY.md`.

---

## 📈 Метрики (для поступления)

Вынеси в `docs/metrics.md` и кратко дублируй здесь:

* **200+ learners**, X уроков/мес, Avg session **Y** мин, D7 retention **Z%**.
  Добавь 3–4 скрина + короткий demo‑GIF.

---

## 🧰 Скрипты npm (рекомендуется)

```jsonc
{
  "scripts": {
    "dev:frontend": "vite", 
    "dev:both": "run-p -l dev:server dev:frontend", 
    "build": "vite build", 
    "typecheck": "tsc -p tsconfig.json --noEmit", 
    "lint": "eslint .", 
    "test": "vitest run --coverage"
  }
}
```

---

## 🧭 Дорожная карта

* [ ] Тесты: vitest + @testing-library/react; e2e: Playwright.
* [ ] Netlify Deploy Previews для PR.
* [ ] Sentry performance.
* [ ] Проверка i18n на пропуски ключей.

---

## 🤝 Вклад и лицензия

См. `CONTRIBUTING.md`. Лицензия MIT в `LICENSE`.

---

## ⚙️ Badge Netlify

Netlify → **Site settings → Status badges** → замени `PROJECT_BADGE` на реальный ID.

```
```
Ниже готовые файлы. Скопируй их в корень репозитория `kabylovtl-dotcom/DeltaYurt` с сохранением путей. Потом создай PR `admission-polish` → `main`.

---

## README.md

```markdown
# DeltaYurt

[![Netlify Status](https://api.netlify.com/api/v1/badges/PROJECT_BADGE/deploy-status)](#)
![Build](https://img.shields.io/badge/build-CI-green)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

**Live:** https://deltayurt.netlify.app

DeltaYurt — живой онлайн‑класс по физике: интерактивные симуляции, многоязычие EN/RU/KY, мгновенная обратная связь.

## Demo
> Вставь GIF/видео 10–20c из `docs/demo.gif`.

## Стек
- React + TypeScript
- Vite (или Next/Vite — уточни)
- Socket.IO (реалтайм)
- i18n JSON (EN/RU/KY)
- Netlify (хостинг)

## Архитектура
```

src/
app/               # провайдеры, роутер
features/          # модули уроков и симов
entities/          # модели (Lesson, User, Sim)
shared/            # ui, lib, hooks, api
i18n/              # en.json, ru.json, ky.json

````

## Метрики (на сегодня)
- 200+ learners
- X уроков/мес
- Avg session: Y минут
- Retention D7: Z%

## Как запустить за 60 секунд
```bash
node -v   # >= 20
npm ci || npm install
npm run dev
````

Открой [http://localhost:5173](http://localhost:5173) (или порт фреймворка).

## Сборка и деплой

```bash
npm run build
```

Netlify собирает из `main`. Для PR — Deploy Previews.

## Локализация

* Все строки — только ключи i18n
* Скрипт проверки «нет пропусков» (todo)

## Безопасность и приватность

См. [SECURITY.md](SECURITY.md). Нет трекинга PII. Web Vitals только агрегировано.

## Дорожная карта

* [ ] Тесты: vitest + @testing-library/react
* [ ] e2e: Playwright
* [ ] Sentry для фронта
* [ ] PWA (оффлайн‑кэш основных симуляций)

## Лицензия

MIT — см. [LICENSE](LICENSE).

````

---

## LICENSE (MIT)
```text
MIT License

Copyright (c) 2025 kabylovtl

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
````

---

## netlify.toml

```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/_/*"
  to = "/index.html"
  status = 200
  force = true

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

# Security headers
[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-Content-Type-Options = "nosniff"
    Referrer-Policy = "strict-origin-when-cross-origin"
    Strict-Transport-Security = "max-age=31536000; includeSubDomains; preload"
    Content-Security-Policy = "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; font-src 'self' data:; connect-src 'self' https:; frame-ancestors 'none';"

# Caching
[[headers]]
  for = "/assets/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/index.html"
  [headers.values]
    Cache-Control = "no-cache"
```

---

## .github/workflows/ci.yml

```yaml
name: ci
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
      - name: Install
        run: |
          npm ci || npm install
      - name: Lint
        run: |
          if npm run -s | grep -q '^lint$'; then npm run lint; else echo 'no lint'; fi
      - name: Typecheck
        run: |
          if npm run -s | grep -q '^typecheck$'; then npm run typecheck; else echo 'no typecheck'; fi
      - name: Test
        run: |
          if npm run -s | grep -q '^test$'; then npm test -- --coverage; else echo 'no tests'; fi
      - name: Build
        run: |
          if npm run -s | grep -q '^build$'; then npm run build; else echo 'no build'; fi
```

---

## CODE_OF_CONDUCT.md

```markdown
# Code of Conduct

Мы придерживаемся [Contributor Covenant](https://www.contributor-covenant.org/), версия 2.1.

Оскорбления, дискриминация и домогательства неприемлемы. Нарушения сообщайте на `maintainer@deltayurt.example`.
```

---

## SECURITY.md

```markdown
# Security Policy

## Supported Versions
Текущее `main`.

## Reporting a Vulnerability
Пишите на `security@deltayurt.example` с шагами воспроизведения. Мы ответим в течение 7 дней. Не публикуйте детали до фикса.
```

---

## CONTRIBUTING.md

```markdown
# Contributing

1. Форк → ветка `feature/<short>`
2. `npm ci` → `npm run dev`
3. Тесты: `npm test` (если есть)
4. PR в `main` с описанием и скрином/видео

Стиль: TypeScript strict, ESLint + Prettier.
```

---

## .github/ISSUE_TEMPLATE/bug_report.md

```markdown
---
name: Bug report
about: Report a problem
labels: bug
---

**Describe**

**Steps**
1.
2.
3.

**Expected**

**Actual**

**Env**
- OS/Browser
- Commit/Version
```

---

## .github/ISSUE_TEMPLATE/feature_request.md

```markdown
---
name: Feature request
about: Suggest an idea
labels: enhancement
---

**Problem**

**Proposal**

**Alternatives**

**Additional context**
```

---

## PULL_REQUEST_TEMPLATE.md

```markdown
## Summary

## Changes

## Screenshots / Video

## Checklist
- [ ] Lint/Typecheck
- [ ] Tests (если применимо)
- [ ] Docs/README обновлены
```

## README.md — Full replacement

````markdown
# DeltaYurt — Live Classroom Platform

[![Netlify Status](https://api.netlify.com/api/v1/badges/PROJECT_BADGE/deploy-status)](#)
![Build](https://img.shields.io/badge/build-CI-green)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

**Live:** https://deltayurt.netlify.app
**Repo:** https://github.com/kabylovtl-dotcom/DeltaYurt

Интерактивные живые уроки, симуляции и задания. Локализация: EN / RU / KY.

---

## 🚀 Быстрый старт

```bash
# Требования
node -v    # >= 20

# Установка
npm ci || npm install
cd server && npm ci || npm install && cd ..

# Запуск (два терминала)
cd server && npm run dev         # API  http://localhost:3005
# во втором терминале
npm run dev:frontend             # FE   http://localhost:8081

# Альтернатива (если есть скрипт)
npm run dev:both
````

### Переменные окружения

Создай `.env` и `server/.env` при необходимости.

```ini
# server/.env
PORT=3005
NODE_ENV=development
```

---

## 🧭 Структура проекта

```
src/
  components/{classroom,teacher,student,ui}
  store/            # Zustand
  types/
  pages/
server/
  index.ts          # Express + Socket.IO
  seed.ts           # тестовые данные
public/locales/{en,ru,ky}/{common,teacher,student,sim}.json
```

---

## 🌍 i18n

* Переключатель языка в UI. Выбор сохраняется в localStorage.
* Добавить новый язык:

```bash
mkdir -p public/locales/{lang}/{common,teacher,student,sim}
cp public/locales/en/*.json public/locales/{lang}/
```

Обнови `src/i18n.ts` и `src/components/ui/LanguageSwitcher.tsx`.

---

## ✨ Основные функции

* Управление классами, коды подключения, статистика.
* Живая комната: чат, «поднять руку», управление симуляцией, очередь выступлений.
* Календарь уроков/дедлайнов с фильтрами.
* Профили, достижения, лидерборд.
* Темная/светлая тема, системный режим.

**Стек:** React + TypeScript + Vite + Tailwind + shadcn/ui + Zustand + Framer Motion + Recharts; Realtime: Socket.IO; Backend: Express + TS.

---

## 📡 API и события (кратко)

REST:

```
GET /api/classes/:code
POST /api/homeworks/:homeworkId/grade
```

Socket.IO события: `register_user`, `join_class`, `teacher_start_lesson`, `teacher_present_simulation`, `chat_message`, `raise_hand`, `grade_submission`, и др.

---

## 🧪 Демо-аккаунты (локально)

Учитель: `teacher@deltayurt.test` / `password123`
Студенты: `student1@deltayurt.test` / `password123`
Код класса: `DY-TEST1`

> На проде не публикуй реальные пароли. Для демо используй `server/seed.ts`.

---

## 🔐 Безопасность и приватность

* Нет PII‑трекинга; Web Vitals только агрегировано.
* Политики и контакты в `SECURITY.md`.

---

## 📈 Метрики (для поступления)

Вынеси в `docs/metrics.md` и кратко дублируй здесь:

* **200+ learners**, X уроков/мес, Avg session **Y** мин, D7 retention **Z%**.
  Добавь 3–4 скрина + короткий demo‑GIF.

---

## 🧰 Скрипты npm (рекомендуется)

```jsonc
{
  "scripts": {
    "dev:frontend": "vite", 
    "dev:both": "run-p -l dev:server dev:frontend", 
    "build": "vite build", 
    "typecheck": "tsc -p tsconfig.json --noEmit", 
    "lint": "eslint .", 
    "test": "vitest run --coverage"
  }
}
```

---

## 🧭 Дорожная карта

* [ ] Тесты: vitest + @testing-library/react; e2e: Playwright.
* [ ] Netlify Deploy Previews для PR.
* [ ] Sentry performance.
* [ ] Проверка i18n на пропуски ключей.

---

## 🤝 Вклад и лицензия

См. `CONTRIBUTING.md`. Лицензия MIT в `LICENSE`.

---

## ⚙️ Badge Netlify

Netlify → **Site settings → Status badges** → замени `PROJECT_BADGE` на реальный ID.

```
```
