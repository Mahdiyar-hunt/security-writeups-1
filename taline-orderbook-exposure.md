# Sensitive Data Exposure in Public API

**Target:** `wall.tlyn.ir`  
**Endpoint:** `/api/orderbook`  
**Type:** Sensitive Data Exposure  
**Severity:** Critical  
**Status:** Reported (awaiting triage)

---

## Summary

The public API endpoint `/api/orderbook` exposes sensitive market data without any authentication. This includes the exchange's gold reserve (`chest`), wallet balance (`wallet`), full list of active buy and sell orders with exact prices and amounts, and internal pricing parameters (`main_price`, `buy_quote`, `sell_quote`).

Any user from anywhere in the world can access this data with a single GET request.

---

## Steps to Reproduce

1. Send a GET request to:
https://wall.tlyn.ir/api/orderbook

text

2. The response contains:
- `chest` — gold reserve
- `wallet` — rial wallet balance
- `active_orders` — full order book
- `prices` — internal pricing parameters

3. Add the `limit` parameter to retrieve more data:
https://wall.tlyn.ir/api/orderbook?limit=1000

text

---

## Proof of Concept

```bash
curl -s "https://wall.tlyn.ir/api/orderbook"
Partial response:

json
{
  "status": true,
  "data": {
    "chest": 2.034,
    "wallet": 206902,
    "active_orders": {
      "buy": [
        {"subject": "get", "quote": "247922000", "total_gram": 0.152, "order_count": 1}
      ],
      "sell": [
        {"subject": "pay", "quote": "248000000", "total_gram": 0.38, "order_count": 1}
      ]
    },
    "prices": {
      "main_price": 247875500,
      "buy_quote": 248867000,
      "sell_quote": 246884000,
      "lower_price": 198300400,
      "upper_price": 297450600,
      "min_gram": 0.01,
      "min_wallet": 1983004,
      "max_gram": 3000
    },
    "last_price": 248000000
  }
}
