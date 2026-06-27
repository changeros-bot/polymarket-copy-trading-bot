# Josh Deployment Guide

Date: 2026-06-27

## Project direction

This repository is the main production base for the Polymarket copy trading system.

The strategy is to reuse the open source architecture instead of rebuilding the trading engine from scratch.

## Current rule

Do not change the core trading engine unless there is a confirmed bug.

Safe customization is limited to:

- mobile-first dashboard wording
- Chinese UI labels
- deployment documentation
- safe preview configuration
- helper tools for finding wallet addresses
- reporting and AI scoring layers that read existing analysis outputs

## First deployment target

Deploy only the web dashboard first.

Vercel project settings:

```text
Framework Preset: Next.js
Root Directory: web
Build Command: npm run build
Install Command: npm install
Output Directory: .next
```

## Safe environment variables

Initial mode must be read-only / preview-only.

```env
PREVIEW_MODE=true
COPY_SIZE=1
MAX_ORDER_SIZE_USD=2
TRADE_AGGREGATION_ENABLED=false
USER_ADDRESSES=0x6d9fc316c3b8377060a44b852ba664adbfd59790
```

`USER_ADDRESSES` is the source of truth for the bot. Username lookup is only a helper for discovering wallet addresses.

## Runtime split

Use Vercel for:

- dashboard
- settings UI
- analysis display
- read-only API routes

Use Railway / Render / VPS for:

- long-running bot process
- scheduled monitoring
- live copy trading loop
- Telegram notifications

Do not run real-money trading from Vercel serverless functions.

## First validation

1. Deploy `web/` to Vercel.
2. Confirm the dashboard builds successfully.
3. Confirm the web dashboard can load trader analytics.
4. Keep `PREVIEW_MODE=true`.
5. Track only MEPP wallet first.
6. Run simulated tracking for 7-14 days before considering real trading.

## Initial tracked wallet

MEPP verified wallet:

```text
0x6d9fc316c3b8377060a44b852ba664adbfd59790
```

## Non-goals

- Do not rebuild the copy trading engine.
- Do not replace the original analytics pipeline.
- Do not enable private keys during MVP validation.
- Do not add real order execution before simulation proves positive expected value.
