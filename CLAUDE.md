# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run dev` — start the Vite dev server (http://localhost:5173)
- `npm run build` — production build to `dist/`
- `npm run preview` — serve the production build locally
- `npm run lint` — run ESLint over the repo

There is no test runner configured.

## Architecture

A single-page React 19 + Vite expense/finance tracker. This is a teaching starter (from a Claude Code course), originally shipping as one messy file and being improved incrementally. There is no routing, no backend, and no persistence — transactions are seeded in `useState` and reset on reload.

- `src/main.jsx` — entry point; mounts `<App />` in `StrictMode`.
- `src/App.jsx` — the root component. Owns the shared `transactions` state, the hardcoded `categories` array, and the `addTransaction` handler, then composes the three child components below. No presentational markup of its own beyond the header.
- `src/Summary.jsx` — derives income/expense/balance totals from `transactions` and renders the summary cards.
- `src/TransactionForm.jsx` — the add-transaction form. Owns its own form field state (`description`, `amount`, `type`, `category`); takes `categories` and an `onAdd` callback as props.
- `src/TransactionList.jsx` — the transactions table plus the type/category filter dropdowns. Owns its own `filterType`/`filterCategory` state; takes `transactions` and `categories` as props.
- Styling is plain CSS: `src/index.css` (global) and `src/App.css` (component).

### Things to know

- Transaction `amount` is a **number**. Seed rows use numeric literals, and `TransactionForm` coerces the `<input type="number">` string with `Number(amount)` before adding. `Summary` also wraps totals in `Number(t.amount)` defensively. (This fixes the starter's original string-concatenation bug in the summary totals.)
- `categories` is a hardcoded array in `App.jsx`, passed down to drive both the add-form and filter dropdowns.
- New transactions use `Date.now()` for `id`; seed rows use small integer ids.

## Lint config

ESLint uses the flat-config format (`eslint.config.js`) with the recommended JS, react-hooks, and react-refresh rulesets. `no-unused-vars` ignores identifiers matching `^[A-Z_]` (constants/components).
