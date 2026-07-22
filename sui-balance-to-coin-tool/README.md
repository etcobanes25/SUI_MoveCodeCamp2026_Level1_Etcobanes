# Sui Balance-to-Coin Tool

Converts SUI sitting in your Sui **address balance** (introduced by the May 2026
v1.72 upgrade) into a real, spendable `Coin<SUI>` object — fixing the
"Cannot find gas coin for signer address..." error.

## Do I need this tool?

**Most workshop attendees will NOT need this.** Only use it if BOTH of these are true:

1. `sui client balance` shows you have SUI (for example, 0.05 SUI or more), **but**
2. `sui client publish` (Step 5) or `sui client call` (Step 6) fails with an error
   like **"Cannot find gas coin for signer address..."**

If your publish/call commands work normally, you can ignore this folder entirely.

## Why this exists

Since v1.72, SUI can sit in two places: classic coin objects, or the newer
"address balance." Wallets like Slush increasingly send funds via the
address-balance path. `sui client balance` shows both combined, but
`sui client publish` / `sui client call` need an actual coin object to pay
gas with — so the balance looks fine while gas payment still fails. This tool
moves a small amount out of the address balance and into a coin object so gas
payment works again.

## Setup

1. This folder (`sui-balance-to-coin-tool/`) sits next to `portfolio_contract`
   and `portfolio_frontend` inside your project. You can also copy it anywhere
   else on your machine if you prefer.
2. In the **VS Code terminal**, go into this folder:
   ```powershell
   cd sui-balance-to-coin-tool
   ```
3. Install its dependencies:
   ```powershell
   npm install
   ```
4. Make sure you have already run `sui client addresses` at some point (from
   the workshop setup in Step 3) — this tool reads the same keystore file that
   command created, so there is nothing extra to configure for your key.

## Usage

Run this from inside the `sui-balance-to-coin-tool` folder. Replace the number
with the amount you want to convert, in **MIST** (1 SUI = 1,000,000,000 MIST):

```powershell
npm run balance:to-coin:minimal -- <amount_in_mist>
```

Example — convert 0.02 SUI (20,000,000 MIST), which is plenty for gas:

```powershell
npm run balance:to-coin:minimal -- 20000000
```

Then confirm it worked:

```powershell
sui client gas
```

You should now see a coin object listed. Once it appears, go back to the main
workshop guide and re-run the command that was failing (`sui client publish` in
Step 5, or `sui client call` in Step 6).

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
- This never asks for or stores your private key anywhere new — it only
  reads the keystore file the `sui` CLI itself already created.
