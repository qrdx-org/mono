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
| 12 | **One-tap quantum migration**: binds the account's own `0xPQ` address as the classic `0x` address's successor (`ACCOUNT_POLICY add_controller`, sent through the write precompile `0x…0401` and signed with the ECDSA key). Once bound, the post-quantum key turns the ECDSA key off and moves the classic address's QRDX | Every wallet account already has both addresses from the same phrase, so the wallet can do it in one approval | Settings → Quantum safety (`components/wallet/settings/QuantumSafetyPage.tsx`, `src/core/migration.ts`) | done. Run on testnet November 18 from the wallet: bind, classic key off, QRDX moved |
| 13 | **Multisig signing**: set up a multisig from members' public keys, start a request (send QRDX, controllers, own key, cancel a waiting change), sign as a member, merge copies, submit | The wallet could bind a multisig as a controller but not sign as one | Settings → Multisig (`MultisigPage.tsx`, `src/core/multisig.ts`). Requests travel as text or a file, with no server: at 3 to 6 KB per key and signature they are too large for a QR code | done. A 2-of-3 multisig sent QRDX on testnet from the wallet, November 18 |
| 14 | **Guardian recovery**: accounts you guard, back a new controller or withdraw the backing; a recovery waiting against your own account shown at the top with Cancel | Guardians could be named, but there was no screen to act as one | Settings → Account control | done. On testnet, November 18: backing queued a recovery, and the warned owner cancelled one from the wallet. Accounts you guard show by their 20-byte account id, since `account_getControlled` returns ids, not `0xPQ` addresses |
| 15 | **Bridge in the wallet**: deposit from Ethereum and withdraw back without leaving the wallet | Users bridge today through the explorer and docs | Wallet home → Bridge | blocked: testnet has no bridged assets (`bridge_getAssets` is empty), so neither direction can be built against anything real |
| 16 | **Notifications**: funds received and price alerts (existed); account security (a waiting change, a recovery, a new controller, a key turned off) added. Still to do: order fills, liquidation warnings | A waiting change or recovery is the moment a user must act | `src/core/notify.ts`, `useWalletNotifications.ts` | account security done. They work only while the wallet is open; alerts while it is closed need a push server |
| 17 | **Phone approves desktop**: pair by QR code so the iPhone app approves what a desktop site asks | Already exists as QRDX Connect (`src/core/connect`, `components/wallet/connect`) | — | done (existed) |
| 22 | **Bitcoin and Solana**: an address on each from the recovery phrase (the paths other wallets use), balances, history, sending | Users hold more than EVM assets; one phrase should show all of it | `src/core/bitcoin.ts`, `solana.ts`, `other-chains.ts`, `OtherSendModal.tsx` | done, not committed. A SOL transfer and a token transfer (creating the recipient's account) passed devnet simulation, and a devnet send through the wallet confirmed. Bitcoin addresses match BIP-84's vectors and coin selection was checked against mainnet coins; no bitcoin has been sent, since no test account holds any |
| 23 | **More EVM networks and your own**: Linea, Scroll, Unichain, Mantle, Gnosis, Celo, Sonic; any EVM network by RPC; `wallet_addEthereumChain` from sites | Sites ask to add networks; users expect to | `src/core/chains.ts`, `settings/NetworksPage.tsx`, the provider router | done, not committed. Every RPC and token contract checked live; dead endpoints (Ethereum's primary, Polygon's, Sepolia's, Amoy's, every Ankr fallback, all of Fantom's) replaced |
| 24 | **Coin and network logos** | Initials on gradients read as unfinished | `components/wallet/CoinLogo.tsx`, `public/logos` | done, not committed. One colour, both themes; only listed tokens get a logo |
| 25 | **Each network shows what works on it**: Receive offers only addresses the network can use, actions and tabs follow the network, test coins are not priced, QRDX testnet is the default until mainnet launches | Offering the PQ address on Ethereum invites a lost transfer; mainnet as the default opened on "Signing disabled" | Dashboard, Receive, Network selector | done, not committed |
| 26 | **Prompt to protect the classic address** on home | The roadmap's follow-up to item 12 | Dashboard | done, not committed |
| 28 | **One portfolio across every network**, QRDX first: assets grouped across networks, real prices (test networks' coins at the real coin's), QRDX verified assets from the trade registry, unverified tokens folded unless trusted, unified activity | Phantom, MetaMask and OKX all lead with one portfolio | `src/core/portfolio.ts`, `components/wallet/portfolio/` | done, not committed. 21 networks read in about 1.5 s |
| 28a | **Portfolio value history**: the account's holdings replayed backwards through its transactions on every network, priced at each moment (DefiLlama) | A value chart that moves with deposits and swaps, not just price × today's holdings | `src/core/portfolio-history.ts`, `fetchBalanceEvents` in `history.ts` | done, not committed. Transactions read on Ethereum, Optimism, Base, Arbitrum, Polygon, zkSync, Unichain, Celo, Avalanche, Mantle (Blockscout and Routescan, keyless), Bitcoin (latest 50) and Solana (latest 40). BNB, Linea, Scroll, Gnosis and Sonic have no keyless explorer: drawn at today's holdings, and the chart says so. QRDX mainnet needs its node's history when it launches |
| 29 | **Asset pages with market data** | "Actual price information": price, changes, chart, market cap, volume, supply, ATH | `portfolio/AssetPage.tsx`, `src/core/market.ts` | done, not committed |
| 30 | **Trade where it is, or bring it to QRDX** | QRDX-first routing for assets on other networks | `src/core/aggregators.ts` (LI.FI, Jupiter), `src/core/bridge.ts`, `portfolio/TradeSheet.tsx` | done, not committed. Quotes live; LI.FI's transaction estimated successfully from a funded Base account; Jupiter's v0 transaction built and simulated. No swap executed: it would spend real funds. The bridge path is untested: testnet bridges nothing yet |
| 27 | **EVM activity** | Etherscan retired its keyless API, so EVM history was empty | `src/core/history.ts` (Blockscout) | done, not committed. BNB, Avalanche, Linea, Mantle, Gnosis, Scroll and Sonic have no keyless explorer API and show only what the wallet sent |

### What Phantom, OKX and MetaMask have that the wallet does not yet

Checked November 19, 2026. All three lead with a multichain portfolio and in-app swaps through aggregators (Phantom: LI.FI; MetaMask: its own aggregation, 0.875 %; OKX: its DEX API), and Phantom and MetaMask offer perps through Hyperliquid, which QRDX's own exchange covers here.

| # | Item | Seen in | Status |
| --- | --- | --- | --- |
| 31 | **Cross-network swaps in one step** (e.g. USDC on Base to SOL on Solana): LI.FI quotes these already; needs the destination address per network and bridge status tracking | MetaMask, Phantom, OKX | todo |
| 32 | **Token approvals manager**: list and revoke allowances a site holds | OKX, MetaMask (Portfolio), Rabby | todo |
| 33 | **Watch-only accounts**: follow any address's portfolio | OKX, Rabby | todo |
| 34 | **Hardware wallets** (Ledger) for the classic keys | all three | todo |
| 35 | **Transaction scanning on other networks** (simulation exists on QRDX and EVM calls; add a threat feed for drainers and address poisoning) | MetaMask (Blockaid), Phantom | todo |
| 36 | **Staking SOL and ETH** (native or liquid) beside QRDX staking | Phantom, OKX, MetaMask | todo |
| 37 | **Buy and sell with a card or bank** | all three | todo: needs a provider agreement |
| 38 | **A swap fee for QRDX** (LI.FI and Jupiter accept an integrator fee) | all three charge one | todo: needs a decision; none is charged now beyond LI.FI's keyless 0.25 % |
| 39 | **NFTs on other networks** | Phantom, OKX | todo |
| 40 | **MEV-protected sends on Ethereum** (a private RPC) | MetaMask (Smart Transactions), OKX | todo |

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
