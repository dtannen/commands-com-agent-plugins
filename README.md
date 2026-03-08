# Commands.com Agent Plugins

Standalone provider plugin repository for Commands.com Desktop/Agent.

This repo currently ships:
- `openai` provider plugin
- `gemini` provider plugin

## Getting started

See [`GETTING_STARTED.md`](./GETTING_STARTED.md) for a quick setup guide including:
- Commands Desktop setup
- plugin install
- first-run settings
- credentials setup

## Why this repo exists

The packaged desktop app does not yet include a built-in plugin installer UI.
This repo is the distribution source for provider plugins and install instructions.

## Quick install (packaged desktop app)

1. Clone this repo.
2. Run the installer script:

**macOS / Linux:**

```bash
./scripts/install-plugins.sh
```

**Windows (or any platform with Node.js):**

```bash
node scripts/install-plugins.mjs
```

Both scripts:
- copy plugins to the platform-appropriate providers directory
- install each plugin's npm dependencies

| Platform | Default providers directory |
|----------|---------------------------|
| macOS / Linux | `~/.commands-agent/providers` |
| Windows | `%LOCALAPPDATA%\commands-agent\providers` |

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

**macOS / Linux:**

```bash
mkdir -p ~/.commands-agent/providers

# Copy plugins
rsync -a --delete ./plugins/openai/ ~/.commands-agent/providers/openai/
rsync -a --delete ./plugins/gemini/ ~/.commands-agent/providers/gemini/

# Install deps in destination
npm install --prefix ~/.commands-agent/providers/openai --omit=dev
npm install --prefix ~/.commands-agent/providers/gemini --omit=dev
```

**Windows (PowerShell):**

```powershell
$dest = "$env:LOCALAPPDATA\commands-agent\providers"
New-Item -ItemType Directory -Force -Path "$dest\openai", "$dest\gemini"

# Copy plugins (exclude node_modules)
robocopy .\plugins\openai "$dest\openai" /MIR /XD node_modules
robocopy .\plugins\gemini "$dest\gemini" /MIR /XD node_modules

# Install deps in destination
npm install --prefix "$dest\openai" --omit=dev
npm install --prefix "$dest\gemini" --omit=dev
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
./scripts/install-plugins.sh        # macOS/Linux
node scripts/install-plugins.mjs    # Windows (or any platform)
```

## Uninstall

**macOS / Linux:**

```bash
rm -rf ~/.commands-agent/providers/openai ~/.commands-agent/providers/gemini
```

**Windows (PowerShell):**

```powershell
Remove-Item -Recurse -Force "$env:LOCALAPPDATA\commands-agent\providers\openai", "$env:LOCALAPPDATA\commands-agent\providers\gemini"
```
