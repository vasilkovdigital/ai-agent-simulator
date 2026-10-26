# Симулятор ИИ-агента «Менеджер задач»

Сайт: https://vasilkovdigital.github.io/ai-agent-simulator/

Мобильный веб-симулятор к выступлению «ИИ-агенты — это просто: создаём своего прямо в зале» (IT ALL STAR, Минск, 31 октября).
Зритель сканирует QR-код, за 2–3 минуты проходит 14 экранов создания агента и получает чек-лист, как собрать такого агента в Claude (Cowork) или ChatGPT (Work).

- Статический сайт: без фреймворков, сборки и бэкенда.
- Никаких запросов к API, аналитики и сбора данных. Внешний ресурс один — шрифт DM Sans с Google Fonts; если он не загрузится, используется системный шрифт.
- Состояние хранится только в памяти: после перезагрузки всё начинается сначала.
- Вес всего сайта — около 0,3 МБ.

## Структура

```
index.html                         каркас страницы
style.css                          стили
app.js                             логика экранов; CONFIG — в самом начале файла
assets/poses/*.webp                позы персонажа (пережатые)
skill/menedzher-zadach-claude.md   скилл для Claude (Cowork): промпт, правила, чек-лист
skill/menedzher-zadach-chatgpt.md  скилл для ChatGPT (Work): промпт, правила, чек-лист
.nojekyll                          отключает обработку Jekyll на GitHub Pages
```

## Как посмотреть локально

Откройте `index.html` в браузере. Для копирования в буфер лучше запустить локальный сервер:

```bash
python -m http.server 8080
```

и открыть http://localhost:8080.

## Публикация на GitHub Pages

1. Создайте на GitHub новый публичный репозиторий, например `agent-simulator`.
2. Загрузите в корень репозитория содержимое этой папки (`index.html`, `style.css`, `app.js`, `assets/`, `skill/`, `.nojekyll`, `README.md`) в ветку `main`.
   Через сайт: **Add file → Upload files**, перетащите файлы и папки, нажмите **Commit changes**.
3. Откройте **Settings → Pages**.
4. В блоке **Build and deployment** в поле **Source** выберите **Deploy from a branch**.
5. В поле **Branch** выберите `main` и папку `/ (root)`, нажмите **Save**.
6. Через 1–2 минуты сайт будет доступен по адресу `https://<ваш-логин>.github.io/<имя-репозитория>/`. Ссылка появится вверху страницы **Settings → Pages**.
7. Сделайте QR-код на эту ссылку и проверьте его с телефона.

Если интерфейс GitHub выглядит иначе, сверьтесь с документацией:
https://docs.github.com/ru/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

## Где заменить ссылки

Все ссылки — в объекте `CONFIG` в самом начале `app.js`:

```js
const CONFIG = {
  SKILL_URL_CLAUDE: "https://drive.google.com/file/d/1jBpZ3KKIpmCJCzQIec_DEyhCnp804n8M/view?usp=sharing",
  SKILL_URL_CHATGPT: "https://drive.google.com/file/d/1V4glz2WmuZqJFe6MHd7Nbh2k6sQCDm9B/view?usp=sharing",
  TELEGRAM_URL: "https://t.me/nicholasvasilkov",
  // …ссылки для вкладки «Обо мне»
};
```

- **`SKILL_URL_CLAUDE`** и **`SKILL_URL_CHATGPT`** — кнопка «Скачать как скилл» на последнем экране. Она ведёт на ссылку той вкладки, которая открыта (Claude или ChatGPT), и открывается в новой вкладке браузера. Сейчас это файлы на Google Диске; можно указать и файлы из репозитория: `"skill/menedzher-zadach-claude.md"` и `"skill/menedzher-zadach-chatgpt.md"`.
- **`TELEGRAM_URL`** — кнопка «Мой Telegram: гайды и промпты», открывается в новой вкладке.
- **Вкладка «Обо мне»:** `PERSONAL_TG_URL` (широкая кнопка «Написать мне в Telegram»), `SITE_URL`, `YOUTUBE_URL`, `LINKEDIN_URL`, `X_URL`, `TIKTOK_URL`, `SMARTRESPONSE_URL`. Ссылка со значением `"#"` на странице не показывается.

Термины глоссария лежат в массиве `GLOSSARY` в `app.js`, фото для «Обо мне» — `assets/nikolay.webp`.

Тексты промпта, чек-листа и ответов на 5 частых вопросов лежат там же, в начале `app.js` (`agentPrompt()`, `checklist()`, `FAQ`). Промпт `agentPrompt()` совпадает с разделом 2 обоих скиллов — если меняете его, поправьте и `skill/menedzher-zadach-claude.md`, и `skill/menedzher-zadach-chatgpt.md` (и файлы на Google Диске).
