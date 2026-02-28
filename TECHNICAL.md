# Technical Implementation

## Architecture

The `solana_deep_forensic` offering runs as a seller service on ACP, using:

- **Helius RPC** for Solana mainnet data
- **DexScreener API** for price/volume
- **Custom bundling algorithm** (<1% diff detection)
- **Risk scoring engine** (0-100)

## Data Sources

1. **getTokenLargestAccounts** - Top 20 holders with amounts
2. **getTokenSupply** - Total supply
3. **getSignaturesForAddress** - First transaction per wallet (age)
4. **DexScreener** - Price, volume, transaction counts

## Core Algorithms

### Bundling Detection

```typescript
for each consecutive pair in top 20:
  diff = |amount_i - amount_{i+1}| / amount_i * 100
  if diff < 1%:
    mark as BUNDLED
```

Threshold: <1% difference = 99% confidence of shared control.

### Risk Scoring

| Factor | Threshold | Points |
|--------|-----------|--------|
| Top 1 >20% | 20% | 30 |
| Top 5 >50% | 50% | 30 |
| ≥3 bundled pairs | count ≥3 | 30 |
| ≥3 fresh top holders | age <24h | 30 |
| 24h gain >100% | >100% | 20 |
| Sells >1.5x buys | ratio >1.5 | 15 |

**Verdicts:**
- 0-29: ✅ LOW RISK
- 30-49: ⚠️ MEDIUM RISK
- 50-79: 🚨 HIGH RISK
- 80-100: 🔴 CRITICAL RISK

## Limitations

- Solana RPC rate limits may affect concurrent requests
- Wallet age requires individual RPC calls (slow for >5 wallets)
- DexScreener data may be delayed for new tokens
- Bundling detection only analyzes consecutive ranks

## Future Improvements

- Add contract code analysis (mint authority, freeze authority)
- Liquidity lock detection via Raydium/Orca APIs
- Wash trading detection via transaction graph
- MEV bot activity scoring

---

See [ACP Seller Reference](../../references/seller.md) for offering configuration.
