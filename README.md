# Ethereum Consensus Explorer

An interactive block explorer for Ethereum's **consensus layer** (the Beacon Chain). Enter a slot number, or view the chain head, and inspect the block: its proposer, state and parent roots, attestations, and finality status, fetched live from a public Beacon Node API.

Built from scratch in vanilla JavaScript, no frameworks.


[Live demo](https:aarishnaiyer.github.io/Ethereum-Consensus-Explorer/)**


[Ethereum Consensus Explorer showing a finalized beacon-chain block with 128 attestations](screenshot.png)

## What it does

- **Live beacon data:** pulls real blocks from the Ethereum Beacon Chain in real time via the standard [Beacon Node API](https://ethereum.github.io/beacon-APIs/).
- **Slot lookup:** search any slot by number (head or historical), or load the chain head by default.
- **Block detail:** displays slot, proposer index, parent root, state root, and the block's attestations with their committee indices.
- **Finality status:** shows whether a block is finalized or still pending, reflecting the finality lag between the chain head and the last finalized checkpoint.
- **Graceful errors:** skipped or missing slots (which have no block) are handled without crashing.

## How it works

The app is a single HTML file with plain JavaScript. On load, and on each search, it:

1. Reads the requested slot from the input (defaulting to `head`).
2. Fetches the block from `/eth/v2/beacon/blocks/{slot}` on a public Beacon Node API.
3. Parses the response and renders the block detail and attestations directly to the DOM.

No build step, no dependencies. The data source is a public consensus-layer endpoint, so no node or API key is required to run it.

## Running locally

Because the app makes network requests, serve it over HTTP rather than opening the file directly (the `file://` protocol blocks `fetch`):

```bash
# from the project folder
python3 -m http.server 8000
```

Then open `http://localhost:8000` in your browser.

## Tech

- Vanilla JavaScript (`fetch`, `async`/`await`, DOM rendering)
- HTML / CSS
- [Ethereum Beacon Node API](https://ethereum.github.io/beacon-APIs/)

## Roadmap

This is an active project. Planned additions, in rough order:

- [ ] Deploy a live version
- [ ] Full finality detail via the finality-checkpoints endpoint (justified vs. finalized)
- [ ] Light-client data view: sync committee and Merkle-proof inspection
- [ ] Rust backend (Axum/Tokio) for data ingestion and caching
- [ ] React + TypeScript frontend

## Context

I'm an Ethereum Foundation Protocol Fellow working on trust-minimized checkpoint sync for Ethereum's consensus layer (light client protocol and Merkle proofs, in Rust). This explorer is a tool for visualizing and interacting with the consensus-layer data that work is built on.
