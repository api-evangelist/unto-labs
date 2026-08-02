---
name: Inspect Thru chain state with the Explorer MCP
description: Ground an agent in live Thru (Alphanet) chain context — blocks,
  transactions, accounts, and on-chain ABIs — using the official hosted Explorer
  MCP server instead of scraping pages.
api: mcp/unto-labs-mcp.yml
operations: [get_block, get_transaction, get_account, list_account_transactions,
  list_recent_blocks, list_recent_transactions, search, get_program_abi]
generated: '2026-07-21'
method: generated
source: https://thru.org/docs/api-ref/explorer-mcp/tools-reference/
---

# Inspect Thru chain state with the Explorer MCP

Use the official hosted MCP server when you need live chain context. It is
read-only; to mutate chain state use the `thru` CLI or an SDK.

## Setup

```bash
claude mcp add --transport http thru-explorer https://scan.thru.org/api/mcp
```

Optionally target another network with `?rpc=<url>` (devnet, staging).

## Steps

1. If you have an unknown identifier, call `search` with the value. It resolves
   whether it is a slot, a transaction signature (`ts...`), or an account
   address (`ta...`). All addresses/signatures use thrufmt encoding.
2. For a block: call `get_block` with the slot (non-negative integer) to see the
   producer, timestamps, and transaction rows. For recent activity call
   `list_recent_blocks` or `list_recent_transactions` (`limit` 1-50).
3. For a transaction: call `get_transaction` with the signature to inspect
   accounts, instructions, and events.
4. For an account: call `get_account` with the address for metadata, balance,
   owner, and flags; walk history with `list_account_transactions`
   (`pageSize` 1-100, cursor `pageToken`).
5. For a deployed program: call `get_program_abi` with the program address to
   fetch the on-chain ABI used for explorer reflection.

## Rules

- Do not assume Solana or EVM semantics — Thru programs are C programs running
  on the Thru VM (see llms/unto-labs-llms.txt).
- Reads can fail with NOT_RETAINED (data past its retention TTL), which is
  distinct from NOT_FOUND — see errors/unto-labs-error-codes.yml.
- Every tool returns an LLM-oriented text format; prefer it over scraping
  scan.thru.org HTML.
