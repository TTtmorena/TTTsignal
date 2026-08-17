# TTTsignal — Advanced API Reference

This document contains the official and recommended data sources used by **TTTsignal**.

---

## 1. Primary Endpoint — Token Fees (Most Important)

**GET** `https://api.bankr.bot/token-launches/{tokenAddress}/fees?days=30`

Legacy (still working but deprecated):  
`https://api.bankr.bot/public/doppler/token-fees/{tokenAddress}?days=30`

### Critical Fields for Signal Generation
| Field | Description | Usage in TTTsignal |
|-------|-------------|--------------------|
| `lifetimeEarnedWeth` | Total lifetime fees earned | Fee strength & sustainability |
| `totals.claimableWeth` | Currently claimable fees | Recent activity indicator |
| `dailyEarnings[]` | Array of `{ date, weth }` | **Core for momentum analysis** |
| `lifetimeBestDay` | Best single day earnings | Peak performance reference |
| `tokens[0].share` | Creator fee share (usually ~57%) | Fee quality check |

### Example Response
```json
{
  "address": "0x...",
  "chain": "base",
  "days": 30,
  "lifetimeEarnedWeth": "1.2345",
  "lifetimeDays": 42,
  "lifetimeBestDay": {
    "date": "2026-03-22",
    "weth": "0.0891"
  },
  "dailyEarnings": [
    { "date": "2026-08-10", "weth": "0.0123" },
    { "date": "2026-08-11", "weth": "0.0087" },
    { "date": "2026-08-12", "weth": "0.0156" }
  ],
  "totals": {
    "claimableWeth": "0.0456",
    "claimedWeth": "1.1889",
    "claimCount": 5
  },
  "tokens": [
    {
      "tokenAddress": "0x...",
      "name": "Example",
      "symbol": "EXM",
      "share": "57.00%",
      "claimable": {
        "token0": "0.0456",
        "token1": "12345.67"
      }
    }
  ]
}
```

**Rules:**
- Always request `days=30` by default (allowed range: 1–90)
- Use `dailyEarnings` to detect fee momentum (rising / falling / spiking)
- Convert all WETH values to approximate USD using current ETH price

---

## 2. Recent Launches (Hot Tokens Scanner)

**GET** `https://api.bankr.bot/token-launches`

Returns the 50 most recent Bankr token launches.  
Use this endpoint when user asks for:
- “hot tokens”
- “volume spikes”
- “trending on Bankr”
- “new launches with momentum”

---

## 3. Agent Profiles

**GET** `https://api.bankr.bot/agent-profiles?sort=marketCap&limit=20`  
**GET** `https://api.bankr.bot/agent-profiles/{slug-or-address}`

Useful fields:
- `marketCapUsd`
- `weeklyRevenueWeth`
- Agent metadata (name, image, etc.)

Use for ranking and comparison signals.

---

## 4. Market Data Sources (Price • Volume • Liquidity • Holders)

Priority order (use the first available):

1. **GeckoTerminal** / **DexScreener** (preferred)
2. **Birdeye**
3. **Zerion** or **Alchemy** (on-chain)
4. CoinGecko (fallback)

### Required Metrics
- Current Price (USD)
- 24h Price Change (%)
- 24h Volume (USD)
- Liquidity (USD)
- Holders count (when available)
- Liquidity / Market Cap ratio → critical for Risk Level

---

## 5. Best Practices for TTTsignal

- Cache all responses for 1–2 minutes within the same conversation
- Never invent or estimate missing numbers
- Always show both WETH and approximate USD
- Liquidity / Market Cap ratio is mandatory for Risk Assessment
- Prefer the newest Bankr endpoints over legacy ones
- If data is incomplete → lower Confidence level and clearly state “Limited data”

---

**TTTsignal** uses these endpoints to deliver the most accurate and actionable trading signals in the Bankr ecosystem.
```
