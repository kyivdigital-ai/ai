# ASK THE LITTLE BOOK — AI version

Это версия с настоящим AI-backend и поиском по полному тексту книги.

## Что внутри

- `index.html` — публичный интерфейс.
- `api/ask.js` — серверный API. API-ключ никогда не попадает в браузер.
- `data/chunks.json` — полный текст книги, разбитый на 382 серверных фрагмента с номерами книжных страниц.
- RAG-поиск (BM25) — сначала находит наиболее релевантные места в книге.
- Claude Sonnet 5 — получает только найденные места и отвечает от лица Little Book.
- Жёсткий prompt запрещает использовать знания вне книги и запрещает пересказывать целые главы.
- История последних вопросов помогает понимать follow-up вроде “what happened after that?”.

## Как запустить на Vercel

1. Распакуй ZIP в отдельную папку.
2. Загрузи эту папку в новый GitHub repository или импортируй её в Vercel.
3. В Anthropic Console создай API key.
4. В Vercel открой **Project → Settings → Environment Variables**.
5. Добавь:

   `ANTHROPIC_API_KEY` = твой ключ

   Опционально:

   `CLAUDE_MODEL` = `claude-sonnet-5`

6. Redeploy проект.
7. После деплоя задай тесты:
   - `What is this book?`
   - `What was your childhood room like?`
   - `What happened to Enerhodar?`
   - `What does sex mean to you?`
   - `What is your favorite Taylor Swift song?`

Последний вопрос должен получить ответ примерно `That isn’t written inside me.`

## Важно про приватность книги

`data/chunks.json` лежит вне публичной части интерфейса и используется серверной функцией. Не переносить его в `/public` и не делать отдельный публичный route к этому файлу.

## Цена

Каждый вопрос делает один вызов Claude API. Стоимость зависит от действующего тарифа Anthropic и модели. Для production можно переключить `CLAUDE_MODEL` на более дешёвую поддерживаемую модель, если качество останется достаточным.

## Что заменить перед публикацией

В `index.html` сейчас кнопка `GET THE BOOK` ведёт на `https://sadgay.com`. Если у тебя есть точная checkout-ссылка, замени оба вхождения URL на неё.
