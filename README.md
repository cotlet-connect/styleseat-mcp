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

## What you can ask your AI to do in StyleSeat

These are real jobs StyleSeat users asked about. Cotlet lets ChatGPT, Claude or any MCP client do them in your own signed-in StyleSeat account.

### Fees, payouts and refunds

#### Can AI check which of my StyleSeat clients were charged the 30% new-client fee and which were already my own clients?

Yes. StyleSeat's New Client Connection takes 30% of a first service, capped at $50, on the premium plan. With Cotlet, your agent lists the month's clients charged the fee in your signed-in StyleSeat and marks those already in your client list, so you know which charges to question or claim.

Ask your agent: "List this month's StyleSeat clients charged the new-client fee and mark the ones who were already in my client list before they booked."

As an API endpoint: Yes. Save the fee check as an endpoint in Cotlet and call it every month from n8n, Make or Zapier to get the list by email.

#### How do I find which StyleSeat payouts are on hold and why, without checking the app every day?

Yes. StyleSeat's help centre says a payout can pause after a StyleSeat or Stripe check. With Cotlet, your agent reads your payouts in your signed-in StyleSeat and lists the ones on hold with amount, date and the reason shown. Cotlet does not release funds.

Ask your agent: "List my StyleSeat payouts that are on hold or late, with amount, date and the reason StyleSeat shows."

As an API endpoint: Yes. Save the payout check as an endpoint in Cotlet and call it every morning from n8n, Make or Zapier to post held payouts to Slack.

#### Can an AI agent answer StyleSeat support for me when 20 messages get the same automated reply about my held payout?

Partly. StyleSeat support works through chat and email. With Cotlet, your agent collects the held payout details from your signed-in StyleSeat and drafts one clear message with dates and amounts, so you stop repeating yourself. Nothing is sent until you say yes.

Ask your agent: "Gather the details of my held StyleSeat payout and draft one clear message to StyleSeat support with dates and amounts."

#### Can an AI agent issue a refund or fix a chargeback dispute in my StyleSeat account for me?

Not on its own. You issue refunds from the appointment, and some charges such as no-show fees and deposits refund only in full. With Cotlet, your agent can list disputed charges and gather the appointment, payment and message details for your chargeback answer. Refunds stay in your hands.

Ask your agent: "List my StyleSeat chargebacks and collect the appointment, payment and client messages for each so I can write my response."

#### How do I automate reminders and no-show or late-cancel fees in StyleSeat, and what do clients get charged?

Partly. StyleSeat sends client reminders and charges no-show and late-cancel fees on its own, by the policy you set, and deposits can be a flat fee or a percentage. With Cotlet, your agent lists this month's fees charged and drafts a note to each client. Nothing is sent until you say yes.

Ask your agent: "List the no-show and late-cancel fees StyleSeat charged this month with client, date and amount, and draft a short note for each client."

As an API endpoint: Yes. Save the fee list as an endpoint in Cotlet and call it every month from n8n, Make or Zapier.

### Income and accounting

#### Can AI build my monthly income and tips report from StyleSeat when the built-in reports only show revenue, clients and bookings?

Yes. StyleSeat has no custom reports. With Cotlet, your agent reads the month's appointments, tips and fees in your signed-in StyleSeat and builds one income table per service and per week, as a CSV or a Google Sheet.

Ask your agent: "Build a table of this month's StyleSeat income with tips, fees and total per week and per service."

As an API endpoint: Yes. Save the income table as an endpoint in Cotlet and call it on the first of each month from n8n, Make or a script.

#### How do I get my StyleSeat income and 1099-K numbers ready for my accountant without retyping them?

Partly. According to its blog, StyleSeat files Form 1099-K as the payment processor, and no accountant export was found. With Cotlet, your agent reads the year's payouts, fees and refunds in your signed-in StyleSeat and totals them in one table. Check the totals against your 1099-K.

Ask your agent: "Total this year's StyleSeat payouts, fees, tips and refunds by month in one table for my accountant."

As an API endpoint: Yes. Save the yearly totals as an endpoint in Cotlet and call it from n8n, Make or a script each quarter.

#### How do I connect StyleSeat to QuickBooks so my payouts and fees show up in my books automatically?

Yes. StyleSeat's help centre shows no QuickBooks link. With Cotlet, your agent reads your payouts and fees in your signed-in StyleSeat and enters them in QuickBooks, signed in in the same Cloud Browser. Nothing is entered until you say yes.

Ask your agent: "Take last week's StyleSeat payouts and fees and enter them in QuickBooks. Show me the entries before you save."

As an API endpoint: Yes. Save the weekly payout read as an endpoint in Cotlet and call it every Monday from n8n, Make or Zapier.

#### How do I connect StyleSeat to Google Sheets so my appointments and clients land in a spreadsheet every day?

Yes. StyleSeat has no Zapier app and no API for professionals. With Cotlet, your agent reads the day's appointments and new clients in your signed-in StyleSeat and adds them as rows to your Google Sheet. Nothing is added until you say yes.

Ask your agent: "Add today's StyleSeat appointments and any new clients to my Google Sheet, one row each with client, service, time and price."

As an API endpoint: Yes. Save the daily read as an endpoint in Cotlet and call it every evening from n8n, Make or Zapier to fill the sheet.

### Clients and campaigns

#### Can ChatGPT or Claude look at my StyleSeat appointments and tell me which clients are overdue for a rebook?

Yes. StyleSeat has a list of clients who are due back. With Cotlet, ChatGPT or Claude reads your calendar and client history in your signed-in StyleSeat, lists who is overdue for the service you pick and drafts a rebooking note for each. Nothing is sent until you say yes.

Ask your agent: "List my StyleSeat clients who have not booked in 8 weeks, with their last service, and draft a rebooking note for each."

As an API endpoint: Yes. Save the overdue list as an endpoint in Cotlet and call it every Monday from n8n, Make or Zapier.

#### Can AI write and send my StyleSeat text and email campaigns to clients who have not booked in 8 weeks?

Partly. StyleSeat sends campaigns from its Marketing menu, and they need connected payments. With Cotlet, your agent lists clients with no booking in 8 weeks and drafts the text and the email for each. Cotlet does not send texts itself. Nothing is sent until you say yes.

Ask your agent: "List StyleSeat clients with no booking in 8 weeks and draft one short text and one email for each. Send nothing."

As an API endpoint: Yes. Save the client list as an endpoint in Cotlet and call it every month from n8n, Make or Zapier.

#### Can I connect StyleSeat to Mailchimp or another email tool to send my client list a newsletter?

Yes. StyleSeat's help centre shows no Mailchimp link, and it sends its own campaigns from the Marketing menu. With Cotlet, your agent reads your client list in your signed-in StyleSeat and adds clients with an email to Mailchimp, signed in in the same Cloud Browser. Nothing is added until you say yes.

Ask your agent: "Add every StyleSeat client with an email address to my Mailchimp audience with first name and last name."

As an API endpoint: Yes. Save the client read as an endpoint in Cotlet and call it weekly from n8n or Make to keep your audience up to date.

### Moving your data

#### How do I export my full client list from StyleSeat to Excel or CSV before I leave?

Yes. StyleSeat's help centre has no export article, and third-party guides describe Export Client List in the Clients tab, which emails you a file. With Cotlet, your agent also reads each client's page in your signed-in StyleSeat and saves name, phone, email, notes and last visit to one CSV.

Ask your agent: "Save every StyleSeat client with phone, email, notes and last visit date to one CSV file I can open in Excel."

#### Can I export my appointment history, payouts and reviews from StyleSeat, or only the client list?

Partly. Third-party reports say StyleSeat exports the client list only, not appointments, payouts or reviews. With Cotlet, your agent reads appointment history, payouts and reviews from the screens your login opens and saves each as a CSV. Check the files before you cancel.

Ask your agent: "Save my StyleSeat appointment history, payouts and reviews as three separate CSV files."

#### How do I import my clients from GlossGenius, Booksy, Vagaro or Square into StyleSeat from a CSV?

Yes. StyleSeat's Import Clients takes a .csv or .xlsx file of up to 5,000 clients and 20 MB, and skips duplicates. With Cotlet, your agent reads your old app's client list, shapes it into the columns StyleSeat asks for and runs the import. Nothing is imported until you say yes.

Ask your agent: "Read my client list from GlossGenius and prepare a StyleSeat import file with name, phone, email, birthday and notes. Show me the first five rows."

#### How do I move my StyleSeat client notes and color formulas to another booking app?

Partly. StyleSeat keeps notes, photos and history on each client profile, and its help centre shows no notes export. With Cotlet, your agent reads each client's notes and formulas in your signed-in StyleSeat and saves them to a file or adds them to your new app. Nothing is added until you say yes.

Ask your agent: "Read the notes and color formulas on every StyleSeat client profile and save them to one CSV with the client name."

### Calendar and booking links

#### How do I sync StyleSeat with Google Calendar and Apple Calendar so personal events block my booking times?

Partly. StyleSeat syncs with Google Calendar from Calendar settings, and its help centre shows no Apple Calendar sync. With Cotlet, your agent reads your personal events in Google Calendar and blocks the matching times in your StyleSeat calendar. Nothing is blocked until you say yes.

Ask your agent: "Read my personal events in Google Calendar for next week and block those times in my StyleSeat calendar. Show me the list first."

As an API endpoint: Yes. Save the event read as an endpoint in Cotlet and call it every morning from n8n or Make to keep StyleSeat blocked.

#### How do I keep my StyleSeat booking link and Instagram book button pointed at the right service?

Yes. StyleSeat makes booking links under Marketing, Tools, Booking links, and Instagram Action Buttons. With Cotlet, your agent opens your signed-in StyleSeat and lists every booking link with the service it opens, so you see which ones need changing. Nothing changes until you say yes.

Ask your agent: "List every StyleSeat booking link and the service it opens, and tell me which Instagram button points at which service."

#### How do I connect StyleSeat to my phone line so an AI assistant answers calls and books into my calendar?

Not on its own. StyleSeat's help centre shows no phone assistant, and Cotlet does not answer calls. With Cotlet, your agent can check your StyleSeat calendar for free times and book a client you name. Nothing is booked until you say yes.

Ask your agent: "Check my StyleSeat calendar for the next free slot for a silk press on Friday and book Jo P. there."

As an API endpoint: Yes. Save the free-times check as an endpoint in Cotlet and call it from n8n or Make so a phone assistant can read your open times.

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
