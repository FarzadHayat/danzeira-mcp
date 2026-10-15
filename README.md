# Danzeira MCP

<img src="assets/logo.png" alt="Danzeira" width="96">

[Danzeira](https://danzeira.com) is event ticketing for partner-dance organisers: socials, workshops, festivals and classes. This plugin connects your AI assistant to the Danzeira remote MCP server so you can run your events from chat.

## Endpoint

```
https://danzeira.com/mcp
```

Authentication is OAuth 2.1 with dynamic client registration. There are no API keys to copy. Your client registers itself, opens a browser, and you sign in with your Danzeira account. Access is limited to the events you organise.

## Tools

- **Events:** `events_list`, `events_show`, `events_create`, `events_update`, `events_delete`, `events_duplicate`, `events_publish`, `events_unpublish`, `events_attendees`, `events_cancel`
- **Series:** `events_publish_series`, `events_cancel_series`, `events_delete_series`, `events_series_slug`
- **Dates:** `dates_show`, `dates_add`, `dates_update`
- **Ticket types:** `ticket_types_create`, `ticket_types_update`, `ticket_types_delete`, `ticket_types_duplicate`
- **Tickets and orders:** `tickets_issue`, `tickets_refund`, `orders_list`
- **Promo codes:** `promo_codes_list`, `promo_codes_create`, `promo_codes_update`, `promo_codes_delete`
- **Check-ins:** `check_ins_list`, `check_ins_check_in`, `check_ins_scan`, `check_ins_undo`
- **Announcements:** `announcements_list`, `announcements_create`, `announcements_stop`
- **Sessions (event schedule):** `sessions_list`, `sessions_create`, `sessions_update`, `sessions_delete`, `sessions_visibility`
- **Team:** `team_list`, `team_invite`, `team_resend`, `team_revoke`
- **Waitlist:** `waitlist_list`, `waitlist_add`, `waitlist_remove`
- **Bans:** `bans_list`, `bans_create`, `bans_lift`

Refunds and cancellations cannot be undone, so your assistant should confirm before running them.

## Connect in Cursor

Install the Danzeira plugin from the Cursor marketplace, or add this to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "danzeira": {
      "url": "https://danzeira.com/mcp"
    }
  }
}
```

Cursor will show a sign-in prompt. Click it, sign in with Danzeira, and approve access.

## Connect in Grok Bot

Add a remote MCP server with the URL `https://danzeira.com/mcp`, then sign in with Danzeira when prompted and approve access.

## Privacy and terms

- Privacy policy: https://danzeira.com/privacy
- Terms of service: https://danzeira.com/terms

## Support

Email hello@danzeira.com.

## License

MIT
