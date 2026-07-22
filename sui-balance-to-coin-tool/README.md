# Sui Balance-to-Coin Tool

Converts SUI sitting in your Sui **address balance** (introduced by the May 2026
v1.72 upgrade) into a real, spendable `Coin<SUI>` object — so `sui client
publish` / `sui client call` can find a gas coin.

> **You run this as part of Step 5** of the main workshop guide
> (see the repo's `README.md` → "Prepare Your Gas Coin"). This file is just a
> quick reference for the tool itself.

## Why this exists

Since v1.72, SUI can sit in two places: classic coin objects, or the newer
"address balance." Wallets and mentor transfers increasingly send funds via the
address-balance path. `sui client balance` shows both combined, but
`sui client publish` / `sui client call` need an actual coin object to pay gas
with — so the balance looks fine while gas payment still fails with
"Cannot find gas coin for signer address...". This tool moves a small amount out
of the address balance and into a coin object so gas payment works.

## Setup

1. This folder (`sui-balance-to-coin-tool/`) ships inside the workshop repo,
   next to `portfolio_contract` and `portfolio_frontend`.
2. In the **VS Code terminal**, from the repo root, go into this folder:
   ```powershell
   cd sui-balance-to-coin-tool
   ```
3. Install its dependencies (first time only):
   ```powershell
   npm install
   ```
4. Make sure you have already run `sui client addresses` (workshop Step 3) —
   this tool reads the same keystore file that command created, so there is
   nothing extra to configure for your key.

## Usage

Run this from inside the `sui-balance-to-coin-tool` folder. The number is the
amount to convert, in **MIST** (1 SUI = 1,000,000,000 MIST):

```powershell
npm run balance:to-coin:minimal -- <amount_in_mist>
```

Default for the workshop — convert 0.05 SUI, which comfortably covers the
publish (Step 5) plus the portfolio call (Step 6):

```powershell
npm run balance:to-coin:minimal -- 50000000
```

| You want as a coin | MIST to pass |
|---|---|
| 0.02 SUI (minimum) | `20000000`  |
| 0.05 SUI (default) | `50000000`  |
| 0.1 SUI            | `100000000` |

Then confirm it worked:

```powershell
sui client gas
```

You should now see a coin object listed. Go back to the main workshop guide and
run the command that needed it (`sui client publish` in Step 5, or
`sui client call` in Step 6).

## Notes / assumptions

- Assumes the default **ed25519** key scheme (option `0` when `sui client
  addresses` first asked for a key scheme) — the same default this workshop
  uses. Secp256k1/secp256r1 keys are skipped.
- Defaults to reading the keystore from `~/.sui/sui_config/sui.keystore`
  (Windows: `C:\Users\<you>\.sui\sui_config\sui.keystore`). Override with the
  `SUI_KEYSTORE_PATH` environment variable if yours lives elsewhere.
- If your keystore has more than one address, set `SUI_ADDRESS=0xyouraddress`
  before running so it picks the right key.
- Defaults to mainnet (the network this workshop deploys to). Set
  `SUI_NETWORK=testnet` (or `devnet`) if needed.
- This never asks for or stores your private key anywhere new — it only reads
  the keystore file the `sui` CLI itself already created, and only moves SUI
  back to **your own** address.
