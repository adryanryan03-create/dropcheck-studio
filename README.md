# DropCheck Studio

A live web app built on the **DropCheck** Intelligent Contract deployed on GenLayer StudioNet. DropCheck reads a project or airdrop page and stores its terms — project name, chain, deadline/launch timing, cost model (free / paid / unclear) and eligibility summary — on-chain, but only after independent GenLayer validators rendered the same page and reproduced the exact same extraction under the strict equivalence principle. Every record the app shows is read directly from the blockchain; there is no backend and no cached or simulated data.

**Live app:** https://adryanryan03-create.github.io/dropcheck-studio/

## The contract

### DropCheck — consensus-verified airdrop / project terms
`check_drop(url)` renders the page, extracts the structured terms with an LLM, and stores the record only on exact validator agreement. Fields that the page does not state are stored as explicit `NOT_STATED` / `unclear` markers — the contract never guesses.

- Contract: `0x54aD23aA37BDdB41a746Ca71230f534beC2A2760`
- Explorer: https://explorer-studio.genlayer.com/address/0x54aD23aA37BDdB41a746Ca71230f534beC2A2760
- Deploy tx: `0x9f87a3ba44dc6b74232f47e5887a89159b925845a1e840132cb8a021d3d989c8`
- Source: https://github.com/adryanryan03-create/dropcheck-genlayer

## How the app works

`index.html` is a dependency-light static page. It uses the official `genlayer-js` client (loaded from a CDN) configured for StudioNet and calls the contract's read methods (`get_drop_count` / `get_drop`) directly from the visitor's browser. Seeded records currently include Hooja (Robinhood Chain, mint December 1st) and BUZZCHAIN (Ethereum), both extracted and stored by validator consensus.

Writes (checking a new drop) are done in GenLayer Studio with the deployed contract; the app links there with step-by-step instructions. Note that strict consensus deliberately rejects pages whose rendered text changes between fetches — several popular project pages were rejected during seeding for exactly that reason, which is the filter working as designed.

## Why it matters

Airdrop hunters and agents waste time and money on drops whose real terms — paid vs free, whitelist vs open, actual deadline — are buried in marketing pages or change silently. DropCheck turns any public project page into a consensus-verified, citable on-chain terms record that anyone can re-check before committing.
