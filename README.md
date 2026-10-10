# Project 2 — Card, UPI and Wallet Payment Flows

## Objective
Explain the high-level lifecycle of card payments, UPI and tokenized card payments through supported wallets, then turn common failure states into a practical operations troubleshooting guide.

## Files
- `index.html`: responsive browser-based guide with flow diagrams and expandable operational sections.
- `docs/payment_flow_analysis.md`: source-backed research note, comparison table and troubleshooting guidance.
- `docs/sources.md`: public source register with direct URLs.
- `diagrams/card_flow.mmd`, `diagrams/upi_flow.mmd`, `diagrams/tokenized_wallet_card_flow.mmd`: editable Mermaid diagrams.
- `assets/*.svg`: standalone SVG versions of the diagrams.
- `data/payment_rail_comparison.csv`: comparison dataset.
- `data/failure_scenarios.csv`: operational failure matrix with response suggestions and references.

## Main findings
- Card authorization, clearing and settlement are distinct steps.
- UPI participants include payer/payee PSPs and remitter/beneficiary banks; a debit without beneficiary credit requires status checking and appropriate reversal/escalation handling.
- A wallet can be a user interface and/or credential store. A tokenized card in a wallet often uses the underlying card network for authorization; it is not automatically a separate payment rail.
- Refund, reversal and chargeback/dispute are distinct concepts.
- Unknown payment status should be reconciled before retrying, to reduce duplicate charges.

## Method
The report paraphrases public materials from NPCI, RBI, Visa, Apple Developer, Google Pay Help, Razorpay docs, and Mastercard. It avoids a universal promised settlement/refund timeline because those details depend on the rail, transaction type, provider, issuer/bank, merchant agreement and current rules. Direct citations are listed in `docs/sources.md`.

## GitHub Pages
This page is plain HTML/CSS/JavaScript and uses local SVG assets, so it requires no build step. To publish it from the portfolio root, enable Pages from `main` and `/ (root)`.

## Interview walk-through
1. Draw the authorization route for a card transaction and distinguish it from clearing/settlement.
2. Explain why a UPI debit alone may not prove merchant credit confirmation.
3. Describe how wallet tokenization affects credential handling without confusing the wallet with the underlying card rail.
4. Walk through the unknown-status runbook and required evidence for provider escalation.
5. Explain how you would measure incident frequency, reversal aging and reconciliation breaks.
