# FoxReach Claude Code Plugin

Official [Claude Code](https://claude.com/claude-code) plugin for managing [FoxReach](https://foxreach.io) cold email outreach directly from your terminal.

## What it does

This plugin gives Claude Code the ability to manage your FoxReach account:

- **Leads** — list, create, update, delete, bulk import
- **Campaigns** — create, configure, start, pause, add leads/accounts
- **Sequences** — add multi-step email sequences to campaigns
- **Templates** — manage reusable email templates
- **Email Accounts** — view accounts and health scores
- **Inbox** — read and categorize reply threads
- **Analytics** — dashboard overview and campaign performance

Claude uses the [FoxReach Python SDK](https://github.com/foxreach/foxreach-python-sdk) under the hood, executing operations through inline Python scripts.

## Installation

### From the marketplace

```
/plugin marketplace add foxreach/foxreach-claude-plugin
/plugin install foxreach@foxreach-marketplace
```

### Local install (for development)

```bash
git clone https://github.com/foxreach/foxreach-claude-plugin.git
claude --plugin-dir ./foxreach-claude-plugin
```

## Prerequisites

Install the FoxReach Python SDK before using:

```bash
pip install foxreach
```

Get your API key from the [FoxReach dashboard](https://foxreach.io) under Settings > API Keys.

## Usage

### Auto-invoked

Just ask Claude anything about FoxReach, and it will use the plugin automatically:

```
List my active campaigns
Create a lead for jane@example.com at Acme Corp
Show me campaign analytics for cmp_xyz789
Check my inbox for unread replies
```

### Manual invoke

```
/foxreach:foxreach list leads --search "acme"
/foxreach:foxreach create campaign "Q1 Outreach"
/foxreach:foxreach analytics overview
```

### JSON output

Claude formats results as readable tables by default. Ask for JSON when you need raw data:

```
List my leads as JSON
```

## Skill contents

| File | Description |
|------|-------------|
| `skills/foxreach/SKILL.md` | Main skill — quick reference, workflows, pagination, error handling |
| `skills/foxreach/api-reference.md` | Complete data types, all endpoints, field definitions |
| `skills/foxreach/examples.md` | Working code examples for every operation |

## Related

- [FoxReach Python SDK](https://github.com/foxreach/foxreach-python-sdk) — `pip install foxreach`
- [FoxReach TypeScript SDK](https://github.com/foxreach/foxreach-typescript-sdk) — `npm install foxreach`
- [FoxReach CLI](https://github.com/foxreach/foxreach-cli) — `pip install foxreach-cli`

## License

MIT
