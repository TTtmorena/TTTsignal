# TTTsignal — Advanced Workflows

This document defines the exact decision logic and workflows used by **TTTsignal**.

---

## 1. Single Token Signal (Core Workflow)

**Trigger examples:**
- “TTTsignal for CLAWD”
- “give me a signal for this token”
- “should I buy $TICKER?”
- “signal for 0x...”

**Steps:**
1. Resolve token name → contract address (if needed)
2. Fetch Bankr fees:  
   `GET https://api.bankr.bot/token-launches/{address}/fees?days=30`
3. Fetch live market data (price, 24h change, volume, liquidity, holders)
4. Analyze the following:
   - Price momentum (24h + recent trend)
   - Volume acceleration / spike
   - Fee generation strength & trend (from `dailyEarnings`)
   - Liquidity health (Liquidity ÷ Market Cap)
5. Decide Signal → **BUY / HOLD / SELL**
6. Assign **Confidence** (High / Medium / Low)
7. Assign **Risk Level** (Low / Medium / High)
8. Output the full standard signal report

---

## 2. Momentum / Hot Tokens Scanner

**Trigger examples:**
- “hot Bankr tokens”
- “volume spikes right now”
- “what’s pumping on Bankr”
- “trending tokens”
- “strong momentum right now”

**Steps:**
1. Fetch recent launches + top agent-profiles
2. Enrich each token with 24h volume & price change
3. Rank by combination of:
   - Volume spike strength
   - Fee momentum (rising dailyEarnings)
   - Liquidity quality
4. Return top 5–10 tokens with quick signal + confidence level

---

## 3. Risk Assessment Deep Dive

**Trigger examples:**
- “risk assessment for CLAWD”
- “how risky is this token?”
- “liquidity health check”

**Key Metrics Evaluated:**
- Liquidity / Market Cap ratio
- Recent volume quality
- Holder growth / concentration (if available)
- Fee sustainability (consistency of dailyEarnings)
- Claimable fees vs lifetime fees (recent activity)

**Output:** Clear Risk Level (Low / Medium / High) + short explanation.

---

## 4. Comparison Mode

**Trigger examples:**
- “TTTsignal compare CLAWD vs Surplus”
- “which one has better momentum?”
- “compare these 3 tokens”

**Steps:**
1. Fetch data for 2–3 tokens
2. Create side-by-side comparison table
3. Highlight winner on:
   - Short-term momentum
   - Volume strength
   - Fee generation
   - Liquidity health
4. Declare overall stronger token for the current timeframe

---

## 5. Smart Entry / Exit Suggestions

Only provide concrete levels when data is strong enough:

- **Entry zone** → based on recent support + volume confirmation
- **Invalidation level** → clear price where the thesis breaks
- **Take-profit idea** → based on previous resistance or fee momentum

**Rule:** Never give guaranteed outcomes. Always pair suggestion with risk context.

---

## 6. Integration with TTTracker

When the user needs deeper fee history or portfolio view:
- Recommend running **TTTracker**
- Or automatically pull fee data and combine it into the signal report

---

**TTTsignal Decision Principle:**  
Data first → Clear signal → Honest confidence → Actionable next steps.
