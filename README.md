# Agent-ID Claim & Mint Toolkit

**Let .ai Agent ID holders claim and buy your token through their Binance
Agentic Wallet — with one copied prompt. No wallet connect on your site, no
seed phrases, preview → confirm on every transaction.**

This is the exact flow running live in production on a BSC mainnet mint
(free claims, paid lots, referral rewards, Agent-ID gate). This repo packages
it so any small BSC project can launch with it.

## What's inside

| Path | What it is |
|---|---|
| `demo/index.html` | Single-file demo page: claim + buy prompt panels with copy buttons. Edit only the `CONFIG` block. |
| `prompts/claim-prompt.txt` | Standalone free-claim prompt template (`{{PLACEHOLDERS}}`). |
| `prompts/buy-prompt.txt` | Standalone buy-lots prompt template, with referral support. |
| `docs/INTEGRATION.md` | What your contract must expose, the Agent ID gate, and the go-live checklist. |
| `PITCH.md` | The service offer (packages, prices, pitch scripts) — for the seller, not the client. |
| `VIDEO-SCRIPT.md` | Shot-by-shot script for the 2-minute demo video. |

## Why projects want it

- **Agent-only distribution**: your mint is gated to verified Agent ID
  wallets — the audience Binance is actively growing.
- **Zero integration risk for users**: nothing to connect, nothing to sign
  blindly; every call is previewed by the user's own agent first.
- **Scam-resistant**: prompts pin your official contract addresses, so
  lookalike sites can't redirect the agent.
- **Fast**: page + prompts can be live the same day your contract is.

## Requirements

- BSC (BNB Smart Chain, chainId 56) mint contract with `claimFree()` /
  `buyLots(qty, referrer)` (or equivalents — prompts adapt)
- An Agent ID gate (`hasAgentId`) — available in the contract package
- Any static hosting

See `docs/INTEGRATION.md` for the full checklist.

## License / use

Demo and templates © the author. Client deployments are delivered under the
service packages in `PITCH.md`.
