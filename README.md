# DropCheck Studio

A live web app built on the **DropCheck** Intelligent Contract deployed on GenLayer StudioNet. DropCheck reads a project or airdrop page and stores its terms — project name, chain, deadline/launch timing, cost model (free / paid / unclear) and eligibility summary — on-chain, but only after independent GenLayer validators rendered the same page and reproduced the exact same extraction under the strict equivalence principle. Every record the app shows is read directly from the blockchain; there is no backend and no cached or simulated data.

**Live app:** https://adryanryan03-create.github.io/dropcheck-studio/

## The contract

### DropCheck — consensus-verified airdrop / project terms
`check_drop(url)` renders the page, extracts the structured terms with an LLM, and stores the record only on exact validator agreement. Fields that the page does not state are stored as explicit `NOT_STATED` / `unclear` markers — the contract never guesses.

**v2 (v1.1 milestone — re-verification & change detection):** the app now reads DropCheck v2. Any record can be re-checked with `recheck_drop(drop_id)`: the contract re-runs the same consensus extraction on the stored URL, compares the five term fields, and appends the outcome to an on-chain verification history (`UNCHANGED` / `CHANGED`, the fields that changed, and the new terms snapshot). Each card shows the record's check count and last status, and the app's Verification history section shows the full history per record.

- Contract (v2): `0xCD24Cbe4188Ed138a67c7e6763B4013c830FFf30`
- Explorer: https://explorer-studio.genlayer.com/address/0xCD24Cbe4188Ed138a67c7e6763B4013c830FFf30
- Deploy tx: `0x700dd35ce3310fa14ca919a08fd7747f7b51607637611c4197ec6ca53404524e`
- Contract (v1, superseded): `0x54aD23aA37BDdB41a746Ca71230f534beC2A2760` (deploy tx `0x9f87a3ba44dc6b74232f47e5887a89159b925845a1e840132cb8a021d3d989c8`)
- Source: https://github.com/adryanryan03-create/dropcheck-genlayer

## How the app works

`index.html` is a dependency-light static page. It uses the official `genlayer-js` client (loaded from a CDN) configured for StudioNet and calls the contract's read methods (`get_drop_count` / `get_drop`, plus `get_history_count` / `get_history_entry` for the verification history) directly from the visitor's browser. Seeded records currently include Hooja (Robinhood Chain, mint December 1st) and BUZZCHAIN (Ethereum), both extracted and stored by validator consensus. The first live re-check already caught a real change on the Hooja record: deadline `December 1st` → `December 1, 2026` and eligibility `NOT_STATED` → `Mint is open to everyone.` (status `CHANGED`, visible in the app's history section).

Writes (checking a new drop) are done in GenLayer Studio with the deployed contract; the app links there with step-by-step instructions. Note that strict consensus deliberately rejects pages whose rendered text changes between fetches — several popular project pages were rejected during seeding for exactly that reason, which is the filter working as designed.

## Why it matters

Airdrop hunters and agents waste time and money on drops whose real terms — paid vs free, whitelist vs open, actual deadline — are buried in marketing pages or change silently. DropCheck turns any public project page into a consensus-verified, citable on-chain terms record that anyone can re-check before committing.
