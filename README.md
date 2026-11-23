# StyleSeat API integration and MCP server: connect StyleSeat to ChatGPT, Claude and any AI agent

StyleSeat API integration and MCP server, unofficial, by Cotlet. Your AI works in your own signed-in StyleSeat account: appointments, clients, payouts, fees, reviews and marketing, on the same screens your login opens. No developer and no StyleSeat API key needed. Cotlet is independent and is not affiliated with StyleSeat.

**Does StyleSeat have an API or an MCP server?** StyleSeat has no public API for professionals and no Zapier app (checked 23 Nov 2026). Its official MCP server is for clients only: it finds pros and open times and looks up appointments, and it cannot book or cancel. StyleSeat's client export is a CSV of the client list, and its import takes CSV or XLSX files of up to 5,000 clients. StyleSeat has no MCP server or ChatGPT or Claude app for the pro's own account.

## What beauty pros ask their AI to do in StyleSeat

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Clients due for a rebook | "List clients who have not booked in 8 weeks, with their last service, and draft a text to each. Send nothing yet." | Weekly |
| Who paid the new-client fee | "List this month's clients charged the new-client fee and mark the ones who were already my clients." | Monthly |
| Payouts on hold | "Show which payouts are on hold or pending and since when." | Daily |
| Income and tips for your accountant | "Build my monthly income, tips and fees report from StyleSeat in a spreadsheet for my accountant." | Monthly |
| 1099-K numbers | "Total this year's payouts, fees and refunds so my accountant can match them to my 1099-K." | Yearly |
| Support message about a held payout | "Write the message to StyleSeat support about my held payout, with the dates and amounts from my account." | When a payout is held |
| No-shows and late cancels | "List this month's no-shows and late cancellations with the fee that applied." | Monthly |
| Appointments in a spreadsheet | "Put each day's appointments with client, service and price in my Google Sheet." | Your own endpoint, called from Google Sheets |
| Import from another app | "Import my clients from this GlossGenius export into StyleSeat. Show me before you save." | Once |
| Leaving StyleSeat | "Export my clients with notes, my appointment history and my payouts before I move to another app." | Once |

Your AI asks you before it sends, books or changes anything. Payments stay in your hands: Cotlet refuses payments.

## Your own StyleSeat API, built with Cotlet

StyleSeat gives pros no API and no Zapier app. With Cotlet you build your own: your AI does the job once in your Cloud Browser, you save it as an endpoint, and you call it from Google Sheets, QuickBooks, Mailchimp, Zapier, Make, n8n, your own code.

Examples you can build in a few minutes (none of them exists until you build it):

| Endpoint you build | What it returns or does |
|---|---|
| `appointments-for-day` | appointments for a date with client, service and price |
| `lapsed-clients` | clients with no booking for the weeks you name, with their last service |
| `payouts` | payouts with status, amount and date, including holds |
| `fees-for-period` | new-client fees, booking fees and refunds for a period, per client |
| `income-for-period` | income, tips and fees for a period |
| `add-client` | adds a client with phone, email and a note (writes only after you allow it) |

Call it with your Cotlet API key, JSON in and JSON out:

```bash
curl -X POST https://cotlet.dev/api/v1/custom/appointments-for-day \
  -H "Authorization: Bearer $COTLET_API_KEY" -H "Content-Type: application/json" -d '{}'
```

Limits: one call runs up to 90 seconds, writes wait for your consent, payments are blocked. Start here: https://cotlet.dev/app/custom-api/new?app=styleseat

## Connect in 2 minutes

Sign up at https://cotlet.dev, sign in to StyleSeat once in your Cloud Browser, then add Cotlet to your AI.

### Claude

Settings → Connectors → Add custom connector, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev.

### ChatGPT

Open Plugins in ChatGPT, choose Create as a plugin, paste `https://cotlet.dev/mcp/v1` and sign in with Cotlet.dev. No API key needed.

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

## Questions from StyleSeat pros
**Does StyleSeat have an API or a Zapier app for pros?**

No. StyleSeat has no public API for professionals and no Zapier app (checked 23 Nov 2026). With Cotlet you build your own endpoints in your signed-in StyleSeat account and call them from Google Sheets, Zapier, Make or n8n.

**Is StyleSeat's ChatGPT and Claude connector for pros?**

No. StyleSeat's official MCP server is for clients: it finds pros and open times and cannot book or cancel. Cotlet lets your own ChatGPT or Claude work in your pro account.

**Can AI tell me which StyleSeat clients were charged the new-client fee?**

Yes. Your AI lists the month's clients charged the new-client fee and marks the ones who were already your clients, so you know which charges to question.

**Can AI tell me which StyleSeat clients are overdue for a rebook?**

Yes. It lists clients with no booking for the weeks you choose and drafts a text for each. Nothing goes out until you say yes.

**How do I get my StyleSeat income ready for my accountant?**

Your AI builds the month's income, tips and fees in a spreadsheet, and totals the year's payouts, fees and refunds for your 1099-K.

**How do I export my StyleSeat clients and history before I leave?**

StyleSeat's own export is the client list. Your AI also reads notes, appointment history and payouts from the screens your login opens and saves them with it.

## Find it on

- [StyleSeat MCP server on Cotlet.dev](https://cotlet.dev/integrations/styleseat-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=styleseat&utm_content=find)
- [StyleSeat API with Cotlet](https://cotlet.dev/integrations/api/styleseat?utm_source=github&utm_medium=readme&utm_campaign=styleseat&utm_content=find)
- [Does StyleSeat have an API or MCP server?](https://cotlet.dev/api-mcp-status/styleseat?utm_source=github&utm_medium=readme&utm_campaign=styleseat&utm_content=find)

## More beauty apps on Cotlet

- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [Squire MCP](https://github.com/cotlet-connect/squire-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
