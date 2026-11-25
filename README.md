# YouTube Keyword Suggestions MCP Server

[![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-youtube-keyword-suggestions%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-youtube-keyword-suggestions&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-youtube-keyword-suggestions)](https://www.npmjs.com/package/@mambalabsdev/mcp-youtube-keyword-suggestions) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-youtube-keyword-suggestions)](https://www.npmjs.com/package/@mambalabsdev/mcp-youtube-keyword-suggestions) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-youtube-keyword-suggestions)](https://github.com/mambalabsdev/mcp-youtube-keyword-suggestions/blob/main/LICENSE)

An MCP server for the Mamba Labs YouTube Keyword Suggestions actor on Apify. YouTube autocomplete keywords as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes seed keywords and returns YouTube's search suggestions for each, expanded with letters, question words, or prepositions, as one row per unique suggestion with its rank, plus a free summary row per seed.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-youtube-keyword-suggestions": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-youtube-keyword-suggestions"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_youtube_keyword_suggestions` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Give me YouTube keyword ideas for lofi music in the US, expanded A to Z."
- "What do people search on YouTube that starts with how to edit videos, in Germany in German?"

## Tool

`get_youtube_keyword_suggestions`. One tool. Example arguments:

```json
{
  "keywords": [
    "lofi music"
  ],
  "expand": "a-z",
  "max_suggestions_per_keyword": 50
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `keywords` | array | no | One seed per item, for example lofi music or how to edit videos. Up to 500 seeds per run. As text, put one seed per line. |
| `expand` | string | no | a-z (default): the seed, then the seed followed by each letter a to z (27 queries). questions: the seed, then how, what, why, when, where, which, who, can, is, does, will, and are before it (13 queries). prepositions: the seed, then for, with, without, to, vs, near, like, in, and on after it (10 queries). all: all three (48 queries). none: the seed only (1 query). |
| `country` | string | no | Two letter country code for the suggestions, for example US, GB, DE, IN. Default US. |
| `language` | string | no | Two letter language code, for example en, de, es, ja. Default en. |
| `max_suggestions_per_keyword` | number | no | Unique suggestions returned per seed, across all its queries. Default 300, maximum 1000. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `suggestion-row`, one unique suggestion | $0.0003 | $0.000285 | $0.00027 | $0.000255 |
| `apify-actor-start`, per run start | $0.004 | $0.004 | $0.004 | $0.004 |

Repeats within a seed, keyword summaries, seeds with no suggestion, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/youtube-keyword-suggestions](https://apify.com/mambalabs/youtube-keyword-suggestions)

## License

MIT. See [LICENSE](LICENSE).
