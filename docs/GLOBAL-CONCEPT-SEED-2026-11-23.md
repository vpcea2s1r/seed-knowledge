# Глобальный концепт SEED — единый источник знаний и опыта (23.11.2026)

**Статус:** рабочая система P1 развёрнута; концепт развивается автономно по циклу «поиск → анализ → фиксация → проверка».

## 1. Видение

Один переносимый источник, к которому **любая машина, агент или телефон подключаются по требованию** и получают только нужные знания/навыки/опыт. Центр управления — Cloudflare; канон — GitHub; устойчивость — 3 корзины; секреты — только локальное OS-хранилище и бэкап-сервер 700.

## 2. Боли, которые закрывает

1. Новый комп = «учить агента как ребёнка» → `bootstrap` подключает за секунды.
2. Память размазана по 5 слоям → один хозяин на концепцию + `concepts.json` + поиск.
3. Знания разрознены между компами → карточки навыков + загрузка полного текста по требованию.
4. Гибель компа = потеря всего → Hasleo (машина) + SEED (мозг) + ≥3 корзины.
5. Неизвестная машина не знает имени навыка → `representativeQueries` + need-очередь + federation.

## 3. Архитектура (4 слоя)

| Слой | Компоненты | Роль |
|---|---|---|
| Канон | GitHub, `prototypes/seed/source/` | версионируемый, санитизированный источник |
| Центр управления | Cloudflare Worker `seed-edge`, **D1** (индексы), **KV** (тексты), **Queues** (очереди), Turnstile (опционально) | единая точка входа по HTTPS, discovery, onboarding, аудит |
| Клиенты | ноут с OpenCode, 3 iPhone, рабочий комп; MCP Streamable HTTP | подключение по требованию |
| Устойчивость | Hasleo 6.0.2.0, бэкап-сервер 10 ТБ, NAS, `dr-pack` | Restore машины и мозга |

Живой endpoint: `https://seed-edge.2497567.workers.dev`

## 4. Принципы

- **Progressive disclosure:** карточка (name/description/representativeQueries) видна всегда, `SKILL.md` грузится только при совпадении задачи.
- **Zero-trust:** токен устройства `se_…`, SHA-256 в D1, plaintext один раз в OS-хранилище; по умолчанию read-only; publish → `verified=false`.
- **Стандарты вместо велосипедов:** MCP Streamable HTTP + `io.modelcontextprotocol/skills` (SEP-2640), A2A, ARD `representativeQueries`, Agent Skills format.
- **Один писатель**, provenance, `verified` флаг, `supersedes` вместо удаления.
- **3-2-1:** ≥3 корзины, ≥2 провайдера, ≥1 вне машины.
- **Секреты** не попадают в git/облако/чат; санитайзер обязателен.

## 5. Топология теста

| Устройство | Роль | Состояние |
|---|---|---|
| Рабочий комп | publisher (веб/копирование, 1С) | D1/KV наполнены, замеры есть |
| Ноут с OpenCode | consumer + publisher | устройство `laptop-opencode` зарегистрировано |
| iPhone ×3 | consumer (тексты/заметки, согласны владельцы) | `iphone-1..3` зарегистрированы |
| Бэкап-сервер | приватный архив 700, knowledge-web :8090 | работает |

Токены устройств сохранены в Windows Credential Manager (`seed-device-<name>`), admin — `seed-hub-admin`; в чат не выводились.

## 6. Экономика

Все компоненты хаба — **бесплатные**: Workers 100k запросов/день, KV 1 ГБ, D1 5 ГБ, Queues 10k операций/день, GitHub. **R2 не используется** — требует привязки карты даже при $0; заменён KV. Клиенты: бесплатные (Lantern MCP, Joey Lite, MCP Agent, OpenCode, Cline, Goose).

## 7. Зрелость и критерии

| Этап | Критерий | Статус |
|---|---|---|
| P0 | локальный прототип, smoke, manifest, 3 корзины | ✅ |
| P1 | живой Worker, MCP handshake, enroll, 4 устройства, 15 концепций | ✅ |
| P2 | MCP Skills Extension, `/semantic` (Workers AI embeddings 56×384 + hybrid), federation referrals, `/skills/approve` | ✅ всё проверено на живом URL |
| P3 | A2A-делегирование, общий agentmemory daemon, ежеквартальный дрилл | план |

## 8. Риски

- iPhone-клиенты требуют iOS 26 (Lantern/MCP Agent) — проверить версии владельцев; запасной вариант — Joey Lite (iOS 13+).
- Хуки-клиенты не делают автоматического одобрения — инструменты могут вызываться автоматически; токены devices ограничить scopes.
- `io.modelcontextprotocol/skills` реализован и проверен; federation с публичными реестрами и одобрение self-registered навыков ещё не подключены.
- Удалённый `/enroll` включён admin-секретом; при утере — ротация через Credential Manager + `wrangler secret put`.

## 9. Ссылки

`docs/TECHNICAL-SPEC-COMMON-SOURCE.md`, `docs/ON-DEMAND-SKILLS-2026-11-23.md`, `docs/MESH-INTEGRATION-RESEARCH-2026-11-22.md`, `docs/MEMORY-CONSOLIDATION-2026-11-22.md`, `docs/SELF-DEVELOPMENT-CONCEPT-2026-11-22.md`, `docs/RESEARCH-ARCHIVE-INDEX-2026-11-23.md`, `docs/CLIENT-SOFTWARE-FREE-2026-11-23.md`.
