# Бесплатные клиенты для подключения к SEED (23.11.2026)

Endpoint: `https://shambhala.2497567.workers.dev/mcp` (MCP Streamable HTTP, Bearer-token устройства).

## iPhone (бесплатные варианты)

| Приложение | Цена | iOS | MCP | Комментарий |
|---|---|---|---|---|
| **Lantern MCP** | free до 3 серверов (Pro $1.99 разово) | 26+ | Streamable HTTP, 2026-07-28, bearer/OAuth, Keychain, без аккаунта | **лучший для теста**: проверяет протокол и показывает JSON-RPC |
| **MCP Agent** | free (Pro $2.99/мес за безлимит и голос) | 26+ | Streamable HTTP, bearer/OAuth, Siri/Shortcuts | голос и Siri, write-действия подтверждаются |
| **Joey MCP Client Lite** | free (Premium $4.99 разово) | 13+ | remote HTTP, OpenRouter/свой OpenAI-совместимый ключ | запасной вариант для старых iOS; 1 сервер в Lite |
| **MCPy** | бесплатный базовый уровень — уточнить в App Store | — | любой MCP-сервер, auth headers | универсальный чат; проверить условия тарифа |
| **ReqPad** | free (MCP только в Pro) | — | MCP + REST | не подходит как бесплатный MCP |

Платные, но упомянуты для полноты: Vanta (разово), MCP Explorer Mobile ($0.99), Orvena ($14.99), Chapper Pro — **не выбираем**.

**Настройка на iPhone (одинаково для Lantern/MCP Agent):**
1. Установить приложение из App Store.
2. Add server → URL `https://shambhala.2497567.workers.dev/mcp`, transport **Streamable HTTP**.
3. Auth → Bearer token; взять токен из Windows Credential Manager `seed-device-iphone-1/2/3` (передать владельцу безопасным каналом, не в чат).
4. Connect → проверить список инструментов (`source_search`, `source_get`, `skill_discover`, `source_help`).
5. Спросить задачу текстом: «найди правила копирования сайта» — сработает `/discover` через `skill_discover`.

## Ноут с OpenCode (бесплатно)

OpenCode V2 — нативный MCP-клиент, самый короткий путь. Конфиг (`~/.config/opencode/opencode.jsonc` или проект `.opencode/opencode.jsonc`):

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "servers": {
      "seed-edge": {
        "type": "remote",
        "url": "https://shambhala.2497567.workers.dev/mcp",
        "oauth": false,
        "protocol": "auto",
        "headers": {
          "Authorization": "Bearer {env:SEED_HUB_TOKEN}"
        }
      }
    }
  }
}
```

`SEED_HUB_TOKEN` — токен устройства `laptop-opencode` из Credential Manager; **не писать в конфиг**. Проверка: `opencode mcp list` → `connected`.

Альтернативы (тоже free): **Cline** (Apache-2.0, VS Code), **Continue** (Apache-2.0), **Goose** (Apache-2.0, desktop/CLI), **Claude Desktop** free plan (1 custom connector), **LibreChat/Open WebUI** (self-hosted, MIT) — если нужен общий веб-чат на команду.

## Важно

- Токены устройств одноразово показываются при `/enroll`, хранятся в Keychain/Credential Manager; при компрометации — отзыв по `device_id`.
- Инструменты хаба read-only; `publish` создаёт навык с `verified=false` до одобрения оператором.
- Автоматического одобрения tool call в iPhone-клиентах нет — проверять настройки приложения.
- `/mcp` реализует полный MCP handshake v2026-07-28 + `io.modelcontextprotocol/skills` (`server/discover`, `skills/list`, `skills/get`, `resources/read` с SHA-256).
