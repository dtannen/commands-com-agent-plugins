# Commands.com Agent Plugins

Standalone provider plugin repository for Commands.com Desktop/Agent.

This repo currently ships:
- `openai` provider plugin
- `gemini` provider plugin

## Why this repo exists

The packaged desktop app does not yet include a built-in plugin installer UI.
This repo is the distribution source for provider plugins and install instructions.

## Quick install (packaged desktop app)

1. Clone this repo.
2. Run the installer script:

```bash
./scripts/install-plugins.sh
```

This script:
- installs each plugin's npm dependencies
- copies plugins to `~/.commands-agent/providers`

3. Open Commands Desktop.
4. Go to **Settings > Developer**.
5. Enable:
- `Dev Mode`
- `Trust All Plugins`
6. Restart the app.
7. Create or edit an agent profile and select provider `openai` or `gemini`.

## Credentials

### OpenAI plugin

Use either:
- Desktop profile `apiKey` field (stored as provider config), or
- Codex OAuth token at `~/.codex/auth.json` (run `codex` and sign in)

### Gemini plugin

Use either:
- Desktop profile `apiKey` field, or
- Gemini OAuth creds at `~/.gemini/oauth_creds.json` (run `gemini` and sign in)

## Manual install (without script)

```bash
mkdir -p ~/.commands-agent/providers

# Copy plugins
rsync -a --delete ./plugins/openai/ ~/.commands-agent/providers/openai/
rsync -a --delete ./plugins/gemini/ ~/.commands-agent/providers/gemini/

# Install deps in destination
npm install --prefix ~/.commands-agent/providers/openai --omit=dev
npm install --prefix ~/.commands-agent/providers/gemini --omit=dev
```

## CLI/runtime usage (non-desktop)

External providers are only loaded when plugin verification is explicitly enabled.

```bash
COMMANDS_AGENT_DEV=1 \
COMMANDS_AGENT_TRUST_ALL_PLUGINS=1 \
node dist/index.js start
```

Optional custom plugin path:

```bash
COMMANDS_AGENT_PROVIDERS_DIR=/custom/providers/path
```

## Updating plugins

```bash
git pull
./scripts/install-plugins.sh
```

## Uninstall

```bash
rm -rf ~/.commands-agent/providers/openai ~/.commands-agent/providers/gemini
```
