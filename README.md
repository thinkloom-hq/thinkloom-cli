# thinkloom CLI

`thinkloom` is the command line for ThinkLoom. Manage knowledge bases and
documents, search content and ask questions from your terminal, and connect AI
tools through MCP.

This repository only hosts the release binaries. The exact connect command for
your server is in ThinkLoom under **Settings → Publish & Integrations → CLI**.

## Install

macOS and Linux with [Homebrew](https://brew.sh):

```bash
brew install thinkloom-hq/tap/thinkloom
```

Without Homebrew (macOS and Linux). The script checks the download against the
release checksums and installs to `/usr/local/bin`, or `~/.local/bin` when that
isn't writable:

```bash
curl -fsSL https://github.com/thinkloom-hq/thinkloom-cli/releases/latest/download/install.sh | sh
```

Windows: download `thinkloom_windows_amd64.zip` (or `_arm64`) from the
[latest release](https://github.com/thinkloom-hq/thinkloom-cli/releases/latest),
unzip it and put `thinkloom.exe` in a folder on your `PATH`.

Upgrade with `brew upgrade thinkloom`, or run the install script again.

## Connect to your server

Create a profile for your ThinkLoom server, then sign in with your email and
password:

```bash
thinkloom profile add thinkloom --host 'https://your-thinkloom-server' --use &&
thinkloom auth login
```

Use the server address you open ThinkLoom at, without `/api/v1`. Check the
connection and list the knowledge bases you can access:

```bash
thinkloom doctor
thinkloom kb list
```

For scripts and CI, skip `auth login` and set `THINKLOOM_HOST` plus
`THINKLOOM_API_KEY` instead; nothing is written to disk.

## Common commands

```bash
thinkloom doc upload ./document.pdf --kb "KB_ID"
thinkloom search chunks "query" --kb "KB_ID"
thinkloom chat "question" --kb "KB_ID" --format text
thinkloom agent list
```

Every command prints a JSON envelope by default; add `--format text` for
human-readable output. Run `thinkloom --help` or `thinkloom <command> --help`
for the full reference.

## Connect an MCP client

After signing in, add this to an MCP client that supports stdio (Claude
Desktop, Cursor, Claude Code and others):

```json
{
  "mcpServers": {
    "thinkloom": {
      "command": "thinkloom",
      "args": ["--profile", "thinkloom", "mcp", "serve"]
    }
  }
}
```

If the client can't find `thinkloom`, set `command` to the binary's full path
(`which thinkloom`).

## Agent Skills

The binary bundles Agent Skills that teach coding agents to use it:

```bash
thinkloom skills install    # writes to ~/.claude/skills
```

## Shell completion

```bash
thinkloom completion zsh --help   # also bash, fish, powershell
```
