# Personal OS (pos)

A single-file personal AI assistant powered by Claude Code. No installation, no dependencies, no complex setup—just a bash script that turns Claude into your autonomous personal operating system.

**Think [OpenClaw](https://openclaw.ai) but simpler.** If OpenClaw is the full-featured Linux distro, pos is the single USB boot drive that just works.

The script itself is just a model + prompt + your context. There's no base code for integrations, skills, or interfaces—everything gets generated on demand, hyper-personalized to your exact situation. When you need a WhatsApp interface, Claude builds it. When you need a calendar skill, Claude writes it. Every piece of code is bespoke, disposable, and yours.

*See [Most Code is Just Cache](https://shrivu.substack.com/p/most-code-is-just-cache) for more on this idea.*

## What It Does

- **Autonomous execution** — Give it goals, it delivers outcomes. You review results, not steps.
- **Browser control** — Logs into websites, fills forms, extracts data via Chrome DevTools MCP.
- **Scheduled tasks** — Cron jobs that run Claude autonomously (daily briefings, monitoring, maintenance).
- **Persistent memory** — Learns about you, stores preferences, builds skills over time.
- **Any interface** — Terminal, WhatsApp, Telegram, Slack, iMessage, SMS—set up once, works forever.
- **Self-improving** — Creates its own skills and documentation as it solves problems.

## Quick Start

```bash
# One-liner
mkdir -p ~/personal-os && curl -o ~/personal-os/pos https://raw.githubusercontent.com/sshh12/personal-os/main/pos && chmod +x ~/personal-os/pos && ~/personal-os/pos
```

Or clone the repo:
```bash
git clone https://github.com/sshh12/personal-os ~/personal-os && ~/personal-os/pos
```

On first run, it onboards you interactively—asks about your tools, preferences, and sets up your chosen interfaces.

## Requirements

- macOS (Linux support possible with modifications)
- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed (`~/.local/bin/claude`)
- Claude API access (Max subscription or API key)

## How It Works

`pos` is a ~10KB bash script that wraps Claude Code with:
- A carefully crafted system prompt for autonomous operation
- Chrome DevTools MCP for browser automation
- Structured data storage in `~/personal-os-data/`
- Support for scheduled tasks via macOS launchd

All state lives in `~/personal-os-data/` as markdown files and scripts—human-readable, version-controlled, portable.

## vs OpenClaw

| | pos | OpenClaw |
|---|---|---|
| Setup | Single file, run immediately | npm install, configuration |
| Complexity | ~10KB bash script | Full Node.js application |
| Interfaces | Claude builds them for you | 50+ ready-made connectors |
| Community | Just you and Claude | Active Discord, shared skills |

**Choose pos if:** You're willing to spend time debugging and working with Claude to build a custom solution. Slower to set up initially, but you own every line and understand exactly what's running.

**Choose OpenClaw if:** You want ready-made integrations, an active community, and something that works out of the box.

## Example Use Cases

- "Check my email every hour and summarize anything important"
- "Monitor Zillow for houses matching my criteria and text me matches"
- "Track my subscriptions and cancel unused ones"
- "Apply to jobs matching my resume while I sleep"
- "Keep my notes organized and remind me of forgotten ideas"
- "Send follow-up emails to contacts I haven't talked to in 30 days"

## Architecture

```
~/personal-os-data/           # All your data (git repo)
├── CLAUDE.md                 # Central hub, preferences, tool mappings
├── user/                     # Learned context about you
├── .claude/skills/           # How-to guides pos creates
├── interfaces/               # Scripts for WhatsApp, Telegram, etc.
├── scripts/                  # Reusable automation
└── .env                      # Secrets (gitignored)

~/.claude/projects/           # Session logs for debugging
```

## Customization

Override the data directory:
```bash
POS_ROOT=/path/to/custom ./pos
```

Pass flags through to Claude:
```bash
./pos --verbose --output-format stream-json -p "check my calendar"
```

## Safety

**This is dangerous.** Let's be absolutely clear:

- Claude has **full, unrestricted access** to your system—files, shell, browser, everything
- It runs with `--dangerously-skip-permissions` which means no confirmation prompts
- It can read, write, delete, execute, and send data anywhere
- A misunderstood prompt could delete your files, send emails as you, or worse

**Use at your own risk.** This is a power tool for people who understand the tradeoffs. Mitigations:

- Runs locally—data stays on your machine unless you set up external interfaces
- Default allow-listing for interfaces (only your phone number, your Slack user, etc.)
- All actions logged in `~/.claude/projects/` for audit
- You can review the ~10KB script yourself—there's no hidden complexity

## Contributing

This is a single-file project by design. PRs that keep it simple are welcome.

## License

MIT

---

*Inspired by the incredible work on [OpenClaw](https://openclaw.ai) by Peter Steinberger and community. If you want the full-featured experience with 50+ integrations and an active Discord, check them out.*
