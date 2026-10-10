# Card, UPI and Wallet Payment Flows — Research Brief

## Executive summary
This study compares three customer payment experiences, but they should not be treated as three equivalent underlying networks. A conventional card transaction uses card acceptance, acquiring/processing, a card network and the issuer. UPI is India's instant account-to-account payment system built over IMPS infrastructure and operated by NPCI. A digital wallet can be a front-end interface or hold a tokenized card credential; when a wallet uses a tokenized card, the authorization usually continues over the underlying card rail in the supported integration. Specific wallet capabilities vary by geography, issuer, device and provider.

## 1. Card payment lifecycle
1. **Checkout / credential** — Customer presents a physical card or enters a card credential; eligible wallets may supply a tokenized credential instead of the primary card number.
2. **Merchant → acquirer / processor** — The merchant acceptance environment creates the payment request and forwards it through its acquiring/processing path.
3. **Network routing → issuer** — The card network routes the authorization request to the issuing bank.
4. **Issuer decision** — The issuer approves or declines after its checks (such as account status, funds/credit availability, authentication and risk). The response returns through the network/acquirer to merchant.
5. **Capture / clearing** — For an approved transaction, capture and subsequent clearing steps provide final transaction details for processing. Exact timing depends on transaction type and setup.
6. **Settlement / payout / reconciliation** — Funds are settled between relevant financial institutions; merchant payout and fees depend on the merchant-acquirer/provider agreement and payout schedule. Clearing and settlement are distinct.

## 2. UPI payment lifecycle (high-level)
1. **Initiation** — Payer selects a UPI-enabled app and starts a QR, intent, VPA or collect flow supported by the app and merchant.
2. **Authentication and routing** — The payer authorizes as required (commonly using a UPI PIN for bank-account transactions). The PSP/bank participants process the request through the UPI ecosystem/NPCI.
3. **Bank processing** — The remitter/payer bank processes the debit and the beneficiary/payee bank processes the credit according to the transaction path.
4. **Status and confirmation** — The payer app, bank, PSP and merchant system may receive status updates through different channels. A timeout or delayed confirmation is not proof of success or failure by itself.
5. **Reconciliation** — Merchant systems should reconcile authoritative transaction status, bank/provider references, events and order records.

NPCI's published FAQ describes real-time reversal handling for UPI declines and notes that status/credit confirmation can sometimes be delayed. RBI publishes a framework for specified failed transaction types. The operational treatment depends on the exact category, the current applicable rule and the banks involved; support staff should check current official requirements rather than promise one generic deadline.

## 3. Wallet-enabled card payment lifecycle
This flow represents a **tokenized card in a supported wallet integration**, not every product branded “wallet”.
1. User chooses a card provisioned in a supported wallet.
2. The wallet supplies the relevant tokenized payment credential and a transaction cryptogram or other required data, depending on the wallet/network implementation.
3. Merchant/PSP/acquirer sends an authorization request into the underlying card processing path. In supported Apple Pay flows, Apple documents an encrypted payment token and device-specific account number/one-time cryptogram; Google Pay documents tokenized card credentials for supported use cases.
4. Network routes to issuer, issuer approves/declines, and status returns.
5. Capture, clearing, settlement, refunds and disputes follow the underlying payment product's rules; token provisioning and wallet/device support add their own operational failure points.

**Regional limitation:** Wallet feature availability differs by region, card issuer, device and merchant integration. Public product documentation is not proof that a feature is supported for a specific Indian card or merchant.

## 4. Comparison: what should operations teams measure?
| Dimension | Card | UPI | Tokenized card in wallet |
|---|---|---|---|
| Key participants | Merchant, acquirer/processor, network, issuer | Payer PSP/app, NPCI ecosystem, remitter and beneficiary banks, merchant/payee PSP | Wallet/token service plus card participants |
| Approval concept | Issuer authorization over card network | UPI transaction processing and bank status across UPI ecosystem | Token/cryptogram-enabled card authorization; issuer still decides |
| Common unknown-state risk | Timeout/response lost after issuer decision | Delayed bank/beneficiary status; debit versus credit confirmation | Token provisioning, device/credential issue, plus underlying card timeout |
| Reconciliation focus | Auth vs capture vs clearing vs settlement vs payout | Request ID, UPI transaction ID / reference, debit/credit and reversal status | Token/wallet diagnostics plus underlying card payment IDs |
| Refund/reversal distinction | Authorization reversal/void vs captured-payment refund vs dispute | Failed transaction reversal differs from complaint/dispute for a completed transfer | Wallet UI does not replace underlying card refund/dispute process |

## 5. Failure patterns and recommended first response

### A. Card issuer decline
- **What it may mean:** issuer declined the request; this is not automatically an outage.
- **First response:** show an actionable, non-sensitive explanation where available and offer a supported alternative; avoid repeated blind retries.
- **Ops evidence:** transaction reference, response-code family, timestamp, payment type and acquirer/provider.

### B. Timeout or unknown status
- **What it may mean:** the caller did not receive the final response; downstream authorization might still have happened.
- **First response:** query status, reconcile with provider/bank records, use idempotent retry controls, and keep customer/order status pending until authoritative evidence exists.
- **Avoid:** creating a fresh charge immediately simply because the first call timed out.

### C. UPI debit but beneficiary/merchant credit not confirmed
- **First response:** keep the status accurate (pending/failed/unknown as supported), preserve UPI transaction/reference details, check bank/app status and the current applicable NPCI/RBI rule for that transaction category. Escalate where the expected window is exceeded.
- **Avoid:** inferring a successful order payment from a debit-only signal or promising a timeline without verifying transaction type and current policy.

### D. Payment succeeded, merchant order remains unpaid
- **First response:** reconcile the payment ID and order ID; check webhooks/events; repair the order state only after authoritative status is confirmed; handle duplicate events idempotently.
- **Avoid:** trusting only a browser redirect or processing the same event twice.

### E. Refund has been initiated but customer does not see credit
- **First response:** distinguish refund initiation from bank posting; check the provider refund ID and the relevant provider/issuer guidance; provide timestamp and a reference.
- **Avoid:** claiming all refunds complete instantly. Provider terms differ.

### F. Wallet token provision or device error
- **First response:** verify supported issuer/card/device/region and review non-sensitive error categories; escalate through the relevant issuer/wallet/provider channel.
- **Avoid:** requesting or recording full PAN, CVV, OTP or PIN in a support ticket.

## 6. Reversal, refund and chargeback are not the same
- **Authorization reversal/void:** a request to reverse or release an authorization/hold where the payment path supports it. Exact semantics depend on the rail and state.
- **Refund:** merchant/provider-initiated return of funds for a completed/captured payment. Timing can involve merchant, provider, network and issuer posting.
- **UPI failed-transaction reversal:** scheme/bank handling for certain failed or incomplete debit/credit outcomes; use the current rules for the specific category.
- **Chargeback/dispute:** a formal dispute process with separate reason codes, evidence requirements and timelines. It is not just another name for refund.

## 7. Operational tracker fields and KPIs
Suggested fields: order ID; payment ID; rail/method; provider; issuer/acquirer/bank where available; transaction amount/currency; request timestamp and timezone; current status; response code/category; idempotency key; capture/refund ID; bank reference/RRN/UTR where applicable; last event timestamp; SLA owner; escalation date; final resolution. Do not store sensitive authentication secrets.

Useful monitoring views: authorization success/decline rate by code; technical decline and timeout rate; unknown/pending state aging; reversal rate and aging; refund turnaround by provider/status; duplicate-charge rate; settlement reconciliation breaks; payout variance; uptime/incident duration; time to first partner response and time to resolution. Definitions must be agreed and denominators made explicit before reporting these metrics.

## 8. Limits and next steps
This is a high-level learning model, not payment-system certification or legal advice. Before operational use, validate each flow with the contracted PSP, issuer/acquirer and scheme/bank documentation for the exact country, payment type, product version, failure code and current rulebook. Use public sources in the source register and attach capture dates to evidence.
