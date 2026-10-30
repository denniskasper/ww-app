# Marketplace split payments with reserve-now-charge-later in Germany

Research for [#4](https://github.com/denniskasper/ww-app/issues/4). Sources checked 2026-10-30. Fees are list prices and change; confirm them before signing anything.

## Question

The Butcher is the legal seller and the platform takes a commission. Payment is reserved (authorised) at checkout, captured when a Butcher Accepts, and released if nobody Accepts. Which provider supports this in Germany, and does the authorisation last long enough for Scheduled Orders?

## Answer

**Use Stripe Connect** with destination charges, `on_behalf_of` set to the Butcher's connected account, `application_fee_amount` for the commission, and `capture_method=manual`. Offer cards, Apple Pay, Google Pay, PayPal and Klarna. **Don't offer SEPA Direct Debit at checkout**, because no provider can hold it.

The authorisation window is less of a problem than the ticket suggests. Per the map, Scheduled Orders "go to Butchers as soon as they're placed", so capture happens at **Accept**, not at the morning slot. The hold only has to last from checkout until the Accept time limit runs out, which is minutes or hours. A card hold lasts 7 days, so an Order placed the evening before (or several days ahead) is covered. Two cases fall outside that window:

- A Scheduled Order placed more than about 6 days ahead. Either cap advance booking at 6 days, or use Stripe's extended authorisation (up to ~30 days, card CIT only, +0.08 % fee).
- Capturing at fulfilment instead of at Accept. Don't. Capture at Accept and refund if the Butcher later fails to deliver.

Mollie Connect is the runner-up. In its marketplace model the platform owns the payment, which doesn't fit "the Butcher is the legal seller". Adyen for Platforms fits technically but is built and priced for large platforms with a minimum invoice. PayPal's multiparty product needs partner approval, and its multi-seller flow requires `intent=CAPTURE`.

## Comparison

| | Stripe Connect | Adyen for Platforms | Mollie Connect | PayPal Complete Payments (multiparty) |
|---|---|---|---|---|
| Split to Butcher + commission | Destination charge + `application_fee_amount`, or separate charges and transfers [S1][S2] | `splits` at authorisation or at capture (Commission, BalanceAccount…) [A1][A2] | Marketplace: routes (delayed routing recommended); Platform model: application fees via OAuth [M1][M2] | `platform_fees` on the capture call; seller needs `PARTNER_FEE` feature [P1][P2] |
| Who is the seller / settlement merchant | Destination charge: platform by default; `on_behalf_of` makes the Butcher the settlement merchant (Butcher's descriptor and country fees). Direct charges: Butcher [S1][S2] | Docs don't say "merchant of record". Marketplace brand faces the Customer, funds sit in user balance accounts [A1] | Marketplace: "Your marketplace is the payment owner", platform has first-line chargeback liability. Platform model: merchant owns payment [M2] | Funds settle to the `payee` seller; docs don't say "merchant of record" [P3] |
| Auth then capture | `capture_method=manual` [S3] | Manual capture; split instructions can be sent at capture [A2][A3] | `captureMode=manual` [M3]; docs don't say whether routing combines with manual capture | `intent=AUTHORIZE` + capture [P2]; multi-seller page requires `intent=CAPTURE` [P3] |
| Card hold validity | 7 days online (Visa MIT 4 d 18 h); extended auth up to ~30 days, CIT only, +0.08 % [S3][S4] | Default 28 days for global schemes; per scheme e.g. Visa CIT CNP 10 days, Mastercard 7 (final) / 30 (pre-auth), Amex 7 [A4] | Visa 7, Amex 7, Mastercard 30 days; cannot be extended [M3] | 29-day authorisation, 3-day honor period [P4] |
| PayPal hold | 10 days, auto-extended to 20 [S3] | Via PayPal contract [A5] | 29 days (manual capture in beta) [M3] | 29 days / 3-day honor [P4] |
| Klarna hold | 28 days [S3] | Supported (adjustable) [A4] | 28 days [M3] | — |
| SEPA Direct Debit hold | Not supported ("does not support delayed capture") [S3] | Not among separate-capture methods | Not in manual-capture list [M3] | — |
| Apple Pay / Google Pay | Card rails (unverified on Stripe docs: wallet pages 404'd) | Priced "defined by card used" [A5] | Priced by card used [M4] | Not listed for online |
| Butcher onboarding / KYC | Stripe-hosted onboarding, form generated per country and entity type; web browser only, not in embedded webviews [S5] | Hosted onboarding; DE supports individual, sole proprietorship, organisation [A6] | KYB per merchant; OAuth or co-branded Client Links; each Butcher is its own Mollie organisation [M2] | Partner approval required before live; sellers onboarded through PayPal [P1] |
| Fees (DE, list) | Cards: 1.5 % + €0.25 standard EEA, 2.8 % + €0.25 premium EEA; PayPal 0.2 % + €0.10 + PayPal fees; Klarna 2.99 % + €0.35; SEPA €0.35 [S6][S7]. Connect: €2 per monthly active account + 0.25 % + €0.10 per payout ("you handle pricing") [S8] | $0.13 + interchange++ + 0.60 % (Visa/MC); Klarna DE $0.13 + 2.99 %; no monthly fees but "a minimum invoice depending on industry"; platform fees separate [A5] | Cards 1.80 % + €0.25 (EEA consumer); Klarna DE 2.99 % + €0.35; PayPal fees + €0.10; SEPA €0.35; Connect pricing on request [M4] | PayPal and advanced cards 2.99 % + €0.39; platform pricing not published [P5] |

## Notes per provider

### Stripe Connect

- Stripe calls destination charges and separate charges and transfers "best for marketplaces" [S1]. Destination charges are simpler for one Butcher per Order: the full amount moves to the connected account after capture, and the application fee comes back to the platform [S2].
- Without `on_behalf_of` the platform is the business of record. With it, the Butcher is the settlement merchant, its statement descriptor appears, and its country's fee structure applies [S2]. That matches "the Butcher is the legal seller". Refunds and chargebacks on destination charges still debit the platform balance [S1]. Plan for that in the Butcher contract.
- Methods that support separate authorisation and capture: cards, Affirm, Afterpay, Cash App Pay, Klarna and PayPal. ACH and iDEAL don't. SEPA Direct Debit doesn't support delayed capture, and you can't mix cards and SEPA on one PaymentIntent with manual capture [S3].
- Extended authorisation: Visa, Mastercard, Amex and Discover; customer-initiated only; no regional restriction; 0.08 % extra for most merchant categories [S4]. The docs don't say whether it works with destination charges. Test this before relying on it.
- Under the "Stripe handles pricing" Connect option the platform pays no per-account or payout fees, and Stripe bills Butchers directly [S8].
- Germany field-level KYC lists weren't readable from the docs page (it points to a requirements endpoint) [S9]. The hosted form collects whatever the country and entity type need [S5].

### Adyen for Platforms

- Strong fit technically: splits can be decided at capture, which suits "commission known at Accept" [A2]. If the capture amount differs from the authorisation and no splits are given, all funds go to the platform's liable balance account [A2].
- Commercially aimed at larger platforms: indicative pricing only, minimum invoice, platform products "priced separately" [A5].

### Mollie Connect

- Marketplace model: platform owns the payment and carries chargeback liability; split payments must be requested from Mollie; delayed routing forces gross settlement (monthly fee invoice); unrouted funds sweep to the platform after 90 days [M1][M2].
- Card support for routing isn't shown in the routing table [M1], and the docs don't say whether routing combines with manual capture. Both need confirming with Mollie before choosing it.
- Requires a Mollie partner account [M2].

### PayPal Complete Payments Platform

- Only for approved partners; unapproved calls return 401 [P1].
- Auth-capture with `platform_fees` on capture is documented [P2], but the multi-seller page says `intent` must be `CAPTURE` [P3]. Germany availability isn't stated [P1].
- Better to offer PayPal as a payment method through Stripe (or Mollie) than to build on PayPal's platform product.

## Open points for the map

- Capture at Accept (recommended), or at fulfilment? Capture at fulfilment would push some Scheduled Orders past the 7-day card window.
- Maximum advance booking for Scheduled Orders. 6 days avoids extended authorisation entirely.
- Who carries refunds and chargebacks: the platform is debited first under Stripe destination charges [S1].
- Regulatory: not researched here. Using a licensed provider's marketplace product (any of the four) is the usual way a platform avoids needing its own payment-institution licence under ZAG. Confirm with a lawyer.

## Sources

- [S1] Stripe, Charge types: https://docs.stripe.com/connect/charges
- [S2] Stripe, Destination charges: https://docs.stripe.com/connect/destination-charges
- [S3] Stripe, Place a hold on a payment method: https://docs.stripe.com/payments/place-a-hold-on-a-payment-method
- [S4] Stripe, Extended authorisation: https://docs.stripe.com/payments/extended-authorization
- [S5] Stripe, Hosted onboarding: https://docs.stripe.com/connect/hosted-onboarding
- [S6] Stripe pricing (DE): https://stripe.com/en-de/pricing
- [S7] Stripe local payment methods pricing (DE): https://stripe.com/en-de/pricing/local-payment-methods
- [S8] Stripe Connect pricing (DE): https://stripe.com/en-de/connect/pricing
- [S9] Stripe, Required verification information: https://docs.stripe.com/connect/required-verification-information
- [A1] Adyen, Marketplaces: https://docs.adyen.com/marketplaces
- [A2] Adyen, Split payments at capture: https://docs.adyen.com/marketplaces/split-transactions/split-payments-at-capture
- [A3] Adyen, Capture: https://docs.adyen.com/online-payments/capture
- [A4] Adyen, Pre-authorisation and validity: https://docs.adyen.com/online-payments/adjust-authorisation and https://docs.adyen.com/point-of-sale/pre-authorisation
- [A5] Adyen pricing: https://www.adyen.com/pricing
- [A6] Adyen, Verification requirements / onboarding: https://docs.adyen.com/marketplaces/verification-requirements and https://docs.adyen.com/marketplaces/onboard-users
- [M1] Mollie, Route and split payments: https://docs.mollie.com/docs/connect-route-and-split-payments
- [M2] Mollie, Connect overview / core concepts: https://docs.mollie.com/docs/connect-overview and https://docs.mollie.com/docs/core-concepts
- [M3] Mollie, Place a hold for a payment: https://docs.mollie.com/docs/place-a-hold-for-a-payment
- [M4] Mollie pricing (DE): https://www.mollie.com/de/pricing
- [P1] PayPal multiparty overview / get started: https://developer.paypal.com/docs/multiparty/ and https://developer.paypal.com/platforms/get-started/
- [P2] PayPal, Authorize and capture (multiparty): https://developer.paypal.com/docs/multiparty/checkout/standard/customize/auth-capture/
- [P3] PayPal, Multi-seller payments: https://developer.paypal.com/docs/multiparty/checkout/multiseller-payments/
- [P4] PayPal, Authorization and honor period: https://developer.paypal.com/payment-methods/auth-honor
- [P5] PayPal fees (DE): https://www.paypal.com/de/business/paypal-business-fees
