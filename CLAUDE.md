# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A self-contained HTML reference page for a bridge bidding system called "Strong Club with Canapé Style Openings." There is no build system, bundler, or test framework — the entire application lives in `index.html` (HTML + CSS + JS, no external dependencies).

## Architecture

- **Data-driven rendering**: The bidding system is defined as a JS object tree (`const system = [...]`) at the top of the `<script>` block. The renderer (`renderSection`/`renderNode`) converts this data into HTML at load time and injects it into `<div id="app">`. To add or modify bids, edit the data array — not the generated HTML.
- **Node schema**: Each node has `bid` (string or array of strings for combined badges), `desc` (HTML string), optional `alert` (boolean), optional `info` (HTML string for tooltip), optional `children` (array), and optional `flat` (boolean — renders as a non-expandable section header).
- **Sections vs tree nodes**: Top-level entries in `system` render as collapsible sections (`.section-header` + `.section-body`). Their children render as nested `<ul>/<li>` trees inside `.tree` containers.
- **Collapse mechanism**: Toggling adds/removes the `collapsed` class on `<li>` elements (hides child `<ul>`) and `hidden` class on `.section-body` elements. The page starts fully collapsed (`collapseAll()` called on load).
- **Bid styling**: CSS classes like `.bid-1C`, `.bid-2H` etc. color-code by suit. Suit symbols use `::after` pseudo-elements on `.suit-C`, `.suit-D`, `.suit-H`, `.suit-S`.
- **Info tooltips**: Nodes can have an `info` property containing HTML. This renders as a blue "i" circle (`.info`) with a popover (`.info-text`) toggled on click. Only one tooltip is open at a time.
- **Leaf nodes**: Nodes with no children use `.toggle.leaf` (invisible arrow, no click handler).

## Conventions

- To add new bids, add entries to the `system` array (or as `children` of existing nodes). The renderer handles all HTML generation.
- `bid` can be a string (`"1D"`) or array (`["2H","2S","3C"]`) for combined badges sharing the same description.
- Use `\u2013` (en-dash) for ranges in JS strings (e.g., `"11\u201315 HCP"`), `\u2014` (em-dash) for separators. In HTML context use `&ndash;`/`&mdash;`.
- Use the `s()` helper for suit symbols in descriptions: `` `5+${s('H')}` `` renders as `5+♥`.
- Set `alert: true` on artificial/alertable bids.
- Set `flat: true` on top-level entries that have no response tree (e.g., weak openings).

## Development

Open `index.html` directly in a browser. No server or build step required.
