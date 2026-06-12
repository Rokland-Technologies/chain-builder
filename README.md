# Rokland LoRa Antenna Chain Builder

Public testing build of the Rokland LoRa Antenna Chain Builder, served at
**[rokland-technologies.github.io/chain-builder](https://rokland-technologies.github.io/chain-builder/)**.

The chain builder helps customers plan the connector chain (adapter, cable,
pigtail) needed to mate a specific LoRa antenna with a specific Rokland
device. Pick a starting device or antenna, walk the picker through what
mates next, and finish with a kit summary that includes a shareable URL
hash so the chain can be copied between devices.

## Status

**BETA — public testing.** Issues and edge-case bugs are expected; that's
why this build is here. Report anything off via the "Report a bug" link
in the footer (or to dweldy@rokland.com directly).

> **A custom domain at `tools.rokland.com` is planned but not yet live** —
> waiting on access to whoever manages the rokland.com DNS records
> (currently delegated to `searchingscout.com` nameservers, not a
> self-serve provider). Once a CNAME record is in place, this site will
> move; the github.io URL above will keep working via redirect.

## How this repo works

This repo is a **publish target**, not the source of truth. The actual
generator lives in the private Rokland monorepo at
`Tidio/src/render_chain_builder.py`, which embeds the live product
catalog data (`Tidio/data/compat/*.json`) into a single self-contained
HTML file with all CSS, JS, and data inlined.

To update the published page:

1. Regenerate locally: `python Tidio/src/render_chain_builder.py`
2. Copy `Tidio/data/compat/chain-builder.html` to this repo's `index.html`
3. Commit + push — GitHub Pages picks up the change within ~30 seconds

A nightly sync workflow that does this automatically is on the followup
list; for now the publish step is manual.

## Files

| File | Purpose |
|---|---|
| `index.html` | The chain builder itself. Single-file standalone — no fetches, no external dependencies, no backend. Hot-links Shopify CDN images for product photos. |
| `README.md` | This file. |

(A `CNAME` file will be added back here once `tools.rokland.com` DNS is
configured. Until then, the site serves directly at the `github.io` URL.)

## Architecture / privacy notes

- Zero secrets, zero PII, zero analytics, zero cookies.
- All product data is public (Rokland's Shopify storefront catalog).
- Product images are hot-linked from `cdn.shopify.com` — visitor browsers
  load these directly, which Shopify intends and supports for public
  product images.
- Chain state serializes to the URL hash for shareable links; nothing
  is sent to any server.
