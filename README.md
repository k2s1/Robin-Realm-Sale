# Robin Realm Sale — Live Site

Static bundle for the REALM token sale frontend on **Robinhood Chain** (chain ID 4663).

## Live

- **Sale contract**: [`0x4f55359D197A4ec598aB3e09e8a1bF051aee8037`](https://robinhoodchain.blockscout.com/address/0x4f55359D197A4ec598aB3e09e8a1bF051aee8037)
- **REALM token**: [`0xd82633652947A5B5Ddbc334d15433608D7E61aEd`](https://robinhoodchain.blockscout.com/address/0xd82633652947A5B5Ddbc334d15433608D7E61aEd)
- **Robin Realms NFT**: [`0x624975f2557f5ac891a69d00c97f0c6fe6765051`](https://robinhoodchain.blockscout.com/address/0x624975f2557f5ac891a69d00c97f0c6fe6765051)

## What's in this repo

This is the **production-built static site** (Vite output, ~160KB JS gzip-friendly). Deployed as-is on Vercel — no build step needed.

```
.
├── index.html              entry, loads ethers v6 from CDN
├── assets/
│   ├── index-*.js          React bundle
│   └── index-*.css         Tailwind CSS
├── nft/                    10 Robin Realms preview SVGs
└── vercel.json             caching + security headers
```

## How it's built

Source lives separately in `realm-token/site-react/` (Vite + React + Tailwind). Build with:

```bash
cd realm-token
npm install
cd site-react
npm install
npm run build
# output: site-react/dist/
```

## Why ethers loads from a CDN

ethers v6 + Vite has a known bundling issue (the bundler picks the wrong entry point, so the library never makes it into the bundle → `ReferenceError: ethers is not defined` at runtime). Workaround used here: load `ethers.umd.min.js` as a UMD script in `index.html` so it exposes `window.ethers` globally. The React code does `const { ethers } = window;` instead of `import { ethers } from "ethers"`. Bundle: 432KB → 159KB.

## Sale details

- **Chain**: Robinhood Chain (4663)
- **Buckets**: 240M REALM (NFT, weighted by rarity) + 60M REALM (public, 0.0005 ETH/wallet cap)
- **Price**: 1 gwei / token
- **Window**: see contract `startTime()` / `endTime()`

## Re-deploy

This repo is wired to Vercel — every push to `main` triggers a redeploy.
