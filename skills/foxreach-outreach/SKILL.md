---
name: foxreach-outreach
description: Run day to day cold email work in the user's own FoxReach workspace through the FoxReach connector tools. Use when the user asks about their FoxReach campaigns, replies, leads, sequences, templates, sending accounts or results.
---

# FoxReach outreach

The FoxReach connector acts on the user's own FoxReach workspace: their leads, campaigns, sequences, templates, sending accounts and inbox. Everything goes through the same limits and sending safeguards as the FoxReach app. The user signs in with their FoxReach account the first time a FoxReach tool runs.

The user's explicit instructions take priority over the suggested workflows in this skill. The ground rules below still apply in every case.

## Ground rules

- Work only with data in the user's workspace. Do not scrape websites or social networks for contacts, and do not invent email addresses.
- Before any change (send a reply, create or update a lead, start, pause or resume a campaign, assign leads or accounts, import, delete), say exactly what will change and wait for the user to confirm.
- Never send a reply the user has not seen. Draft the text in the conversation first, then call `send_inbox_reply` once they approve it.
- Before any tool that spends credits, tell the user how many credits it will spend and wait for a yes: `reveal_leads` and the `reveal` action of `manage_lead_finder_list` (lead finder credits), `verify_email`, `verify_emails_bulk` and the `verify` action of `bulk_update_leads` (verification credits), and `rewrite_copy` (AI credits). Spent credits are not refunded.
- Refuse copy that impersonates another company or person, hides who the sender is, or tricks the recipient (fake invoices, password resets, false urgency).
- Billing and plan changes are not available through the connector. Tell the user to manage their plan inside FoxReach.

## Common jobs

**"How is my outreach doing?"** Call `get_workspace_overview`. For a single campaign, find it with `list_campaigns` and call `get_campaign_stats`. Report sends, opens, replies and bounces, and point out anything unusual (high bounce rate, no replies after many sends).

**"What came in?"** Call `list_inbox_threads`, then `get_inbox_conversation` for the threads worth reading. Group replies into interested, not now, unsubscribe and out of office. Offer to draft answers for the interested ones.

**"Add these leads."** Use `create_lead` for each contact the user gives you (email is required). To put them into a campaign, find the campaign with `list_campaigns` and call `assign_leads_to_campaign`.

**"Import this CSV."** Read the header row of the attached file, propose which columns map to email, first name, last name, company, title, phone, LinkedIn and website, and confirm the mapping with the user. Then call `import_csv_leads` with the mapping and the file contents as `csv_text` (up to 2 MB; split bigger files). Other columns become custom fields named after their header. Report how many leads were imported and how many were skipped as duplicates.

**"Set up a campaign."** Create it with `create_campaign`, add steps with `create_sequence`, attach sending accounts with `assign_accounts_to_campaign`, add leads, then summarize the setup and ask before calling `start_campaign`.

**"Use my other workspace."** Tools run in the user's personal workspace by default. Call `list_workspaces`, confirm which one they mean, and pass its id as `workspace_id` on the tool calls for that workspace.

**"Are my inboxes OK?"** Call `list_email_accounts`, then `get_email_account_health` on any account that looks off. Explain problems in plain words (authentication records, bounce rate, warmup progress).

## Writing outreach copy

Keep emails short, specific to the recipient and honest about who is writing. One clear ask per email. Include an easy way to opt out when the user's sequence does not already have one.
