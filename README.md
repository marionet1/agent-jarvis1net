# jarvis1net (agent-jarvis1net)

Python AI assistant runtime for OpenRouter with Telegram and CLI entry points.
It connects to `mcp-jarvis1net` through stdio.

Related repositories:
- [stack-jarvis1net](https://github.com/marionet1/stack-jarvis1net)
- [mcp-jarvis1net](https://github.com/marionet1/mcp-jarvis1net)

## What this agent provides

- Chat responses via OpenRouter.
- Telegram bot mode (`src/telegram_bot.py`) and CLI mode (`src/main.py`).
- MCP tool calls (filesystem, shell diagnostics, Microsoft Graph tools).
- Runtime session context and JSONL audit logging.
- Optional Microsoft Graph integration (MSAL device code flow).

## Requirements

- Python 3.12+
- OpenRouter API key (`OPENROUTER_API_KEY` or `/jarvis-set-openrouter-key`)
- A reachable MCP stdio server command configured in `.env`

## Recommended run mode (Docker stack)

From the parent `stack-jarvis1net` repository:

```bash
cp agent-jarvis1net/.env.example .env
docker compose build
docker compose up -d
```

The compose service name is `jarvis1net`.

Run CLI inside the same image:

```bash
docker compose run --rm jarvis1net python3 src/main.py
```

## Local run (agent only)

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -c "import json,subprocess,sys; d=json.load(open('requirements.json', encoding='utf-8')); subprocess.check_call([sys.executable,'-m','pip','install',*d['python_dependencies']])"
cp .env.example .env
python3 src/main.py
```

If running this repository outside the Docker stack, make sure `.env` points to the MCP server:

```dotenv
MCP_STDIO_COMMAND=python3
MCP_STDIO_ARGS=["/absolute/path/to/mcp-jarvis1net/src/server.py"]
```

## Telegram commands

- `/start`, `/help`
- `/info`
- `/jarvis-config-check`
- `/jarvis-set-openrouter-key ...`
- `/jarvis-config-reset`
- `/restart`
- `/jarvis-limits`
- `/microsoft-*`

For production, set `TELEGRAM_ALLOWED_CHAT_IDS`.

## Environment variables

See `.env.example` for full documentation.

Main groups:
- OpenRouter: `OPENROUTER_API_KEY`, `MODEL`, `OPENROUTER_SHOW_COST_ESTIMATE`
- Telegram: `TELEGRAM_*`, `TELEGRAM_ALLOWED_CHAT_IDS`
- MCP: `MCP_STDIO_COMMAND`, `MCP_STDIO_ARGS`, `MCP_ALLOWED_ROOTS`, `MCP_*` limits
- Paths: `AUDIT_LOG_PATH`, `SESSION_CONTEXT_PATH`
- Microsoft: `MICROSOFT_*`
- Timezone: `DISPLAY_TIMEZONE` (IANA, e.g. `Europe/Warsaw`)

## Project layout

- `src/core/` - core runtime logic
- `src/main.py` - CLI entry point
- `src/telegram_bot.py` - Telegram entry point
- `deploy/` - deployment helper scripts

## License

See the parent stack repository policy and repository license files.
