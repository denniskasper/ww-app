# Does the platform need a ZAG licence for Stripe Connect marketplace payments?

Research for [#12](https://github.com/denniskasper/ww-app/issues/12), following [#4](https://github.com/denniskasper/ww-app/issues/4). Sources checked 2026-10-30.

**Not legal advice.** This note reads the statute, BaFin's published administrative practice and Stripe's documentation. Before launch, a German payments lawyer should confirm the conclusion, and it can be confirmed bindingly through a BaFin inquiry (see [What to do](#what-to-do)).

## Question

The Butcher is the legal seller. The Customer pays through Stripe Connect **destination charges** with `on_behalf_of` set to the Butcher's connected account. The platform takes `application_fee_amount`, and capture is manual at Accept. Customer money never touches a bank account of the platform. Does the platform need a payment institution licence under § 10 ZAG, or can it rely on an exemption such as the commercial-agent exception? What must the setup look like to stay outside licensing?

## Answer

**Probably no licence is needed, but the commercial-agent exception is not the reason.** The platform stays outside the ZAG because it **does not provide a payment service at all**. Stripe, a licensed e-money institution, acquires the card payment for the Butcher, and the platform never has the funds in its possession. BaFin states this route explicitly. A platform operator does not carry on the licensable Finanztransfergeschäft "wenn er sich eines lizensierten Zahlungsinstituts bedient, das für die Parteien des Kauf-/Dienstleistungsvertrages den Zahlungsfluss abwickelt" [B1]. Two conditions apply:

1. **Each seller has its own contract with the payment institution.** The party using the institution must "jeweils einen eigenen Vertrag mit dem Zahlungsinstitut abschließen …, da sie – und nicht die Plattform – Zahlungsdienstnutzer ist" [B1]. Each Butcher signs Stripe's connected-account agreement during onboarding, so the setup meets this.
2. **The platform has no influence on the payment flow.** The operator "darf demgegenüber keine Einwirkungsmöglichkeit auf den Zahlungsfluss haben, da er ansonsten die Grenzen eines technischen Dienstleisters … überschreitet" [B1]. Technical service providers are exempt only if they "zu keiner Zeit in den Besitz der zu übertragenden Gelder gelangen" (§ 2 Abs. 1 Nr. 9 ZAG) [B2]. Anyone who "an irgendeiner Stelle des Zahlungsvorgangs in den Besitz der Kundengelder [gelangt] oder … die Verfügungsbefugnis über die zu transferierenden Geldbeträge [hat]" loses that exemption [B3].

**Condition 2 is the weak point, and it needs a lawyer's sign-off.** In this design the platform's software decides when to capture (at Accept), whether to cancel the hold (nobody Accepts), how much commission to take, and when to refund. On a destination charge, Stripe books the payment on the platform's account ("sodass die Zahlung im Saldo Ihrer Plattform erscheint") and moves it to the Butcher "sofort und automatisch" [S1][S2]. Refunds and disputes are also debited from the platform's balance [S2]. Stripe's position is that with Connect the platform "keine Kontrolle über die Gelder" has, that the funds sit "auf einem von Stripe verwalteten Bankkonto" [S4], and that platforms using Connect "nehmen keine Zahlungen von Käufer/innen an Verkäufer/innen entgegen" [S3]. Stripe also says each platform must assess its own structure and that its guides are not legal advice [S3][S4][S5]. Stripe's own Germany page warns that a licence "kann erforderlich sein, wenn ein Marktplatz selbst in Zahlungsflüsse eingreift oder Gelder verwaltet" [S5].

My reading is that triggering capture or cancellation of an authorisation Stripe holds, for the Butcher's own sale, is an instruction to Stripe, not control of the funds. The platform has no account where the money could stop, and no way to send it anywhere except the Butcher. This is the mainstream Connect setup across the EEA. Still, "keine Einwirkungsmöglichkeit" is strict wording, and no BaFin text addresses manual capture by a platform. **Treat this as likely, not certain.**

### Why the commercial-agent exception does not help

§ 2 Abs. 1 Nr. 2 ZAG exempts payments through a Handelsvertreter "der aufgrund einer Vereinbarung befugt ist, den Verkauf oder Kauf von Waren oder Dienstleistungen **nur im Namen des Zahlers oder nur im Namen des Zahlungsempfängers** auszuhandeln oder abzuschließen" [Z2]. This platform should not rely on it:

- BaFin says: "wer auf beiden Seiten eines Rechtsgeschäfts steht, wie etwa eine Online-Handelsplattform, kann die Handelsvertreterausnahme nicht in Anspruch nehmen" [B4]. The platform has terms with both the Customer and the Butcher, so it is on both sides.
- BaFin, quoting the legislative reasoning: platforms that "bestimmte Bedingungen für Verträge vorgeben, die über die Plattform zustande kommen, fallen regelmäßig nicht unter die Ausnahmevorschrift", because they "lediglich in automatisierter Weise zum Vertragsschluss bei[tragen]" [B4]. The platform sets the Platform Price and dispatches Orders automatically, which is exactly this case. A Handelsvertreter needs its own scope to decide: "Die entsprechende Befugnis darf jedoch nicht nur auf dem Papier stehen" [B4].
- PSD2 recital 11 allows a two-sided platform the exclusion only "if they do not, at any time enter into possession or control of client funds" [E1]. The EU Commission's answer in EBA Q&A 2020_5355 adds that a B2C platform qualifies only if it acts for the payee "and if it does not also act on behalf of the payer", and leaves that assessment to the national authority [E2]. BaFin's practice is stricter than recital 11 and simply excludes two-sided platforms [B4].
- **PSD3/PSR closes the door further.** The PSR final compromise text (Council 8221/26, 17 April 2026) keeps the exclusion only if the agent acts "on behalf of only the payer or only the payee, but not both of them" and the agreement gives "a real scope to negotiate … or conclude" [E3, Art. 2(2)(b)]. Recital 11 now says the one-side test applies "regardless of whether or not the commercial agent is in the possession of client's funds". Platforms acting for both sides "without … any real scope or autonomy to negotiate" "should not be excluded", and the EBA is to issue guidelines [E3, recital 11]. The "never holds funds" fallback in PSD2 recital 11 is gone. The final texts were published in April 2026, but publication in the Official Journal was still outstanding in the sources checked [E4]. The PSR applies roughly 21 months after entry into force [E4], so it does not bind a 2027 launch, but it removes this exception as a long-term basis. Re-check EUR-Lex before launch.

The EBA itself lists "Concerns that the exclusion is exploited to circumvent requirements of PSD2 (e.g. by e-commerce platforms)" [E5, para. 67]. Build on "no payment service", not on the agent exception.

## What keeps the setup outside licensing

- **Licensed provider settles directly to the Butcher.** Use destination charges with `on_behalf_of` = Butcher (or direct charges). Stripe (STEL, Central Bank of Ireland EMI, ref. C187865, passported into the EEA [S4]) is the provider of the payment service.
- **Every Butcher has its own Stripe contract.** Use Stripe-hosted onboarding, where the Butcher accepts Stripe's connected-account terms [B1].
- **No platform bank account or wallet in the money path.** Customer money must never go to an account in the platform's name, not even for a moment, and the platform never pays Butchers out of its own funds [B3][S3].
- **The platform only gets its commission, as an application fee on the Butcher's own sale.** It does not net, pool or hold back the Butcher's share.
- **The platform's Stripe actions follow fixed, documented rules tied to the Butcher's sale**, set out in the Butcher contract: capture on Accept, cancel if nobody Accepts, refund per policy. This keeps the platform's role as close to "technischer Dienstleister" as the design allows [B1][B2].
- **Don't claim the commercial-agent exception** in terms, contracts or marketing [B4].

## What would trigger a licence (or put the analysis at real risk)

| Change | Why it matters |
|---|---|
| **Separate charges and transfers** where the platform charges first and transfers to the Butcher later (e.g. after delivery, weekly batches, netting refunds) | Stripe: "Die Zahlung auf Ihrem Plattformkonto wird von den Überweisungen auf Ihre verbundenen Konten entkoppelt", and transfers come from the platform's balance [S6]. The platform then decides whether, when and to whom Butcher money moves. That is "Verfügungsbefugnis über die zu transferierenden Geldbeträge" [B3], and it looks like the escrow model BaFin classes as Finanztransfergeschäft ("Treuhandservices … die Kaufpreiszahlung vorab treuhänderisch … entgegennimmt") [B1]. Stripe itself recommends this charge type only if you are liable for connected accounts' negative balances [S6]. |
| **Charges without `on_behalf_of`** | The platform becomes the business responsible for the payment ("Wenn `on_behalf_of` weggelassen wird, ist die Plattform das für die Zahlung verantwortliche Unternehmen") [S2]. This contradicts "the Butcher is the legal seller" and moves the platform towards being the collecting party. |
| **Platform's own account in the flow**: PayPal or bank transfer to the platform, then onward payment | Receiving money "im Namen des Zahlungsempfängers" and making it available to the Butcher is the definition of Finanztransfergeschäft (§ 1 Abs. 1 Satz 2 Nr. 6 ZAG) [Z1]. |
| **Cash on delivery collected by Drivers for the Butcher**, settled via the platform | Same Finanztransfer issue. Keep payment online only. |
| **Wallet, credits or stored balances** for Customers beyond simple vouchers | Can be e-money business (§ 1 Abs. 2 ZAG); a separate analysis would be needed. |
| **Platform pays Butchers before the Customer's money arrives** (pre-funding) | Moves the platform into the payment flow, and possibly into credit. |

Running payment services without a licence is a criminal offence: "Mit Freiheitsstrafe bis zu fünf Jahren oder mit Geldstrafe wird bestraft, wer … ohne Erlaubnis nach § 10 Absatz 1 Satz 1 … Zahlungsdienste erbringt" (§ 63 Abs. 1 Nr. 4 ZAG) [Z3]. This is why a lawyer's sign-off is worth paying for.

## Design issue found along the way: the Butcher is unknown at checkout

The decision in #4 assumes `on_behalf_of` = the Butcher. But Orders are dispatched to all Butchers in the Delivery Radius, and the Butcher is known only at **Accept**, after the Customer's payment is authorised. In Stripe's API reference, `on_behalf_of` is a parameter of *Create a PaymentIntent* [S7] but not of *Update* or *Capture* [S8][S9]. `transfer_data` can be passed on update and on capture [S8][S9]. I could not read from the reference whether `transfer_data.destination` can be changed on an already-authorised intent.

This matters for licensing. The obvious workaround is to authorise on the platform account and decide the Butcher at Accept. That makes the platform the business responsible for the payment [S2] and drifts towards the "platform decides where the money goes" pattern above. Options for the payments design (out of scope here, to settle with Stripe):

- Confirm with Stripe whether `on_behalf_of`/destination can be set after authorisation for this use case, or what Stripe recommends for "seller chosen after authorisation".
- **Pickup**, where the Customer chooses the Butcher, has no such problem.
- Re-authorise under the accepting Butcher (with a saved payment method or a second confirmation). This adds friction and may need SCA again.

## What to do

1. **Keep Stripe Connect with destination charges and `on_behalf_of`; reject separate charges and transfers** for the MVP.
2. **Fix the "Butcher known only at Accept" gap** with Stripe before building (previous section). Whichever fix is chosen must keep the platform out of the money path.
3. **Get a short written opinion from a German payments/ZAG lawyer** on the exact flow, especially manual capture/cancel by the platform, the dispatch model and refunds against the "keine Einwirkungsmöglichkeit" condition [B1]. If they are unsure, ask BaFin through its general inquiry process for a view on whether the planned activity requires authorisation. Do this before taking real Customer payments.
4. **Don't rely on the commercial-agent exception**, now or after PSD3/PSR [B4][E3].

## Sources

- [Z1] ZAG § 1: https://www.gesetze-im-internet.de/zag_2018/__1.html (Abs. 1 Satz 1, Satz 2 Nr. 3 and Nr. 6)
- [Z2] ZAG § 2: https://www.gesetze-im-internet.de/zag_2018/__2.html (Abs. 1 Nr. 2, Nr. 9); § 10: https://www.gesetze-im-internet.de/zag_2018/__10.html ("Wer im Inland gewerbsmäßig … Zahlungsdienste erbringen will … bedarf der schriftlichen oder elektronischen Erlaubnis der Bundesanstalt.")
- [Z3] ZAG § 63: https://www.gesetze-im-internet.de/zag_2018/__63.html
- [B1] BaFin, Merkblatt – Hinweise zum ZAG (Stand: Juli 2024, geändert am 05.07.2024), section B.V Finanztransfergeschäft: https://www.bafin.de/SharedDocs/Veroeffentlichungen/DE/Merkblatt/mb_111222_zag.html
- [B2] Same Merkblatt, section C.IX Technische Infrastrukturdienstleistungen (§ 2 Abs. 1 Nr. 9 ZAG)
- [B3] Same Merkblatt, section B.IV/B.VI on network operators and technical providers ("in den Besitz der Kundengelder … Verfügungsbefugnis")
- [B4] Same Merkblatt, section C.II Handelsvertreter und Zentralregulierer (§ 2 Abs. 1 Nr. 2 ZAG)
- [E1] PSD2, Directive (EU) 2015/2366, Art. 3(b) and recital 11. Read via https://www.legislation.gov.uk/eudr/2015/2366/article/3 and https://www.legislation.gov.uk/eudr/2015/2366/introduction (EUR-Lex returned empty responses)
- [E2] EBA Single Rulebook Q&A 2020_5355, "The implementation of commercial agent exclusion for B2C e-commerce platforms" (answer by the European Commission, published 06/12/2021): https://www.eba.europa.eu/single-rule-book-qa/qna/view/publicId/2020_5355
- [E3] Council of the EU, PSR final compromise text, ST 8221/26 (17 April 2026), recital 11 and Art. 2(2)(b): https://data.consilium.europa.eu/doc/document/ST-8221-2026-INIT/en/pdf
- [E4] Arthur Cox, "PSD3 and PSR: Final compromise texts published" (30 April 2026): https://www.arthurcox.com/insights/psd3-and-psr-final-compromise-texts-published/ (secondary; used only for procedure and timeline). OJ publication not confirmed as of 2026-10-30.
- [E5] EBA Opinion on the review of PSD2, EBA/Op/2022/06 (23 June 2022), paras. 67–68: https://www.eba.europa.eu/sites/default/files/document_library/Publications/Opinions/2022/Opinion%20od%20PSD2%20review%20%28EBA-Op-2022-06%29/1036016/EBA%27s%20response%20to%20the%20Call%20for%20advice%20on%20the%20review%20of%20PSD2.pdf
- [S1] Stripe Docs, Connect charge types: https://docs.stripe.com/connect/charges
- [S2] Stripe Docs, Destination charges: https://docs.stripe.com/connect/destination-charges?platform=web&ui=stripe-hosted
- [S3] Stripe, "How PSD2 impacts marketplaces and platforms" (updated 14 Sept 2021): https://stripe.com/guides/how-psd2-impacts-marketplaces-and-platforms
- [S4] Stripe, "Frequently asked questions about Stripe Connect and PSD2" (updated 18 Jan 2024): https://stripe.com/guides/frequently-asked-questions-about-stripe-connect-and-psd2
- [S5] Stripe, "Payment processing for marketplaces in Germany" (updated 18 May 2026): https://stripe.com/de/resources/more/payment-processing-for-marketplaces-in-germany
- [S6] Stripe Docs, Separate charges and transfers: https://docs.stripe.com/connect/separate-charges-and-transfers
- [S7] Stripe API, Create a PaymentIntent: https://docs.stripe.com/api/payment_intents/create
- [S8] Stripe API, Update a PaymentIntent: https://docs.stripe.com/api/payment_intents/update
- [S9] Stripe API, Capture a PaymentIntent: https://docs.stripe.com/api/payment_intents/capture
