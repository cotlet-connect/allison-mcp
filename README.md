# Allison MCP server

Connect Allison to ChatGPT, Claude and other AI agents through Cotlet.dev. Review cleaning jobs, prepare agreed booking moves and draft customer follow-ups in your signed-in Allison business account.

## What you can ask

> Read tomorrow's cleaning jobs, arrival windows and assigned cleaners.

> Prepare the recurring booking move I agreed with the household and show the changes before saving.

> Draft a follow-up in the selected customer conversation and show the recipient and text before sending.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings > Connectors > Add custom connector: `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its Allison tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 allison_agent.py serve
```

Or with Docker:

```bash
docker build -t allison-mcp .
docker run -i --rm -e COTLET_API_KEY allison-mcp
```

Tools: `allison_guide`, `allison_status`, `allison_open`, `allison_read`, `allison_click`, `allison_type`, `allison_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 allison_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 allison_agent.py status    # is Allison ready in your Cotlet.dev account
python3 allison_agent.py guide     # Allison screens and sign-in link
python3 allison_agent.py read      # open Allison, read the page, close the browser
python3 allison_agent.py tools     # list Cotlet.dev tools
python3 allison_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Allison users

**How do I connect my account?**

Request access at https://cotlet.dev/signup?app=allison, sign in to Allison in your Cotlet.dev Cloud Browser and enable it for agents.

**Does Allison offer an official MCP?**

Allison documents an official read-only MCP. The live integration page compares its scope with the account-screen workflows provided by Cotlet.dev.

**Can I prepare a booking change?**

Select the existing visit and the agreed new date and time. Review the proposed booking and cleaner changes before committing them.

**Is Cotlet.dev affiliated with Allison?**

No. Cotlet.dev is independent and not affiliated with Allison.

## Learn more

- [Allison integration and hosted tools](https://cotlet.dev/integrations/allison-api-mcp-server)

---

More: https://cotlet.dev/integrations/allison-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=allison

Request access: https://cotlet.dev/signup?app=allison&utm_source=github&utm_medium=readme&utm_campaign=allison

## Workflows on the hosted integration

Examples documented on the live integration page:

- `schedule_view`
- `schedule_jobs`
- `reschedule_calendar`
- `edit_job`
- `jobs_attention`
- `conflict_detection`
- `assign_cleaners`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
