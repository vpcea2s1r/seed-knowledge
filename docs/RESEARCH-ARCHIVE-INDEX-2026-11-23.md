# Архив исследований и концепций — индекс (23.11.2026)

Назначение: не искать дважды. Все найденные факты, решения и концепции собраны здесь и продублированы в переносимый источник SEED (`prototypes/seed/source/concepts.json`, D1/KV хаба `https://shambhala.2497567.workers.dev`).

## 1. 1С: обновление, патчи, диагностика

| Документ | Суть |
|---|---|
| `KB-1C.md` | Версия 2.1.38.11, 5 официальных патчей, ИТС-правила установки, расхождение способа |
| `PROD-PATCH-CHECKLIST.md` | Чек-лист выкатки патчей на прод через форму БСП (НЕ выполнялся) |
| `TECHNICAL-SPEC-COMMON-SOURCE.md` | ТЗ SEED: 24 F-требования, DR, федерация, ежедневный контур |
| `POST-UPDATE-BUG-PLAYBOOK-2026-11-20.md` | Классы ошибок E1–E8 после выкатки |
| `IB-LOCK-PROCEDURE-2026-11-20.md` | Блокировка сеансов через COM :1561 |
| `PROTOCOL-STEPS-AND-ERRORS-2026-11-20.md` | Хронология шагов/ошибок (42 КБ) |
| `ITS-RESEARCH.md`, `UPDATER-RESEARCH-2026-11-20.md` | Справка ИТС, «Обновлятор» как вспомогательный канал |
| `TEST-UPDATE-FINAL-CLOSURE-2026-11-19.md`, `RECOVERY-RUNBOOK.md` | GO №9, восстановление |

## 2. Устойчивость и DR

| Документ | Суть |
|---|---|
| `RESILIENCE-ANALYSIS-2026-11-21.md`, `OS-IMAGE-BACKUP-METHODOLOGY-2026-11-21.md` | Образы ОС, Hasleo Backup Suite 6.0.2.0 |
| `MISSED-ITEMS-2026-11-21.md` | Auto-режим, секреты, пробелы |
| `TECHNICAL-SPEC-COMMON-SOURCE.md` §9 | Правило ≥3 корзин; Hasleo = машина, SEED = мозг агента |

## 3. Мэш, федерация, обмен между машинами

| Документ | Суть |
|---|---|
| `AGENT-MESH-PROTOCOL-2026-11-21.md` | Signals/actions/lease/checkpoint; mesh_sync не работает без публичного URL и secret |
| `MESH-INTEGRATION-RESEARCH-2026-11-22.md` | A2A v1.0.1 + MCP дополняют друг друга; agentmemory team feed vs mesh |
| `ON-DEMAND-SKILLS-2026-11-23.md` | **Концепция v2**: навыки по требованию между физически разными компами; MCP Skills Extension SEP-2640 |
| `MEMORY-CONSOLIDATION-2026-11-22.md` | 5 слоёв памяти, один хозяин на концепцию, search по 12+ концепциям |

## 4. Саморазвитие и качество

| Документ | Суть |
|---|---|
| `SELF-LEARNING.md`, `SELF-IMPROVEMENT.md`, `AUDIT-PROTOCOL.md` | INBOX → реестр → дистилляция → метрика; AU-1 каждые 15 задач |
| `LESSONS-INBOX.md`, скилл `session-retro` | Захват ≤3 строк, ретро перед «готово» |
| `SELF-DEVELOPMENT-CONCEPT-2026-11-22.md` | Уровни L0–L4; мы на L2; внешние исследования: −15 п.п. от слепого переписывания скиллов |
| `OPENCODE-TELEMETRY-REFERENCES-2026-11-21.md` | Конвейер стадий и «чеки» |

## 5. Внешние проекты и паттерны

| Документ / источник | Что забрали |
|---|---|
| `VIBE-MACHINE-INTEGRATION-2026-11-21.md`, `ONTOLOGY-T2K-REFERENCE.tql` | T2K-онтология «трасса → знание», стадии + чеки, HITL |
| `GITHUB-AI-TOOLS-ANALYSIS-2026-11-21.md`, `TOOLS-ANALYSIS-2026-11-21.md` | bsl-help, vrunner, agentmemory |
| `CLOUDFLARE-FREE-SERVICES-2026-11-22.md` | 18 бесплатных продуктов; R2 требует карту → заменён KV |
| Внешние (не в docs): `agentskills/agentskills`, `EverMind-AI/SkillCorpus`, `onfire7777/universal-ai-skills-library`, `mozilla-ai/cq`, `letta-ai/skills`, `FlorianBruniaux/claude-code-ultimate-guide`, `vibevm.org`, `modelcontextprotocol.io/extensions/skills` | SKILL.md, lockfile/boot lane, ретриев+eval, T2K/опыт, ACE/A2A, SEP-2640 |

## 6. Живой контур эволюции концепции

1. **Захват:** новая идея/факт → `docs/*.md` + `SEED ideas/catalog.jsonl`.
2. **Поиск:** официальные docs (`developers.cloudflare.com`, `modelcontextprotocol.io`, `agentskills.io`),arxiv, GitHub API, ИТС — по явной команде оператора на интернет.
3. **Анализ:** отделить факт от гипотезы; записать цифру/дату/источник; указать, что заменило прежнее решение (пример: R2 → KV, свой REST → SEP-2640).
4. **Фиксация:** `concepts.json` (canonical + also_in), mcp-memory, Semantica, `ORCHESTRATION-LOG`.
5. **Проверка:** smoke-тест прототипа и живой endpoint хаба.

Непроверенное помечается `verified:false` / «не проверено» и не выдаётся за факт.
