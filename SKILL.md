---
name: seed-connect
description: Connect an agent to the SEED live knowledge hub over MCP Streamable HTTP. Use when you need shared skills, facts or experience.
---

# Connect to SEED

1. Read `llms.txt` and the relevant doc under `docs/`.
2. Call the MCP endpoint: https://seed-edge.2497567.workers.dev/mcp (Streamable HTTP).
3. Use `skills/list`, `skills/get`, `resources/read` for on-demand skills; `source_search` / `semantic_search` for concepts.
4. Never place tokens in this repository; store them in the OS credential store.
