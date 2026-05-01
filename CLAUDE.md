# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A self-contained HTML reference page for a bridge bidding system called "Strong Club with Canapé Style Openings." There is no build system, bundler, or test framework — the entire application lives in `index.html` (HTML + CSS + JS, no external dependencies).

## Architecture

- **Sections**: Each opening bid (1C, 1D, 1H, 1S, 1NT, 2C, 2D, 2H, 2S, 2NT) has its own collapsible section with a `.section-header` and `.section-body`.
- **Tree nodes**: Bids are nested `<ul>/<li>` lists inside `.tree` containers. Each node has a `.node` div containing a `.toggle` arrow, a `.bid` badge, a `.desc` description, and optionally an `.alert` marker.
- **Collapse mechanism**: Toggling adds/removes the `collapsed` class on `<li>` elements (hides child `<ul>`) and on `.section-body` elements (hides entire sections). All logic is in inline `<script>` at the bottom.
- **Bid styling**: CSS classes like `.bid-1C`, `.bid-2H` etc. color-code by suit. Suit symbols use `::after` pseudo-elements on `.suit-C`, `.suit-D`, `.suit-H`, `.suit-S`.
- **Leaf nodes**: Nodes with no children use `.toggle.leaf` (invisible arrow, no click handler).

## Conventions

- When adding new bids, follow the existing pattern: wrap in `<li>`, use a `.node` div with toggle/bid/desc spans, nest responses in a child `<ul>`.
- Nodes that have children need `onclick="toggleNode(this)"` on the `.node` div; leaf nodes do not.
- Use `&ndash;` for ranges (e.g., "11–15 HCP") and `&mdash;` for separators.
- Mark artificial/alertable bids with `<span class="alert">ALERT</span>`.

## Development

Open `index.html` directly in a browser. No server or build step required.
