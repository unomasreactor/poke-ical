# poke-ical

An iCloud CalDAV MCP bridge for Poke, deployed as a Cloudflare Worker.

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/mollyvita/poke-ical)

## Deployment guide

### 1. Deploy the Worker

Click the button above, or deploy manually:

```bash
npm install
npx wrangler deploy
```

### 2. Set up secrets

After deploying, add your iCloud credentials as Worker secrets. You can do this in two ways:

**Via the Cloudflare dashboard:**
1. Open your Worker in the [Cloudflare dashboard](https://dash.cloudflare.com).
2. Go to **Settings** → **Variables** → **Secret variables**.
3. Add the following secrets:

| Secret | Value |
|---|---|
| `CALDAV_USERNAME` | Your Apple ID email (e.g. `you@icloud.com`) |
| `CALDAV_PASSWORD` | An app-specific password from [appleid.apple.com](https://appleid.apple.com) |

**Via Wrangler CLI:**

```bash
npx wrangler secret put CALDAV_USERNAME
npx wrangler secret put CALDAV_PASSWORD
```

> To generate an app-specific password, sign in at [appleid.apple.com](https://appleid.apple.com), go to **Sign-In and Security** → **App-Specific Passwords**, and generate a new password for this Worker.

### 3. Connect to Poke

Once deployed, add your Worker URL as an MCP integration in Poke:

```
https://<your-worker>.workers.dev/mcp
```

---

## MCP endpoint

All MCP communication happens at:

```
/mcp
```

Accepts both `GET` (SSE stream) and `POST` (JSON-RPC 2.0) requests.

## Available tools

- `list_calendars` — list all iCloud calendars on the account
- `list_events` — list events in a calendar, with optional date range filter
- `get_event` — fetch raw iCalendar data for a specific event
- `create_event` — create a new event
- `update_event` — update fields on an existing event
- `delete_event` — delete an event
- `search_events` — search events by keyword
- `get_freebusy` — return busy intervals within a date range
- `get_ical_feed` — return all events as a complete `.ics` feed
