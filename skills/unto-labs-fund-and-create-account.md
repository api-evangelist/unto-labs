---
name: Set up the Thru CLI, create an account, and fund it from the faucet
description: The documented happy path from zero to a funded on-chain Thru
  account on Alphanet using the first-party thru CLI.
api: cli/unto-labs-cli.yml
operations: ["thru getversion", "thru keys generate", "thru account create",
  "thru faucet withdraw"]
generated: '2026-07-21'
method: generated
source: https://thru.org/docs/program-development/setting-up-thru-devkit/
---

# Set up the Thru CLI, create an account, and fund it

## Steps

1. Install the CLI (prebuilt binary; no Rust toolchain needed):

   ```bash
   npm i -g thru
   thru --help
   ```

2. Verify connectivity against the default public Alphanet endpoint
   (`https://rpc.alphanet.thru.org`, configurable via `rpc_base_url` in
   `~/.thru/cli/config.yaml`):

   ```bash
   thru --json getversion
   ```

3. Generate a keypair and create the on-chain account (account creation uses a
   fee-payer state proof under the hood):

   ```bash
   thru keys generate default
   thru account create default
   ```

4. If the account cannot pay for writes yet, fund it from the faucet program
   (withdraw is capped at 10000 per transaction):

   ```bash
   thru faucet withdraw default 1000
   ```

## Rules

- The keypair is stored as plaintext hex in `~/.thru/cli/config.yaml` — never
  share or commit it.
- The install workflow changes frequently until v1.0.0; update with
  `npm i -g thru@latest`.
- Addresses print as thrufmt `ta...` strings; transaction signatures as `ts...`.
