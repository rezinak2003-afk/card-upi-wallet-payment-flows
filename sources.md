# Source register

Accessed 10 October 2026. Use source links to verify current product and regulatory details before relying on a process decision. This learning guide paraphrases the sources and avoids asserting a universal settlement/fee promise.

1. **NPCI — UPI FAQs**: UPI description; role of UPI PIN; supported flows; what may happen when an account is debited but transaction does not complete; merchant onboarding and settlement context.
   https://www.npci.org.in/what-we-do/upi/faqs
2. **NPCI — UPI Product Booklet (PDF)**: UPI ecosystem participants including payer PSP, payee PSP, remitter bank and beneficiary bank.
   https://www.npci.org.in/PDF/npci/upi/Product-Booklet.pdf
3. **NPCI — UPI Ecosystem Statistics glossary**: distinction between business declines, technical declines, and debit reversal indicators.
   https://www.npci.org.in/what-we-do/upi/upi-ecosystem-statistics
4. **RBI — Harmonisation of Turn Around Time (TAT) and customer compensation for failed transactions using authorised Payment Systems**: operational framework and timelines for specified failed transaction types, including UPI and cards. Check the current applicable circular and transaction category rather than treating one timeline as universal.
   https://www.rbi.org.in/commonman/Upload/English/Notification/PDFs/CIRCULAR677EC931A7A65E4D99AA957D8E85BC0A2A.PDF
5. **Visa — 3-D Secure guide**: authentication and high-level authorization, clearing and settlement stages.
   https://corporate.visa.com/en/solutions/visa-protect/insights/3d-secure.html
6. **Visa — VisaNet Connect Acceptance: How it works**: card/token-based authorization, request routing to issuer, response routing, and settlement/reporting.
   https://developer.visa.com/capabilities/visanet-connect-acceptance
7. **Apple Developer — Apple Pay Planning**: encrypted payment token, device-specific account number/cryptogram, PSP processing, network routing and issuer decision in supported Apple Pay integrations. Availability and merchant integration differ by region.
   https://developer.apple.com/apple-pay/planning/
8. **Google Pay Help — Supported cards and payment modes (India)**: token replacing the actual card number in supported card use cases; supported use and bank eligibility vary.
   https://support.google.com/pay/india/answer/9674221?hl=en
9. **Razorpay Docs — Refund FAQs**: example of a provider-specific refund workflow, including dashboard/API initiation and fee-treatment notes. These details apply to that provider's documented service and must not be generalized to other providers.
   https://razorpay.com/docs/pos/refunds/faqs/?preferred-country=US
10. **Mastercard — Rules for Merchants**: official merchant rules, chargeback guide, and payment processing material. Check current rulebooks and regional applicability.
    https://www.mastercard.us/en-us/business/overview/support/rules.html

## How sources were used
- Card stages are summarized from Visa's published explanations of authorization, clearing and settlement.
- UPI participants and failure states are summarized from NPCI materials. RBI's failed-transaction TAT circular is referenced as a rulebook source; this project does not calculate legal deadlines for a particular case.
- Wallet examples explain **tokenized card payments** only where supported, and explicitly distinguish the wallet experience from underlying card rails. Google Wallet/Apple Pay capabilities vary by country, issuer and device; do not infer that a global wallet feature is available in India just because documentation exists.
- Fee, settlement, uptime and support values are not invented in Project 1; obtain dated partner evidence before scoring actual providers.
