# cobalt-site

Public data for Cobalt, an art game about the electricity your computer uses
while you play.

## The ledger

<https://mattiawasfound.github.io/cobalt-site/v1/ledger.json>

What all players together have spent on electricity while playing, and what
they have donated to offset it, per country. It is rebuilt once a day, at
about 03:20 UTC, and holds no usernames and nothing about any single player.
Anyone can read it, from any website.

```json
{
  "schema_version": 1,
  "generated_at": "2026-10-04T14:00:00Z",
  "currency": "EUR",
  "global": {
    "players": 1234, "accrued": 5321.12, "artificial": 880.5, "paid": 410.0,
    "artificial_by_tag": { "some_tag": 700.0, "other_tag": 180.5 }
  },
  "countries": {
    "DK": { "players": 40, "accrued": 212.3, "artificial": 31.0, "paid": 12.5, "zones": {} },
    "SE": {
      "players": 25, "accrued": 98.1, "artificial": 12.0, "paid": 3.0,
      "zones": { "SE3": { "players": 14, "accrued": 61.0, "artificial": 8.0, "paid": 2.0 } }
    }
  }
}
```

- **Amounts** are euros, rounded to 4 decimals.
- **`accrued`** is what the players' electricity cost while they played,
  estimated from their computer's power draw and the local price.
- **`artificial`** is the sum of the game's other tracked costs, listed per
  tag in `artificial_by_tag`.
- **`paid`** is what the players donated to offset it.
- **Countries** are ISO 3166-1 alpha-2 codes. **Zones** are electricity price
  zones within a country. Players without a zone count in their country
  only.
- **Small places:** a country or zone with too few players is left out of
  the list, but its players still count in `global` and in their country.
- `schema_version` changes only if the format breaks.

## How it is published

A scheduled GitHub Action ([publish.yml](.github/workflows/publish.yml))
fetches the day's file from the game's server with a read token, checks
it, and publishes it here on GitHub Pages. If a day's fetch fails, the
previous day's file stays up.
