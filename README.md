# Google Flights Fares MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-google-flights-fares)](https://www.npmjs.com/package/@mambalabsdev/mcp-google-flights-fares) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-google-flights-fares)](https://github.com/mambalabsdev/mcp-google-flights-fares/blob/main/LICENSE)

An MCP server for the Mamba Labs Google Flights Fares actor on Apify. Google Flights fares as rows.

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

Takes routes and travel dates and returns one row per flight option from Google Flights with price, airlines, flight numbers, departure and arrival times, duration, stops, layovers, and emissions.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-google-flights-fares": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-google-flights-fares"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_google_flights_fares` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "What do flights from Boston to Lisbon cost on March 3, returning March 10?"
- "Find nonstop one way fares from SFO to Seattle for the next three Fridays."

## Tool

`get_google_flights_fares`. One tool. Example arguments:

```json
{
  "routes": [
    "BOS to LIS"
  ],
  "departure_dates": [
    "2027-03-03"
  ],
  "trip_type": "round_trip",
  "trip_days": 7,
  "max_flights_per_search": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `routes` | array | no | One route per line, origin to destination, for example JFK to LAX, JFK-LAX, or New York to London. Airport codes (3 capital letters) give exact airports; a city name lets Google pick the city's airports. From Clay, put the column inside a one item list. |
| `departure_dates` | array | no | One or more outbound dates, YYYY-MM-DD, up to 330 days ahead. Each date is its own search. Empty: 21 days from today. |
| `trip_type` | string | no | round_trip (default) or one_way. A round trip price is the round trip total, and the row lists the outbound flights. |
| `trip_days` | number | no | Round trip only: days between departure and return, 1 to 60. Default 7. Ignored when return_date is set. |
| `return_date` | string | no | Optional. YYYY-MM-DD. Works with one departure date only; for several dates use trip_days. |
| `cabin` | string | no | economy (default), business, or first. Premium economy is not supported. |
| `adults` | number | no | Adult passengers, 1 to 9. Default 1. price_total is for all passengers; price_per_adult divides it. |
| `currency` | string | no | ISO currency code for the prices, for example USD, EUR, GBP. Default USD. |
| `max_flights_per_search` | number | no | 1 to 100. Default 30. The actor reads the flight list Google renders on the search page, usually 6 to 35 options. |
| `nonstop_only` | boolean | no | true: keep only nonstop options; others are left out and not charged. Default false. |
| `dedupe` | boolean | no | true (default): an option already returned for the same route and dates in this run is skipped and not charged. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `flight-row`, one priced flight option | $0.001 | $0.00095 | $0.0009 | $0.00085 |
| `apify-actor-start`, per run start | $0.005 | $0.005 | $0.005 | $0.005 |

Options with no price, repeats, rows your filters removed, summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/google-flights-fares](https://apify.com/mambalabs/google-flights-fares)

## License

MIT. See [LICENSE](LICENSE).
