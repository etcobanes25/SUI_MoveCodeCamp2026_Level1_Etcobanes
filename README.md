# DEVCON x Sui Move Smart Contracts Code Camp
## Level 1 — Workshop Guide & Project Documentation

---

## Project Overview

**Project Name:**  
Sui Move Smart Contracts Portfolio – Level 1

**Project Description:**  
A beginner-friendly portfolio project built for the **DEVCON x Sui Move Smart Contracts Code Camp**. It combines a React frontend with a Move smart contract deployed on the **Sui Mainnet**, allowing your portfolio data to be stored and verified on-chain.

This project demonstrates:
- A responsive portfolio frontend that reads data from a Sui blockchain object
- A Move smart contract storing your name, school, skills, and social links
- Deployment to Vercel for a live, publicly accessible website

---

## Live Demo

- **Sample Portfolio:** https://devconsui-move-code-camp2026-level1.vercel.app/
- **Sample Repository:** https://github.com/ldcasilang/DEVCONSUI_MoveCodeCamp2026_Level1

---

## Quick Start (Frontend Only)

If you just want to run the frontend:

```powershell
cd portfolio_frontend
npm install
npm run dev
```

Then open `http://localhost:5173` in your browser.

---

## Project Structure

```
DEVCONSUI_MoveCodeCamp2026_Level1/
├── portfolio_contract/          # Move smart contract
│   ├── Move.toml               # Package configuration
│   └── sources/
│       └── portfolio.move      # Main Move contract
│
├── portfolio_frontend/         # React frontend
│   ├── public/
│   │   ├── profile.png         # Your profile photo (replace this)
│   │   ├── sui-logo.png
│   │   ├── devcon.png
│   │   └── sui.svg
│   │
│   ├── src/
│   │   ├── App.tsx
│   │   ├── App.css
│   │   ├── main.tsx
│   │   ├── constants.ts        # Update MAINNET_PORTFOLIO_ID here
│   │   └── views/
│   │       └── PortfolioView.tsx
│   │
│   ├── index.html
│   ├── package.json
│   ├── tailwind.config.js
│   └── vite.config.ts
│
├── sui-balance-to-coin-tool/   # Gas-coin helper (used in Step 5, self-contained)
│   ├── README.md
│   ├── package.json
│   └── scripts/
│       └── balanceToCoin.js
│
├── .gitignore
├── package.json
└── README.md
```

---

## Features

- **Hero Section** — Profile photo, name, course, school, social links
- **About & Skills** — Bio and skills list loaded from the blockchain
- **Move Smart Contracts** — Educational section about Sui and Move
- **Footer** — On-chain object verification link, DEVCON & Sui branding

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React + TypeScript + Vite |
| Styling | CSS + Tailwind CSS |
| Blockchain | Sui Mainnet |
| Smart Contract | Move Language |
| Hosting | Vercel |

---

## Resources

| Resource | Link |
|---|---|
| Sui Official Docs | https://docs.sui.io |
| Sui Testnet Faucet | https://faucet.sui.io |
| GitHub | https://github.com |
| Vercel | https://vercel.com |

---

## Welcome

Welcome to the **DEVCON x Sui Move Smart Contracts Code Camp!**

In this hands-on session, you will build and deploy your own decentralized portfolio application on the **Sui blockchain**. By the end of the workshop, you will have a live website that reads data from a smart contract you wrote and deployed yourself.

No prior blockchain experience is required. Just bring your laptop and follow along step by step.

---

## What is Sui?

Sui is a high-performance Layer 1 blockchain built by Mysten Labs — the team behind Meta's Diem project. It uses an **object-centric model** that allows transactions to run in parallel, making it extremely fast and scalable. Move is its smart contract language, designed for safety and clarity.

---

## Pre-Installation Notice (Partner Schools)

If you are attending at a **partner school workstation**, the tools below are already installed. Skip to **Step 2: Create Your Accounts** and proceed from there.

---

## Step 1 — Install Required Tools

> ⚠️ **This is the only step that uses a separate Administrator PowerShell window.** After Step 1 is complete, close that window. All commands from Step 2 onwards are run in the **VS Code terminal**.

### 1A. Open PowerShell as Administrator

Click Start → search **PowerShell** → right-click → **Run as Administrator**

### 1B. Install Chocolatey

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

Wait for it to finish.

### 1C. Install Node.js, Git, and Sui

```powershell
choco install nodejs-lts git sui -y
```

Wait for all three to finish installing.

### 1D. Verify installation

Close and reopen PowerShell, then run:

```powershell
node --version
git --version
sui --version
```

All three should print a version number. If any says "not recognized", close and reopen PowerShell. If the issue persists, restart your computer.

> ✅ **Step 1 complete.** Close this Administrator PowerShell window. Open VS Code and use its built-in terminal (**Terminal → New Terminal**) for all remaining steps.

---

## Step 2 — Create Your Accounts

> 🖥️ **No terminal commands in this step.**

You need two accounts for this workshop:

| Account | Purpose | Link |
|---|---|---|
| **GitHub** | Store your project files | https://github.com |
| **Vercel** | Host your live website | https://vercel.com |

**Action:** Create or sign in to GitHub first. Then go to Vercel and sign in **using your GitHub account** — this connects them automatically.

---

## Step 3 — Configure the Sui Client

> 🖥️ **Run these commands in the VS Code terminal** (`Terminal → New Terminal`).

This sets up your blockchain wallet on your computer. Run:

```powershell
sui client addresses
```

Follow the prompts:

| Prompt | What to do |
|---|---|
| Connect to a Sui Full node? | Type `y` then press Enter |
| Sui Full node URL | Press **Enter** to accept the default (Testnet) |
| Key scheme | Type `0` and press Enter (selects ed25519) |

After setup, the terminal will show a **Secret Recovery Phrase** and a **Wallet Address**.

> ⚠️ **Save both in Notepad immediately.** Your recovery phrase cannot be recovered if lost.

> 💡 The key you create here is a plain **ed25519** key (that's what option `0` means). Remember this — the gas-coin step in Step 5 uses this same ed25519 key.

---

## Step 4 — Fork and Clone the Repository

> 🖥️ **Clone command is run in the VS Code terminal.**

### Fork on GitHub

1. Go to: https://github.com/ldcasilang/DEVCONSUI_MoveCodeCamp2026_Level1
2. Click **Fork** (top-right corner)
3. In **Repository name**, add your surname at the end:
   ```
   DEVCONSUI_MoveCodeCamp2026_Level1_YourSurname
   ```
4. Click **Create fork**

### Clone to Your Computer

In the **VS Code terminal**, run:

```powershell
git clone https://github.com/your-username/DEVCONSUI_MoveCodeCamp2026_Level1_YourSurname.git
```

Navigate into the project folder:

```powershell
cd DEVCONSUI_MoveCodeCamp2026_Level1_YourSurname
```

Open the project in VS Code:

```powershell
code .
```

> This opens the entire project in VS Code. You will do all your editing here.

---

## Step 5 — Publish the Smart Contract to Mainnet

> 🖥️ **Run all commands in this step in the VS Code terminal.**

### Switch to Mainnet

Run this command:

```powershell
sui client switch --env mainnet
```
> **Note:** If you installed Sui via Chocolatey, your system might have pre-configured these settings. If you see a message saying "Environment already exists," you can run this:

```powershell
sui client new-env --alias mainnet --rpc https://fullnode.mainnet.sui.io:443
```

### Get Your Wallet Address

```powershell
sui client active-address
```

Copy the address shown and save it in Notepad.

### Obtain Mainnet SUI Tokens

You need a small amount of real SUI to pay for deployment gas.

1. Go to https://www.qr-code-generator.com/
2. Click the **Text** tab, then paste your wallet address
3. Click **Create QR Code** to generate it
4. Show the QR code to a mentor — they will send you the SUI tokens needed

### Check Your Balance

```powershell
sui client balance
```

Wait until it shows at least **0.05 SUI** before continuing.

### Prepare Your Gas Coin

Since the Sui **v1.72 mainnet upgrade (May 2026)**, SUI sent from a wallet arrives in your **address balance** — a per-address balance, not a classic coin object. `sui client balance` shows it correctly, but the CLI needs an actual `Coin<SUI>` **object** to pay gas with. So before publishing, everyone converts a small slice of that balance into a spendable coin.

First, look at what you have:

```powershell
sui client balance   # total SUI you received
sui client gas        # coin objects available for gas (likely empty right now)
```

If `sui client gas` shows no coin objects, create one:

1. From the repo root, go into the tool folder:
   ```powershell
   cd sui-balance-to-coin-tool
   ```

2. Install its dependencies (first time only):
   ```powershell
   npm install
   ```

3. Convert 0.05 SUI into a coin object:
   ```powershell
   npm run balance:to-coin:minimal -- 50000000
   ```
   > `50000000` is the amount in **MIST** (1 SUI = 1,000,000,000 MIST). 0.05 SUI comfortably covers a publish plus the portfolio call in Step 6 — pass a smaller number like `20000000` (0.02 SUI) if you only want the minimum.

4. Confirm it worked. You should see `Status: success` and a `New coin object created: 0x...` line, then:
   ```powershell
   sui client gas   # a coin object should now be listed
   ```

5. Return to the repo root before continuing:
   ```powershell
   cd ..
   ```

> 🔒 **Safe by design.** This tool only reads the keystore the `sui` CLI already created (you never paste a private key), and it only moves SUI from your address balance back to **your own** address as a coin object. Full details in the [Appendix](#appendix--the-balance-to-coin-tool).

### Build the Smart Contract

Navigate to the contract folder:

```powershell
cd portfolio_contract
```

> ⚠️ **Before building:** If a `Published.toml` file already exists inside the `portfolio_contract` folder (from a previous attempt), delete it first. Running `sui client publish` with a stale `Published.toml` will cause an "already published" error. In VS Code Explorer, right-click `Published.toml` → **Delete**.

Build:

```powershell
sui move build
```

> This is a verification step to confirm the contract compiles. Only run it again if you see an error — `sui client publish` will handle the build automatically when you publish.

If the build hangs for more than 2 minutes, press `Ctrl + C`. If you see a build error mentioning `.move` cache, clean it first, then rebuild:

```powershell
Remove-Item -Recurse -Force "$env:USERPROFILE\.move"
sui move build
```

### Publish to Mainnet

```powershell
sui client publish
```

After it finishes, look through the output for a line that says **PackageID**:

```
PackageID: 0xabc123...
```

> ⚠️ **Save this PackageID in Notepad now.** You will need it in the next step.

---

## Step 6 — Deploy Your Portfolio Object

> 🖥️ **Run all commands in this step in the VS Code terminal.**

This step puts your personal information onto the blockchain as a Sui object.

> ⛽ **Gas coin running low?** `sui client call` pays gas from a coin object too, just like publishing. If `sui client gas` looks empty, repeat the **Prepare Your Gas Coin** step from Step 5 (`cd sui-balance-to-coin-tool` → `npm run balance:to-coin:minimal -- 50000000`), then continue.

### Add Your Profile Photo

1. Find the photo you want to use on your computer (`.jpg`, `.jpeg`, `.png`, or `.webp`)
2. In VS Code, open the **Explorer panel** on the left sidebar
3. Navigate to `portfolio_frontend` → `public`
4. **Drag your photo file** from File Explorer into the `public` folder in VS Code
5. Right-click the file you just added → **Rename** → rename it to `profile.png`

> The app is set up to look for `profile.png`. If you use a different format, still rename it to `profile.png`.

### Deploy Your Personal Portfolio

Run the command below in the **VS Code terminal**. **Replace all the placeholder values** with your own information.

> ⚠️ **PowerShell line continuation:** The backtick `` ` `` at the end of each line must be the **very last character** — no spaces after it. A trailing space will cause an "unexpected token" error.

```powershell
sui client call `
  --function create_portfolio `
  --module portfolio `
  --package <YOUR_PACKAGE_ID> `
  --args `
  "YOUR FULL NAME" `
  "Your Course" `
  "Your School" `
  "Write a short description about yourself." `
  "https://www.linkedin.com/in/your-profile/" `
  "https://github.com/your-username" `
  "Skill 1,Skill 2,Skill 3,Skill 4,Skill 5"
```

**Example (filled in):**

```powershell
sui client call `
  --function create_portfolio `
  --module portfolio `
  --package 0xabc123youpackageid `
  --args `
  "LADY DIANE CASILANG" `
  "BS in Information Technology" `
  "FEU Institute of Technology" `
  "I am a fourth-year IT student and freelance designer." `
  "https://www.linkedin.com/in/ldcasilang/" `
  "https://github.com/ldcasilang" `
  "Graphic Design,UI/UX Design,Project Management,Full Stack Development,Web & App Development"
```

After the command completes, scroll through the output and find the **Created Objects** section. Your **Object ID** is listed there:

```
Created Objects:
 ┌─────────────────────────────────────────────
 │ ObjectID: 0xYOUR_OBJECT_ID_HERE
 ...
```

> ⚠️ **Save this Object ID in Notepad.** This is your portfolio's on-chain address.

### Update the Frontend with Your Object ID

1. In VS Code, open:  
   `portfolio_frontend` → `src` → `constants.ts`

2. Find this line:

   ```ts
   export const MAINNET_PORTFOLIO_ID = "0xa3343391df96e28464499f4c209d51bf209c07392fdeea97bfeee59e7550f020";
   ```

3. Replace the value in quotes with **your own Object ID**:

   ```ts
   export const MAINNET_PORTFOLIO_ID = "0xYOUR_OBJECT_ID_HERE";
   ```

4. Save the file (`Ctrl + S`).

---

## Step 7 — Run the Frontend Locally

> 🖥️ **Run all commands in this step in the VS Code terminal.**

If you are still inside `portfolio_contract` from Step 5, navigate to the frontend folder with:

```powershell
cd ../portfolio_frontend
```

If you are in the project root, run:

```powershell
cd portfolio_frontend
```

Install dependencies:

```powershell
npm install
```

Start the development server:

```powershell
npm run dev
```

Open your browser and go to:

```
http://localhost:5173
```

You should see your portfolio with your name and information loaded from the blockchain.

> Press `Ctrl + C` in the terminal when you are done previewing.

---

## Step 8 — Push Your Changes to GitHub

> 🖥️ **Run all commands in this step in the VS Code terminal.**

Make sure you are in the root project folder. Navigate up if needed:

```powershell
cd ..
```

Check that you are in the right place (should show your repo name):

```powershell
git remote -v
```

Stage all your changes:

```powershell
git add .
```

Commit your changes:

```powershell
git commit -m "Deploy Move Smart Contract Portfolio"
```

Push to GitHub:

```powershell
git push origin main
```

### If Git Asks for Username and Password

Enter your GitHub username. For the password, you need a **Personal Access Token** (GitHub no longer accepts plain passwords):

1. Go to: https://github.com/settings/tokens
2. Click **Generate new token** → **Generate new token (classic)**
3. Token name: `Git-Push-Token`
4. Expiration: Choose a date or select **No expiration**
5. Under **Select scopes**, check **repo** (all options under it)
6. Scroll down and click **Generate token**
7. Copy the token and use it as your password in the terminal

> ⚠️ **Save this token in Notepad** — GitHub only shows it once.

---

## Step 9 — Deploy to Vercel (Live Website)

> 🖥️ **No terminal commands in this step.**

1. Go to https://vercel.com and log in with your GitHub account
2. Click **Add New Project**
3. Find your forked repository and click **Import**
4. Under **Root Directory**, click **Edit** and select `portfolio_frontend` → click **Continue**
5. Rename the project — remove the last word and replace with your surname:
   ```
   devconsui-move-code-camp2026-level1-YourSurname
   ```
6. Under **Build Command**, click **Edit** and type: `npm run build`
7. Click **Deploy**

Wait for it to finish. When done, click **Visit** to see your live site.

### Save Your Live URL

Copy your Vercel URL (looks like `https://your-project.vercel.app`). Then:

1. Go to your **forked GitHub repository**
2. Click the **gear icon** (Settings) on the right side of the About section
3. Paste your Vercel URL into the **Website** field
4. Click **Save changes**

> 💡 **Do not delete your forked repository.** Your GitHub repo is part of your portfolio — it shows your source code, commit history, and links to your live site. Keep it public.

---

## Step 10 — What's Next

Congratulations on completing the workshop!

### Hackathons

Register on these platforms to find future Sui builder opportunities:

- **DeepSurge:** https://deepsurge.xyz/hackathons/49d6ffaf-6b43-4641-9865-1ac22964de09
- **FirstBlood:** http://firstblood.firstmovers.io

### Level 2 — Portfolio with CMS

An advanced version with an on-chain admin dashboard:  
https://github.com/ldcasilang/sui_portfolio_level2

---

## Learning Resources

| Resource | Link |
|---|---|
| Official Sui Install Guide | https://docs.sui.io/guides/developer/getting-started/sui-install |
| Sui Testnet Faucet | https://faucet.sui.io/ |
| Address Balances (SIP-58) | https://github.com/sui-foundation/sips/blob/main/sips/sip-58.md |
| GitHub | https://github.com |
| Vercel | https://vercel.com |
| Sample Repository | https://github.com/ldcasilang/DEVCONSUI_MoveCodeCamp2026_Level1 |

---

## Quick Reference: Command Cheat Sheet

All commands below are run in the **VS Code terminal** (except Step 1 which uses Administrator PowerShell).

| What it does | Command |
|---|---|
| Check Sui version | `sui --version` |
| Check Node version | `node --version` |
| Check Git version | `git --version` |
| View wallet addresses | `sui client addresses` |
| Get active wallet address | `sui client active-address` |
| Check SUI balance (total) | `sui client balance` |
| List gas coin objects | `sui client gas` |
| Switch to mainnet | `sui client switch --env mainnet` |
| Prepare a gas coin (from address balance) | `npm run balance:to-coin:minimal -- 50000000` |
| Build smart contract | `sui move build` |
| Publish smart contract | `sui client publish` |
| Clear Move cache | `Remove-Item -Recurse -Force "$env:USERPROFILE\.move"` |
| Install frontend deps | `npm install` |
| Run frontend locally | `npm run dev` |
| Stage all changes | `git add .` |
| Commit changes | `git commit -m "your message"` |
| Push to GitHub | `git push origin main` |
| Show wallet mnemonic (private key) | `sui keytool export --key-identity <ALIAS>` |
| List all key aliases | `sui keytool list` |

---

## Troubleshooting

**`sui` is not recognized as a command**  
→ Make sure `C:\sui` is added to your PATH (see Step 1C) and that you reopened PowerShell.

**`sui move build` hangs for too long**  
→ Press `Ctrl + C`, then run `sui move build` again.

**Build error about `.move` cache**  
→ Run `Remove-Item -Recurse -Force "$env:USERPROFILE\.move"` then build again.

**`Cannot find gas coin for signer address...`**  
→ Your SUI is in an address balance, not a coin object. Complete (or repeat) the **Prepare Your Gas Coin** step in Step 5: `cd sui-balance-to-coin-tool`, then `npm run balance:to-coin:minimal -- 50000000`, confirm with `sui client gas`, and retry.

**`No usable ed25519 keys found...` when running the balance-to-coin tool**  
→ Complete Step 3 (`sui client addresses`) first, and make sure you chose key scheme `0` (ed25519). If your keystore lives in a non-default location, set `SUI_KEYSTORE_PATH`.

**`npm install` fails**  
→ Make sure you are inside the correct folder. For the frontend, `cd portfolio_frontend` first. For the gas-coin tool, `cd sui-balance-to-coin-tool` first.

**PowerShell says "unexpected token" on the `sui client call` command**  
→ Make sure there is no trailing space after each backtick `` ` ``. It must be the very last character on the line.

**Git asks for password on push**  
→ Use a GitHub Personal Access Token as your password (see Step 8).

**Website shows old data after update**  
→ Re-run `git add .`, `git commit`, `git push`, then trigger a redeploy in Vercel.

**`sui client publish` fails with "already published" or a `Published.toml` error**  
→ A `Published.toml` file was left over from a previous attempt. In VS Code Explorer, find and delete `portfolio_contract/Published.toml`, then run `sui client publish` again.

---

## Appendix — The Balance-to-Coin Tool

`sui-balance-to-coin-tool/` is a small, self-contained helper used in **Step 5** to create a spendable gas coin. Since Sui's **v1.72 mainnet upgrade (May 2026)**, SUI sent from a wallet arrives in your **address balance** rather than as a `Coin<SUI>` object, and the CLI needs a coin object to pay gas — this tool bridges that gap. You don't need to read this section to use it.

### What it does

It builds one small transaction that:

1. Withdraws the amount you specify from your **address balance** (`tx.withdrawal({ amount, type: '0x2::sui::SUI' })`).
2. Redeems that withdrawal into a real `Coin<SUI>` object using the standard Sui framework function `0x2::coin::redeem_funds`.
3. Sends that coin straight back to your own address, so it appears under `sui client gas` and the CLI can use it to pay gas.

It signs using the ed25519 key already stored in your Sui CLI keystore, and talks to the fullnode over **gRPC** (the forward-compatible API — Sui's public JSON-RPC endpoints are being retired in 2026).

### Usage

Run these from inside the tool folder:

```powershell
cd sui-balance-to-coin-tool
npm install                                  # first time only
npm run balance:to-coin:minimal -- <amount_in_mist>
```

`<amount_in_mist>` is in MIST. **1 SUI = 1,000,000,000 MIST.**

| You want as a coin | MIST to pass |
|---|---|
| 0.02 SUI | `20000000` |
| 0.05 SUI | `50000000` |
| 0.1 SUI | `100000000` |

### Optional environment variables

| Variable | Purpose | Default |
|---|---|---|
| `SUI_NETWORK` | `mainnet`, `testnet`, or `devnet` | `mainnet` (correct for this camp) |
| `SUI_ADDRESS` | Pick a specific address if your keystore has several | first ed25519 key |
| `SUI_KEYSTORE_PATH` | Point at a keystore in a non-default location | see below |

Default keystore location:

- **Windows:** `C:\Users\<you>\.sui\sui_config\sui.keystore`
- **macOS / Linux:** `~/.sui/sui_config/sui.keystore`

The keystore is read from an absolute path, so it doesn't matter which folder you run the tool from — but `npm install` and `npm run` must be executed **inside** `sui-balance-to-coin-tool/`, since that's where its dependencies live.

### Assumptions & safety

- Assumes the **default ed25519** key scheme (option `0` in Step 3). secp256k1 / secp256r1 keys are skipped.
- Defaults to **mainnet**, which matches where this workshop deploys. Only set `SUI_NETWORK=testnet` if you deliberately deployed to testnet.
- It **never** asks for or stores your private key anywhere new — it only reads the keystore file the `sui` CLI itself created.
- It only moves SUI from your address balance back to **your own** address. It never sends funds to anyone else.
