# Исследование: интеграция агентного мэша (22.11.2026)

Запрос оператора: «ищи инфу касательно интеграции». Источники прочитаны 22.11: скиллы `agentmemory-config`, `agentmemory-agents`, документация A2A и agentmemory. Только чтение; конфигурация и сеть не менялись.

## 1. A2A (Agent2Agent) — стандарт «агент ↔ агент»

| Факт | Значение |
|---|---|
| Статус | v1.0.0 — 12.03.2026, v1.0.1 — 28.05.2026; production-ready. С 17.08.2026 в Agentic AI Foundation (Linux Foundation); >150 организаций, 22k+ звёзд, SDK на Python/JS/Java/Go/.NET |
| Назначение | Агенты ** discovering** друг друга через Agent Card (`/.well-known/agent-card.json`), договариваются о модальностях, управляют задачами с жизненным циклом, обмениваются артефактами — без доступа к внутренней памяти/инструментам друг друга |
| Простота | В простейшем случае — один HTTP-запрос; результаты — polling / streaming / webhooks |
| Граница | A2A — между разными агентами/frameworks. Для субагентов внутри одного фреймворка это не описано спецификацией; там хендоффы фреймворка |
| Открытый вопрос | Обнаружение конкретного бизнеса/человека, давшего согласие, — вне A2A (IETF DAWN и др.) |

**A2A + MCP:** MCP = вертикаль (агент → инструменты/данные), A2A = горизонталь (агент → агент). Дополняют друг друга, не заменяют.

## 2. agentmemory: два слоя шаринга (по официальной документации)

### Team feed (один инстанс)
- env: `TEAM_MODE=shared`, `TEAM_ID`, `USER_ID`, `AGENT_ID`;
- инструменты: `memory_team_share`, `memory_team_feed`, `GET /agentmemory/team/profile`;
- записи несут provenance «shared», отличаются от собственных;
- работает **между машинами** при одном `TEAM_ID`, если клиенты смотрят на один daemon.

### Mesh sync (между инстансами)
- peer регистрируется через `POST /agentmemory/mesh/peers` с именем и URL;
- **URL должен быть публичным http/https**: localhost, приватные диапазоны и link-local отвергаются, в том числе после DNS-резолва;
- `POST /agentmemory/mesh/sync` с `direction: push|pull|both`; scopes по умолчанию: memories, actions, semantic, procedural, relations, `graph:nodes`, `graph:edges`;
- конфликты — last-write-wins по timestamp записи, под ключевым локом;
- **требует явно заданный `AGENTMEMORY_SECRET` на обоих пирах**; сгенерированный файловый секрет для mesh недостаточен;
- каждая register/sync/receive попадает в audit trail.

### Ограничения расположения
- REST по умолчанию слушает `127.0.0.1:3111`; viewer `3113` — только loopback (доступ через SSH-туннель);
- удалённый доступ: `AGENTMEMORY_URL` + **явный** `AGENTMEMORY_SECRET` + **TLS** (хуки отсылают capture только на loopback или https; на удалённый http — молча дропают);
- секрет хранится в Windows Credential Manager / env, **не в файлах репозитория**.

## 3. Что это меняет для нашего мэша

1. Наша прежняя гипотеза «второй пир на бэкап-сервере в LAN» **не сработает как mesh**: приватные адреса запрещены. Нужны либо публичный HTTPS-адрес/туннель, либо **один общий daemon** с `AGENTMEMORY_URL` + TLS.
2. `mesh_sync` без секрета — не баг конфигурации, а документированное требование; секрет должен быть задан на обеих сторонах.
3. Наш `help-request → help-inbox → help-resolve` — примитивный аналог A2A-делегирования; для реальной интеграции стандарт — A2A Agent Card + задача с жизненным циклом.
4. Наш `projects/registry.json` + `mesh-search` — файловый «mesh» уровня знаний; протоколы (A2A/MCP) — транспорт, а не хранилище. Канон остаётся в git/SEED.

## 4. Рекомендация по этапам (не выполнялось)

- **P1 (ближайший):** один общий agentmemory daemon с `AGENTMEMORY_URL` + TLS + явный secret из Credential Manager; включить team feed (`TEAM_ID`) и `AGENTMEMORY_INJECT_CONTEXT=true` для авто-обогащения контекста на старте сессии. Это даст «подключился — помнишь всё» без приватного peer URL.
- **P2:** A2A Agent Card для SEED-агента, чтобы другие агенты могли делегировать «найди знание/проверь патч»; внутри — MCP-инструменты (`source_search`, `source_get`, `experience_append`).
- **Сохраняем:** файловый канон, 3 корзины, boot lane, T2K-журнал — протоколы меняются, источник остаётся.

Ссылки: `https://a2a-protocol.org/latest/specification/`, `https://a2a-protocol.org/latest/topics/a2a-and-mcp/`, `https://www.agent-memory.dev/docs/sharing`, `/docs/security`, `/docs/deployment`, `agentmemory-config`/`agentmemory-agents` (локальные скиллы).
