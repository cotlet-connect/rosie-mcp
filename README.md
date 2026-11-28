# Rosie API integration and MCP server: connect Rosie to ChatGPT, Claude and any AI agent

**Rosie API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your Rosie account.** Rosie has no public API, no official MCP server and no app in ChatGPT or Claude; its Zapier app only passes new calls and bookings on. Cotlet.dev connects your own signed-in Rosie account to any AI agent, so the AI reviews your call summaries and updates Rosie's answers for you.

## What you can ask

> List calls Rosie took since 6pm with the caller, the reason and who needs a callback.

> Add yesterday's Rosie calls with a job request to my CRM as new leads.

> Find calls where Rosie could not answer and draft the missing answers for her.

> Which callers asked for a quote this week and never booked? Draft a text to each.

> How many calls, bookings and spam calls did Rosie handle this month?

## What business owners ask their AI to do in Rosie

From business owners' posts on Reddit, Facebook groups, review sites and the Rosie help center (checked 25 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Sort last night's calls | "List calls Rosie took since 6pm with the caller, the reason and who needs a callback." | Every morning |
| Calls into my CRM | "Add yesterday's Rosie calls with a job request to my CRM as new leads." | Daily |
| Fix wrong answers | "Find calls where Rosie could not answer a question and draft the missing answers for her knowledge." | Weekly |
| Holiday hours and greeting | "Change Rosie's hours and greeting for the holidays after my OK." | Before holidays |
| Lost quotes | "Which callers asked for a quote this week and never booked? Draft a text to each." | Weekly |
| Monthly call report | "How many calls, bookings, messages and spam calls did Rosie handle this month?" | Monthly |

Your AI asks you before it sends, buys, replies or changes anything. Payments stay in your hands.

## Rosie API

Rosie has no API, webhooks or MCP server; its Zapier app only sends new calls and bookings out (checked 25 Nov 2026). With Cotlet.dev you build your own Rosie API: show your AI agent a job once in your signed-in Rosie account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/rosie

Endpoints you can build in a few minutes (none of them exists until you build it):

- `callbacks`: Time, caller, reason and the message taken
- `unanswered-questions`: Questions Rosie could not answer, with the call
- `bookings`: Caller, service and time of each booking
- `call-stats`: Calls, messages, bookings and spam for the period
- `missed-quotes`: Callers who asked for a quote and did not book
- `mark-handled`: A call you followed up, marked as handled

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/callbacks`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its Rosie tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"rosie":{"command":"uvx","args":["rosie-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t rosie-mcp .
docker run -i --rm -e COTLET_API_KEY rosie-mcp
```

Tools: `rosie_guide`, `rosie_status`, `rosie_open`, `rosie_read`, `rosie_click`, `rosie_type`, `rosie_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install rosie-mcp        # or run it once: uvx rosie-mcp connect
rosie-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 rosie_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 rosie_agent.py status    # is Rosie ready in your Cotlet.dev account
python3 rosie_agent.py guide     # Rosie screens and sign-in link
python3 rosie_agent.py read      # open Rosie, read the page, close the browser
python3 rosie_agent.py tools     # list Cotlet.dev tools
python3 rosie_agent.py serve     # start the stdio MCP server (same as no argument)
```

## What you can ask your AI to do in Rosie

These are real jobs Rosie users asked about. Cotlet lets ChatGPT, Claude or any MCP client do them in your own signed-in Rosie account.

### Calls and callbacks

#### Can ChatGPT or Claude sort last night's calls in Rosie for me?

Yes. Rosie takes the calls and shows a summary, transcript and message for each. With Cotlet, ChatGPT or Claude reads them in your signed-in Rosie dashboard and lists the calls since 6pm with the caller, the reason and who needs a callback first.

Ask your agent: "List the Rosie calls since 6pm yesterday with the caller, the reason and who needs a callback first. Change nothing."

As an API endpoint: Yes. Save the check as an endpoint in Cotlet and call it every morning from n8n, Make or Zapier to send the callback list to your phone.

#### Can ChatGPT or Claude find the callers Rosie quoted who never booked and follow up?

Yes. With Cotlet, your agent reads this week's calls in your Rosie dashboard, lists callers who asked for a quote and did not book, and drafts a short text for each. Cotlet does not send texts itself. Nothing is sent until you say yes.

Ask your agent: "Which Rosie callers asked for a quote this week and never booked? Draft a short text to each that I can send."

As an API endpoint: Yes. Save the missed-quote list as an endpoint in Cotlet and call it every Friday from n8n, Make or Zapier to send the texts through your own phone line.

### Calls into your CRM

#### How do I get Rosie calls into my CRM as new leads?

Partly. Rosie's Zapier app sends new calls and bookings to other apps as they happen. For past calls, summaries and callbacks, Cotlet lets your agent read them in your Rosie dashboard and add each caller with a job request to your CRM. Nothing is added until you say yes.

Ask your agent: "Add yesterday's Rosie calls that asked for a job to my CRM as new leads with name, phone and the reason for the call."

As an API endpoint: Yes. Save the call read as an endpoint in Cotlet and call it daily from n8n or Make to push each new lead into your CRM.

### Answers and hours

#### Can AI teach Rosie the answers she missed and fix wrong answers?

Yes. With Cotlet, your agent reads the calls in your signed-in Rosie dashboard, finds questions Rosie could not answer or got wrong and drafts the missing answers from your notes. It adds them to Rosie's agent settings only after you say yes. Nothing changes until you say yes.

Ask your agent: "Find Rosie calls where she could not answer a question and draft the missing answers for her settings. Change nothing yet."

As an API endpoint: Yes. Save the unanswered-questions list as an endpoint in Cotlet and call it every week from n8n, Make or Zapier to get the new gaps by email.

#### Can ChatGPT or Claude change Rosie's holiday hours and greeting?

Yes. With Cotlet, your agent opens the Agent settings in your signed-in Rosie dashboard and changes the hours and the greeting for the holidays. Rosie keeps answering as before until you say yes. Nothing changes until you say yes.

Ask your agent: "Change Rosie's hours and greeting for Thanksgiving: closed that day, take messages. Show me the new wording first."

### Monthly reports

#### Can ChatGPT or Claude make a monthly call report from Rosie?

Yes. Rosie has no export of calls or bookings. With Cotlet, your agent reads the month in your signed-in Rosie dashboard and counts the calls, bookings, messages and spam calls Rosie handled, so you get one short report.

Ask your agent: "How many calls, bookings, messages and spam calls did Rosie handle this month? Give me one short report."

As an API endpoint: Yes. Save the count as an endpoint in Cotlet and call it on the first of each month from n8n, Make or a script to fill a Google Sheet.

## Questions from Rosie users

**Does Rosie have an API?**

Rosie has no public API for your account (checked 25 Nov 2026). Its Zapier app passes new calls, conversations and bookings to other apps. Cotlet.dev works in your own Rosie account, so your AI agent can read call summaries and update answers.

**How do I connect Rosie to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to Rosie once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no Rosie API key.

**Is there a way to use Claude with Rosie?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own Rosie account.

**Is there a Rosie MCP server?**

Rosie has no official MCP server. Cotlet.dev is a hosted MCP server that works in Rosie for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**Is there an official Rosie app for ChatGPT or Claude?**

As of November 2026, no official Rosie app is listed in the ChatGPT or Claude directories. Cotlet.dev connects your Rosie account to both today with the connector URL `https://cotlet.dev/mcp/v1`.

**Can AI teach Rosie the answers she missed?**

Yes. Your AI reads call summaries, finds the questions Rosie could not answer, drafts answers from your notes and adds them to her settings after your OK.

**How do I get Rosie calls into my CRM?**

Rosie on Zapier sends new calls and bookings out. For past calls, summaries and callbacks, your AI reads them in your Rosie account, or you build an endpoint and call it from your CRM, n8n or Make.

## Find it on

- [Rosie on Cotlet.dev](https://cotlet.dev/integrations/mcp/rosie?utm_source=github&utm_medium=readme&utm_campaign=rosie&utm_content=find)
- [PyPI package rosie-mcp](https://pypi.org/project/rosie-mcp/)
- [Odel](https://odel.app/modules/cotlet/rosie)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/rosie/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-rosie-to-ai-agents-guide)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/rosie?utm_source=github&utm_medium=readme&utm_campaign=rosie

Start free: https://cotlet.dev/signup?app=rosie&utm_source=github&utm_medium=readme&utm_campaign=rosie
