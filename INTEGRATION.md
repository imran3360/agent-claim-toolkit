# Integration Guide — Agent-ID Claim & Mint Toolkit

What a project needs, and the exact steps to go live.

## How it works (30 seconds)

Users never connect a wallet to your site and never type a seed phrase anywhere.
Your page carries two pre-written **agent prompts** (free claim + buy lots).
The user copies one, pastes it into their **Binance Agentic Wallet**, and the
agent runs it: it reads your mint contract, shows a **preview**, and executes
only after the user replies **"confirm"**. The prompts pin your official
contract addresses, so the agent cannot be redirected to a lookalike contract
by a malicious page.

Proven live: this exact flow runs in production on a BSC mainnet mint
(Agent-ID gated, free claims + paid lots + referrals).

## What your project must have

1. **A mint/sale contract on BSC (chainId 56)** exposing, at minimum:
   - `claimFree()` — free claim, gated (see 2)
   - `buyLots(uint256 qty, address referrer)` payable — or your equivalent
   - Read functions the prompts use: `paused()`, `freeRound`,
     `freeClaimsThisRound`, `freeAmountsForRound(round)`,
     `paidRound`, `lotsSoldThisRound`, `LOT_PRICE`,
     `hasFreeClaimed(address)`
   - If your function names differ, we adapt the prompts — the flow is the same.
2. **An Agent ID gate**: the contract must be able to check that the caller
   holds a `.ai` Agent ID (e.g. `hasAgentId(address)` via the .ai identity
   registry / an adapter). This is what makes the mint "agent-only" and is the
   core of the offer. If you don't have the gate yet, it's part of the
   contract package (see PITCH.md).
3. **A static page host** (Cloudflare Pages, Netlify, Hostinger — anywhere).
   The demo page is a single HTML file; no backend, no database.

## Go-live checklist

- [ ] Contracts deployed on BSC mainnet; addresses verified on BscScan
- [ ] Gate tested: an Agent ID wallet passes, a plain wallet is rejected
- [ ] `demo/index.html` CONFIG filled with your real values
      (token name/symbol, site URL, both contract addresses, lot price in
      BNB **and** wei, max lots per wallet, liquidity note, referrer rule)
- [ ] Prompts in `prompts/` filled with the same values (the page generates
      them from CONFIG automatically; the .txt files are the standalone copies)
- [ ] Test claim with a real Agent ID wallet: copy prompt → paste → preview
      matches the intended call → confirm → txHash on BscScan
- [ ] Test buy with 1 lot; check referral credit lands correctly
- [ ] Page live on your domain; contract addresses on the page match the
      prompts exactly (character for character)

## Safety rules baked into every prompt

- Preview → explicit "confirm" → execute. No silent transactions.
- Address pinning: the agent must refuse any other address or chain.
- Seed phrase / private key: the agent is instructed to **never** ask.
- On-chain / web content is treated as untrusted data, never as instructions
  (prompt-injection hygiene).
- Developer Mode requirement is checked up front, with a clear user message.

## Limits / honest notes

- The flow depends on the user's Binance Agentic Wallet + Developer Mode.
  Users without an Agent ID are stopped at Step 0 with instructions.
- A direct in-browser claim (wallet connect) is a different product; this
  toolkit is deliberately prompt-based because that is the path that works
  with Agentic Wallets today.
