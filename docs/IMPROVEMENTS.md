# QRDX: what to add next

Written November 17, 2026, after the regenesis on node 3.0.0, the Trade terms gate, the website's Network and What's new pages, and the docs guides for account control, names and quantum migration.

QRDX follows Hyperliquid's product patterns. Where a flow is not specified, do what Hyperliquid does, in QRDX's own wording and design.

Each item says what it is, why it matters and where it goes. Status is one of **todo**, **in progress** or **done**. Update the status here when an item ships, and add a line to the website's What's new page (`qrdx-website/lib/changelog.ts`).

## What already exists

Checked in the code before writing this list, so nothing below repeats it:

- **Trade:** spot and perps, TWAP and scale orders, triggers, CSV export, referrals, fee tiers, vaults, leaderboard, alerts, trading keys (one-click trading), sub-accounts, portfolio with spot PnL, terms gate.
- **Wallet:** web, extension (EIP-1193, EIP-6963, EIP-712) and iPhone PWA; transaction simulation, site-risk warnings, address book, QRDX names, account control (controllers, own key, delay, guardians, trading keys), sub-accounts and vaults, staking, swap, NFTs.
- **Explorer:** search, address sharing, NFTs, tokens, bridge, governance, validators.
- **Docs:** search, llms.txt, guides for every live feature.

## Across the sites: looking modern and professional

| # | Item | Why | Where | Status |
| --- | --- | --- | --- | --- |
| 1 | **One shared top bar and footer** on qrdx.org, Trade, Explorer, Docs and Wallet: the same brand mark, app switcher, network badge and legal links | Each site has its own navigation today. Hyperliquid reads as one product because its chrome is identical everywhere | A shared component copied into each repo (no shared package exists), or a small package in mono | todo |
| 2 | **Status page**: RPC and API health, block time, finality lag, incident history | Every serious exchange has one; QRDX has none | `qrdx.org/status`, reading the same testnet data as the Network page | todo |
| 3 | **Security page and `/.well-known/security.txt`**: how to report a vulnerability, bug bounty (marked planned until funded), audit status ("pre-audit"), the post-quantum design in brief | Users look for this before they deposit | qrdx-website | todo |
| 4 | **Screenshots of the real apps** on the website's product pages | The pages describe Trade, Wallet and Explorer without showing them. Real screenshots fit the site's rule that real data is the imagery | qrdx-website `public/`, taken with the e2e scripts | todo |
| 5 | **Share images per page**: an explorer address with its balance, a transaction, a Trade market with its price, each docs page | qrdx.org has one generic image; links shared on X and Telegram are how people arrive | `opengraph-image` route handlers in each app | todo |
| 6 | **⌘K command menu** on qrdx.org and in Trade: jump to a market, an address, a docs page, a setting | Explorer and Docs have search; the others do not | qrdx-website, qrdx-trade | todo |
| 7 | **Accessibility and performance pass**: focus rings, contrast on the dark navy theme, skeleton loaders instead of dashes while loading, Lighthouse on every site | Polish people notice without naming it | All sites | todo |
| 8 | **Brand and press kit page**: logos, colours, how to write "QRDX" | Partners and press ask for it | qrdx-website | todo |

## Trade

| # | Item | Why | Where | Status |
| --- | --- | --- | --- | --- |
| 9 | **Testnet faucet** | A new user cannot get testnet QRDX, so they cannot try anything. Hyperliquid's testnet has a faucet | A small server with a funded key and rate limits (one claim per address and IP a day). The sites are static, so it needs its own hosting and key | todo: needs hosting and a funded key |
| 10 | **Onboarding checklist after the terms gate**: connect, get testnet funds, place a first order, turn on one-click trading | Turns a first visit into a first trade | qrdx-trade | todo |
| 11 | **Points or epoch rewards page**, only if QRDX decides on an incentive programme | A Hyperliquid staple; never show one that is not real | qrdx-trade | todo: needs a decision |

## Wallet

| # | Item | Why | Where | Status |
| --- | --- | --- | --- | --- |
| 12 | **One-tap quantum migration**: binds the account's own `0xPQ` address as the classic `0x` address's successor (`ACCOUNT_POLICY add_controller`, sent through the write precompile `0x…0401` and signed with the ECDSA key). Once bound, the post-quantum key turns the ECDSA key off and moves the classic address's QRDX | Every wallet account already has both addresses from the same phrase, so the wallet can do it in one approval | Settings → Quantum safety (`components/wallet/settings/QuantumSafetyPage.tsx`, `src/core/migration.ts`) | built, not committed. The calldata was checked against testnet with `eth_call`. Binding itself is untested: the test account holds no testnet QRDX for the fee |
| 13 | **Multisig signing**: set up a multisig from members' public keys, start a request (send QRDX, controllers, own key, cancel a waiting change), sign as a member, merge copies, submit | The wallet could bind a multisig as a controller but not sign as one | Settings → Multisig (`MultisigPage.tsx`, `src/core/multisig.ts`). Requests travel as text or a file, with no server: at 3 to 6 KB per key and signature they are too large for a QR code | built, not committed. Testnet authenticated the wallet's 2-of-3 signature and refused a corrupted one. No multisig transaction has executed yet: that needs a funded multisig |
| 14 | **Guardian recovery**: accounts you guard, back a new controller or withdraw the backing; a recovery waiting against your own account shown at the top with Cancel | Guardians could be named, but there was no screen to act as one | Settings → Account control | built, not committed |
| 15 | **Bridge in the wallet**: deposit from Ethereum and withdraw back without leaving the wallet | Users bridge today through the explorer and docs | Wallet home → Bridge | blocked: testnet has no bridged assets (`bridge_getAssets` is empty), so neither direction can be built against anything real |
| 16 | **Notifications**: funds received and price alerts (existed); account security (a waiting change, a recovery, a new controller, a key turned off) added. Still to do: order fills, liquidation warnings | A waiting change or recovery is the moment a user must act | `src/core/notify.ts`, `useWalletNotifications.ts` | account security built, not committed. They work only while the wallet is open; alerts while it is closed need a push server |
| 17 | **Phone approves desktop**: pair by QR code so the iPhone app approves what a desktop site asks | Already exists as QRDX Connect (`src/core/connect`, `components/wallet/connect`) | — | done (existed) |

## Explorer

| # | Item | Why | Where | Status |
| --- | --- | --- | --- | --- |
| 18 | **Labels for known accounts**: treasury, precompiles, validators, vaults, bridge | Raw addresses everywhere read as unfinished | qrdx-explorer | todo |
| 19 | **Watchlists with alerts** | Standard on mature explorers | qrdx-explorer | todo |
| 20 | **Verified EVM contract source** | Developers expect it | qrdx-explorer, plus a verification endpoint | todo |

## Docs

| # | Item | Why | Where | Status |
| --- | --- | --- | --- | --- |
| 21 | **"Coming from Hyperliquid or Ethereum"**: the same concepts in QRDX terms, side by side | Most developers arrive from one of the two | qrdx-docs `get-started/` | todo |

## Content to keep current

- **Whitepaper:** qrdx-node has v3.4; qrdx.org serves v3.2. Decide whether v3.4 goes up.
- **Terms:** when they change, change `lastUpdated` on qrdx.org/terms and `TERMS_VERSION` in qrdx-trade `lib/terms.ts` together, so Trade asks again. Get the trading section reviewed by a lawyer before mainnet.
- **Quantum migration on testnet:** no migration registry or canaries are configured (`MIGRATION_REGISTRIES` and `DOOMSDAY_CANARIES` are empty in `qrdx_chainSpec`). The Ethereum path is documented but cannot be used there until they are.
- **Next.js:** the build recommends a security upgrade. Not done yet.
