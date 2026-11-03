# FoxReach plugin for Claude

Run your FoxReach cold email outreach from Claude. Ask how your campaigns are performing, read and answer replies, add leads, set up sequences and templates, check the health of your sending accounts, and pause or resume campaigns, all in plain language.

FoxReach is a cold email outreach platform for sales teams, founders and agencies. You need a FoxReach account to use this plugin: [www.foxreach.io](https://www.foxreach.io).

## What's in the plugin

- **FoxReach connector**: the remote MCP server at `https://api.foxreach.io/mcp`. The first time a FoxReach tool runs, Claude asks you to sign in with your FoxReach account (OAuth). The connector only works with the leads, campaigns, accounts and inbox in your own FoxReach workspace.
- **FoxReach outreach skill**: guidance for Claude on common jobs (campaign results, reply triage, adding leads, campaign setup, inbox health) and ground rules: confirm before any change, never send a reply you have not seen, and state the credit cost before spending paid credits.

## What it sends and where

The plugin runs no local code, hooks or scripts. Claude talks only to `https://api.foxreach.io/mcp`, operated by FoxReach. Requests carry the tool arguments Claude chooses (for example a campaign id or the text of a reply you approved) and your FoxReach sign-in token. Results come back from your FoxReach workspace. Tools that send email, start or resume a campaign, overwrite records or spend paid credits are marked destructive, so Claude asks before running them.

How FoxReach handles your data: [Privacy Policy](https://www.foxreach.io/privacy). Terms: [Terms of Service](https://www.foxreach.io/terms).

## Install

In Claude Code:

```
/plugin marketplace add foxreach/foxreach-claude-plugin
/plugin install foxreach@foxreach-marketplace
```

In Claude (claude.ai, Desktop, Cowork), install FoxReach from the plugin directory once it is listed, then sign in on the plugin's Connectors tab.

## Support

Email support@foxreach.io or use [www.foxreach.io/contact](https://www.foxreach.io/contact). Documentation: [www.foxreach.io/docs](https://www.foxreach.io/docs).

## License

MIT. See [LICENSE](LICENSE).
