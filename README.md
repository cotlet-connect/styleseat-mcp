# StyleSeat MCP server

Connect StyleSeat to ChatGPT, Claude and any AI agent through Cotlet.dev. Work in your own professional account with 18 hosted tools for appointments, refunds, reviews and client campaigns. Write actions ask for approval by default.

## What you can ask

> Read my calendar for today.

> Show clients who are overdue for their next visit.

> Draft a text campaign for clients who are due back and show it before sending.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its StyleSeat tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 styleseat_agent.py serve
```

Or with Docker:

```bash
docker build -t styleseat-mcp .
docker run -i --rm -e COTLET_API_KEY styleseat-mcp
```

Tools: `styleseat_guide`, `styleseat_status`, `styleseat_open`, `styleseat_read`, `styleseat_click`, `styleseat_type`, `styleseat_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 styleseat_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 styleseat_agent.py status    # is StyleSeat ready in your Cotlet.dev account
python3 styleseat_agent.py guide     # StyleSeat screens and sign-in link
python3 styleseat_agent.py read      # open StyleSeat, read the page, close the browser
python3 styleseat_agent.py tools     # list Cotlet.dev tools
python3 styleseat_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from StyleSeat users

**Which StyleSeat account should I connect?**

Connect your own professional account at www.styleseat.com/m/login in the Cotlet Cloud Browser.

**Which actions ask for approval?**

All 13 write tools ask for approval by default. The five read tools run immediately.

**Is Cotlet affiliated with StyleSeat?**

No. Cotlet.dev is independent and not affiliated with StyleSeat.

## Learn more

- [StyleSeat integration and hosted tools](https://cotlet.dev/integrations/styleseat-api-mcp-server)

---

More: https://cotlet.dev/integrations/styleseat-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=styleseat

Request access: https://cotlet.dev/signup?app=styleseat&utm_source=github&utm_medium=readme&utm_campaign=styleseat

## Workflows on the hosted integration

The live integration page documents these 18 operations:

- `view_calendar`
- `set_availability`
- `manage_appointments`
- `checkout_appointment`
- `refund_appointment`
- `respond_to_chargeback`
- `view_payouts`
- `set_deposits`
- `configure_smart_pricing`
- `manage_service_packages`
- `edit_business_info`
- `manage_reviews`
- `get_booking_links`
- `list_clients_due_back`
- `send_campaign`
- `import_clients`
- `delete_clients`
- `view_client_photos`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
