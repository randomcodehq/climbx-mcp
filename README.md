# ClimbX MCP Server

<img src="assets/climbx-icon-512.png" width="96" alt="ClimbX logo" />

The official remote MCP server for [ClimbX](https://climbx.so), an AI co-pilot for growing on X. Bring your ClimbX account into Claude, Cursor, VS Code or any MCP client: Ready Posts in your voice, a Studio co-writer, scheduling and analytics. Built for X creators and indie founders.

- **Endpoint:** `https://climbx.so/mcp`
- **Transport:** Streamable HTTP
- **Auth:** OAuth 2.1 with dynamic client registration — your client opens a ClimbX sign-in page the first time it connects. No API key to copy. Scopes: `climbx:read`, `climbx:write`, `offline_access`. Discovery lives at [`/.well-known/oauth-protected-resource`](https://climbx.so/.well-known/oauth-protected-resource) and [`/.well-known/oauth-authorization-server`](https://climbx.so/.well-known/oauth-authorization-server).
- **Requires:** a ClimbX account ([pricing](https://climbx.so/pricing)).

A bearer API key (`climbx_sk_...`, created at [Settings → API](https://climbx.so/account/settings/api)) also works, passed as `Authorization: Bearer`. Use OAuth when your client supports it.

## What it can do

27 tools, plus ready-made [MCP prompts](https://climbx.so/prompts.md) that show up as slash commands in Claude Desktop and Claude Code:

### Analytics and performance

| Tool | What it does |
| --- | --- |
| `get_account_overview` | Analytics overview: followers, posts, impressions and engagements vs the previous period, with daily series |
| `get_analytics` | Headline KPIs plus a daily impressions series, up to 90 days |
| `get_format_performance` | Medians and trends per post format |
| `get_niche_performance` | Medians and trends per niche |
| `get_posts_to_learn_from` | Best and weakest posts, each with its multiple of median reach |
| `get_posting_habits` | Best posting-hour window, streak and activity |
| `get_weekly_report` | The stored weekly report (takeaway, patterns, reach by weekday) |
| `list_posts` | Recent posts with metrics |

### Voice and learnings

| Tool | What it does |
| --- | --- |
| `get_voice_profile` | Persona, cadence targets and posting schedule |
| `get_learnings` | Current do-more / do-less rules, with evidence |
| `get_learnings_history` | How those rules changed over time |
| `update_voice_profile` | Replace the persona, the tone prompt ClimbX drafts in, and/or the learning mode |

### Ready Posts and planning

| Tool | What it does |
| --- | --- |
| `get_ready_posts` | Today's existing 10 Ready Posts plus generation state (read-only) |
| `prepare_ready_posts` | Create only today's first missing Ready Posts batch |
| `answer_ready_post_question` | Turn the user's own reply to an ask card into one Ready Post |
| `list_scheduled` | Upcoming scheduled posts (optionally one local date) |

### Writing and publishing

| Tool | What it does |
| --- | --- |
| `publish_post` | Publish a post to X now |
| `schedule_post` | Schedule a post (ISO 8601 datetime with offset; pass `ready_post_id` when the text came from Ready Posts) |
| `edit_scheduled_post` | Replace the text of a pending scheduled post in place (same slot, no cap use) |
| `reschedule_post` | Move a pending scheduled post to a new time |
| `cancel_scheduled` | Cancel a pending scheduled post |

### Inspiration

| Tool | What it does |
| --- | --- |
| `get_inspiration_options` | Filter spec and tracked handles; call this once before the outlier tools |
| `get_following_outliers` | Breakout posts from tracked creators, each with a baseline multiplier |
| `get_surprise_outliers` | Network-wide discovery outliers, filterable |

### Engage

| Tool | What it does |
| --- | --- |
| `get_reply_targets` | The account's Engage queue: fresh posts worth replying to, best first |
| `mark_reply_target` | Record a target as replied or skipped so it does not come back |
| `suggest_reply` | One voice-matched reply draft; a suggestion only — the user edits and sends it on X themselves |

Example prompts:
- "Review today's Ready Posts."
- "How did my posts do this month?"
- "What should I reply to?"
- "Queue this post for tomorrow morning."

The server also exposes MCP prompts such as `review_ready_posts`, `review_scheduled_posts`, `find_post_ideas`, `draft_a_post`, `write_like_my_best`, `daily_review`, `analytics_report`, `what_to_post_more`, `week_in_review`, `plan_my_week`, `post_at_my_best_times` and `tune_my_voice`. Full text: https://climbx.so/prompts.md

## Install

The first time a client connects, it opens a ClimbX sign-in page.

### Claude (claude.ai / Claude Desktop)
Settings → Connectors → **Add custom connector** → URL `https://climbx.so/mcp` → Connect, then sign in to ClimbX.

Or add to your Claude Desktop MCP config:

```json
{
  "mcpServers": {
    "climbx": { "url": "https://climbx.so/mcp" }
  }
}
```

### Claude Code
```bash
claude mcp add --transport http climbx https://climbx.so/mcp
```

### Cursor
Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "climbx": { "url": "https://climbx.so/mcp" }
  }
}
```

### VS Code
```bash
code --add-mcp '{"name":"climbx","type":"http","url":"https://climbx.so/mcp"}'
```

Or a workspace `.vscode/mcp.json`:

```json
{
  "servers": {
    "climbx": {
      "type": "http",
      "url": "https://climbx.so/mcp"
    }
  }
}
```

### API key (optional)
If your client cannot complete OAuth, create a key at [Settings → API](https://climbx.so/account/settings/api) and pass it as a header. The full key is shown once; treat it as a secret.

```bash
claude mcp add --transport http climbx https://climbx.so/mcp \
  --header "Authorization: Bearer $CLIMBX_API_KEY"
```

### Clients without remote/OAuth support
Use [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) as a local bridge:

```json
{
  "mcpServers": {
    "climbx": { "command": "npx", "args": ["-y", "mcp-remote", "https://climbx.so/mcp"] }
  }
}
```

## Links
- Website: https://climbx.so
- Tool reference: https://climbx.so/skills.md
- Prompt library: https://climbx.so/prompts.md
- How to get X data through MCP: https://climbx.so/blog/how-to-get-x-data-through-mcp
- Pricing: https://climbx.so/pricing
- Privacy: https://climbx.so/legal/privacy · Terms: https://climbx.so/legal/terms
- Made by [Daniel Smidstrup](https://x.com/DanielSmidstrup)

## License
The contents of this repository (docs and manifests) are MIT licensed. The ClimbX service is subject to the [ClimbX Terms](https://climbx.so/legal/terms).
