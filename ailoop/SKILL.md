---
name: ailoop
description: Human-in-the-loop CLI for AI agent communication. Use when agents need to ask questions, request authorization, send notifications, run server mode, or forward messages; integrating ailoop SDK (Python/TypeScript) or configuring channels and providers.
license: Apache-2.0
---

# Ailoop

Ailoop is a CLI and server that lets AI agents talk to humans in a structured way: ask questions, request authorization, send notifications, and forward agent output. Use this skill when you need to add human-in-the-loop flows or integrate ailoop into an agent or app.

## When to use

- An agent must ask a human a question and wait for an answer.
- An agent needs approval before a critical action (e.g. deploy, delete).
- Sending notifications (build done, alerts) to humans or channels.
- Running a central server for multiple agents (server mode).
- Forwarding agent output to a server or channel.
- Integrating ailoop from Python or TypeScript (SDK).
- Configuring channels or providers (e.g. Telegram).

## Installation

```bash
# Release binaries (Linux/Windows from GitHub Releases)
# https://github.com/goailoop/ailoop/releases

# Homebrew (Linux)
brew install goailoop/cli/ailoop

# Scoop (Windows)
scoop bucket add goailoop https://github.com/goailoop/scoop
scoop install ailoop
```

Verify: `ailoop --version`

## Commands

### ask

Ask a question and wait for a human response.

```bash
ailoop ask "What is the best approach for this task?"
ailoop ask "Should we proceed?" --timeout 60
ailoop ask "Review this code" --channel dev-review --json
```

Options: `--timeout <seconds>`, `--channel <name>`, `--server <url>`, `--json`

### authorize

Request human approval for a critical action. If no response within timeout, authorization defaults to DENIED.

```bash
ailoop authorize "Deploy version 1.2.3 to production"
ailoop authorize "Delete user data" --timeout 300 --channel admin-ops
```

Options: `--timeout <seconds>`, `--channel <name>`, `--server <url>`, `--json`

### say

Send a notification.

```bash
ailoop say "Build completed successfully"
ailoop say "System alert: High CPU" --priority high --channel monitoring
```

Options: `--priority <low|normal|high|urgent>`, `--channel <name>`, `--server <url>`

### serve

Run ailoop in server mode (multi-agent).

```bash
ailoop serve
ailoop serve --port 9000 --host 0.0.0.0
```

Options: `--port <number>`, `--host <address>`, `--channel <name>`

### forward

Stream agent output to the server.

```bash
ailoop forward --channel my-agent
ailoop forward --input output.jsonl --channel my-agent --server http://localhost:8080
```

Options: `--channel <name>`, `--input <file>`, `--server <url>`, `--format <cursor|jsonl>`

### config

Interactive configuration: `ailoop config --init` (optionally `--config-file <path>`).

### provider

List providers and test Telegram: `ailoop provider list`, `ailoop provider telegram test` (use `--config-file <path>` if needed).

## SDK integration

### Python

```bash
pip install ailoop-py
```

```python
from ailoop import AiloopClient
client = AiloopClient(base_url='http://localhost:8081')
response = await client.ask('general', 'What is the answer?')
await client.say('general', 'Task completed', 'normal')
```

### TypeScript

```bash
npm install ailoop-js
```

```typescript
import { AiloopClient } from 'ailoop-js';
const client = new AiloopClient({ baseURL: 'http://localhost:8081' });
const response = await client.ask('general', 'What is the answer?');
await client.say('general', 'Task completed', 'normal');
```

## Channels

Channels isolate workflows. Names: 1-64 chars, lowercase alphanumeric, hyphens, underscores; must start with letter or digit. Default channel is `public`.

## Telegram provider

1. Create bot via [@BotFather](https://t.me/BotFather), copy token.
2. Get chat ID (e.g. [@userinfobot](https://t.me/userinfobot) or group ID).
3. Set token in environment: `export AILOOP_TELEGRAM_BOT_TOKEN=your_bot_token`
4. Config: `ailoop config --init` and enable Telegram with chat_id, or add `[providers.telegram]` with `enabled = true` and `chat_id` in config.
5. Test: `ailoop provider telegram test`
6. Run server: `ailoop serve`; questions/authorizations can be answered in Telegram or terminal.

## Getting help

- `ailoop --version`, `ailoop <command> --help`
- [GitHub](https://github.com/goailoop/ailoop)
