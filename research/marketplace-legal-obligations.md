# Legal and food-safety obligations: platform vs. Butcher vs. Courier Service

Research for [#5](https://github.com/denniskasper/ww-app/issues/5) (map [#1](https://github.com/denniskasper/ww-app/issues/1)). Researched 2026-10-30.

> **Not legal advice.** This is a planning map of the obligations that apply, built from primary sources so the MVP spec has the right features. Items marked **⚖️ Lawyer** need a German lawyer (IT/consumer law, food law, or competition law) before launch. Items marked **(unverified)** could not be checked against the official text in this session.

## Setup assumed

- A München PWA marketplace. Each **Butcher** (a Metzgerei) is the legal seller of the **Products** (Weißwurst, Brezn, süßer Senf, Weißwurstfrühstück). The platform intermediates and takes a commission.
- **Delivery** is done by a third-party **Courier Service** whose **Drivers** collect the **Order** at the Butcher. **Pickup** at the Butcher is also possible.
- Products are mostly *non-prepacked* (loose counter goods packed on request). No alcohol.

## Short answer

1. **Food information is the Butcher's legal duty, but the platform has to build it.** For distance selling, allergens and, in Germany, certain additives must be shown **before the Customer completes the purchase**, not just in the shop. The platform's product page is the only place where that can happen ([LMIV Art. 14(2), 44(1)(a)][lmiv-uk]; [§ 5 Abs. 3 LMIDV][lmidv5]; [§ 4 LMIDV][lmidv4]).
2. **The platform carries the marketplace duties.** These are the P2B rules toward Butchers (T&C, 15-day change notice, reasons for suspension, ranking parameters), the online-marketplace consumer information (who the seller is, how Orders are matched), the platform Impressum, DSGVO/TDDDG, and the reduced DSA set for a small platform.
3. **No withdrawal right for fresh Weißwurst**, because they are goods that "schnell verderben können" ([§ 312g Abs. 2 Nr. 2 BGB][bgb312g]). The Customer must still be **told** that there is no withdrawal right **(unverified: Art. 246a § 1 Abs. 3 EGBGB)**.
4. **The cold chain belongs to the Butcher and the Courier Service.** "The cold chain is not to be interrupted," and transport containers must keep food at appropriate temperatures ([852/2004 Annex II Ch. IX(5), Ch. IV(7)][852-uk]). There is **no statutory numeric limit for cooked sausage (Fleischerzeugnisse)** such as Weißwurst. The numbers (+2/+4/+7 °C) apply to minced meat, meat preparations and fresh meat ([Tier-LMHV Anlage 5][tierlmhv5]). The Butcher's own HACCP sets the limit, and the Courier Service needs suitable insulated boxes.
5. **Two biggest legal risks are not in the ticket list.** **⚖️ Lawyer** needed for both:
   - **One Platform Price set by the platform** for independent sellers is likely a hardcore restriction (resale price maintenance). The 2022 EU Vertical Guidelines say an online intermediation service provider may not impose a fixed or minimum sale price for the transactions it intermediates **(VGL para. 194, via [law-firm summary][cms-vgl]; unverified against the Guidelines text)**.
   - **Collecting the Customer's money on the Butcher's behalf** can be a licensable payment service under the ZAG. Use a licensed payment provider's marketplace product instead of handling the funds yourself **(unverified: § 1, § 2 Abs. 1 Nr. 2 ZAG)**.

## Obligations table

| # | Obligation | Source | Who carries it | What it means for the MVP spec |
|---|---|---|---|---|
| **Food information** | | | | |
| 1 | Food information is the responsibility of "the operator under whose name or business name the food is marketed" | [LMIV Art. 8(1)][lmiv-uk] | **Butcher** (seller). Platform as the one publishing it: **⚖️ Lawyer** (platform liability is debated, see [BVL/LZ][lz-plattform]) | Butcher **confirms** the product data per Product in onboarding (a declaration and timestamp). Platform terms make the Butcher warrant its accuracy. |
| 2 | Non-prepacked food: allergens (Art. 9(1)(c), Annex II) are mandatory | [LMIV Art. 44(1)(a)][lmiv-uk]; [§ 4 Abs. 2 LMIDV][lmidv4] | **Butcher** | Allergen field per Product (structured list of the 14 Annex II allergens, e.g. *Senf* in süßer Senf, *Gluten/Weizen* in Brezn, possibly *Sellerie* in Weißwurst). |
| 3 | Distance selling: the Art. 44 particulars (allergens) must be available **before the purchase is concluded**, at no extra cost, and again on delivery. Placing food on the market without them is prohibited. | [LMIV Art. 14(1)–(2)][lmiv-uk]; [§ 5 Abs. 3 LMIDV][lmidv5] | **Butcher** (duty); **platform** (the only place it can be shown) | Allergens shown on the **Product page and the checkout summary**, before the "zahlungspflichtig bestellen" button. Also on a label or slip in the bag at Delivery. |
| 4 | German additive rules for loose goods, e.g. "mit Phosphat" (only for Fleischerzeugnisse), "mit Konservierungsstoff". In distance selling these must be shown before purchase in the same medium as the allergens. | [§ 5 LMZDV][lmzdv5]; [LMIDV guidance (Chemnitz)][chemnitz-zus] | **Butcher** | Optional **additive tags** field per Product, shown next to the allergens. |
| 5 | Mandatory info in German | [§ 2 LMIDV][lmidv4] (unverified wording) | **Butcher**, shown by **platform** | German product data is authoritative. English is a translation only. |
| 6 | If a Product is ever sold **prepacked**, all Art. 9 particulars except best-before must be available before purchase | [LMIV Art. 14(1)][lmiv-uk]; [§ 5 Abs. 1 Nr. 16 LMIDV][lmidv5] | **Butcher** | Out of scope if everything is packed on request. Keep the data model open for a full ingredient list. |
| 7 | Price display: Endpreis incl. VAT. A Grundpreis (price per kg) is needed only when goods are sold by weight, not per Stück/Paar. | PAngV §§ 3–4 (unverified) | **Platform** sets and displays the Platform Price | Sell Weißwurst **per Stück/Paar** to avoid a Grundpreis. Show gross prices, delivery fee and total before ordering. |
| **Consumer contract (distance selling)** | | | | |
| 8 | No withdrawal right for goods "die schnell verderben können" | [§ 312g Abs. 2 Nr. 2 BGB][bgb312g] | **Butcher** (seller) | No withdrawal flow. **Must** tell the Customer before ordering that no withdrawal right applies (unverified: Art. 246a § 1 Abs. 3 Nr. 1 EGBGB). Warranty (Gewährleistung) for spoiled goods still applies, so a "problem with my Order" path is needed. |
| 9 | Pre-contract info, checkout button "zahlungspflichtig bestellen", order confirmation on a durable medium | §§ 312d, 312f, 312i, 312j BGB, Art. 246a EGBGB (unverified wording) | **Butcher** as seller; **platform** builds checkout | Checkout labels the final button "zahlungspflichtig bestellen" (or equivalent). **Order-confirmation e-mail** naming the Butcher as seller, the Products, price, Delivery or Pickup time, the no-withdrawal note, and the terms. |
| 10 | Online-marketplace info: main ranking parameters and their weighting, whether the seller is a trader, who owes what under the contract, and that the Customer has no contractual claims against the platform when it only helps fulfil | [§ 312l BGB][bgb312l]; [Art. 246d § 1 EGBGB][egbgb246d] | **Platform** | A short "So funktioniert die Vermittlung" text at checkout. It says: the Butcher is the seller; Delivery Orders go to the **first Butcher in the Delivery Radius to Accept** (this is the "ranking"); Pickup shows the nearest Butcher; the Courier Service delivers on the Butcher's or platform's behalf (decide whose). |
| 11 | Consumer dispute-resolution info (VSBG § 36, for traders with more than 10 employees). The EU ODR-platform link duty ended in 2025. | VSBG § 36; Reg. 2024/3228 repealing ODR Reg. 524/2013 (unverified) | **Butcher** and **platform** each | One line in the terms and Impressum: "Wir nehmen nicht an Streitbeilegungsverfahren teil" (or the opposite). **⚖️ Lawyer** to confirm wording. |
| **Platform-to-business (P2B, Reg. 2019/1150)** | | | | |
| 12 | Terms toward Butchers in plain language. Changes need at least **15 days'** notice on a durable medium. | [P2B Art. 3(1)–(2)][p2b-uk] | **Platform** | Butcher terms (AGB) versioned. Change notice by e-mail at least 15 days before it takes effect, with a record of acceptance. |
| 13 | Restricting or suspending a Butcher needs a **statement of reasons on a durable medium** | [P2B Art. 4(1)][p2b-uk] | **Platform** | An admin "suspend Butcher" action requires a reason, which is e-mailed to the Butcher. (Different from **Paused**, which the Butcher sets.) |
| 14 | Main ranking parameters in the terms | [P2B Art. 5(1)][p2b-uk] | **Platform** | State in the Butcher terms that Orders are offered to every open Butcher in the Delivery Radius at once and go to whoever Accepts first. |
| 15 | Internal complaint system and mediators. **Small enterprises are exempt.** | [P2B Art. 11(5), 12(7)][p2b-uk] | **Platform** (exempt while small) | Not needed for the MVP. Still useful: a "contact support" channel for Butchers. |
| **Digital Services Act (Reg. 2022/2065)** | | | | |
| 16 | Single points of contact for authorities and users; T&C on content restrictions | [DSA Art. 11, 12, 14(1)][dsa] | **Platform** | A contact e-mail in the Impressum or terms labelled as the DSA contact point. Terms describe what listings are allowed. |
| 17 | Notice-and-action mechanism (hosting service) | [DSA Art. 16(1)][dsa] | **Platform** | A "report a problem with this Product/listing" link (a simple form is enough). |
| 18 | Transparency reports, Art. 20–28 duties, trader traceability (Art. 30) and compliance-by-design (Art. 31) | [DSA Art. 15(2), 19(1), 29(1)][dsa] | **Platform**. **Exempt while it is a micro/small enterprise** (and for 12 months after losing that status) | Not required for the MVP. Butcher onboarding still collects name, address, contact, register number and USt-IdNr. for P2B, Impressum and consumer info (overlaps with Art. 30). |
| **Impressum, privacy, accessibility** | | | | |
| 19 | Impressum: name, address, legal form, representatives, e-mail, register number, USt-IdNr. | [§ 5 DDG][ddg5] | **Platform** for the app; **each Butcher** for its seller presence | Platform Impressum page. A **seller details** section per Butcher (shown on the Product/checkout page and in the Order confirmation). |
| 20 | DSGVO: privacy notice (Art. 13), legal basis, processing contracts or joint-control agreements with each party that gets Customer data, deletion periods | DSGVO Art. 6, 13, 26, 28 (unverified wording) | **Platform** (controller of the app); **Butcher** and **Courier Service** for their own use | Privacy policy. A data-sharing setup: the Butcher sees name and Order, the Courier Service gets name, address, phone and Order ID only. **⚖️ Lawyer** decides whether the Courier Service is a processor (Art. 28) or a separate controller. |
| 21 | Storing or reading anything on the device needs consent unless "unbedingt erforderlich" for the service the user asked for | [§ 25 TDDDG][tdddg25] | **Platform** | No analytics or marketing cookies without consent. The PWA service worker, cart and session storage are "unbedingt erforderlich". If analytics are used, a consent banner is needed (or use cookieless analytics). |
| 22 | Accessibility of e-commerce services (BFSG, since 28 June 2025). Micro-enterprises providing services are exempt. | BFSG (unverified) | **Platform** | Likely exempt at MVP size. Build to WCAG 2.1 AA anyway. |
| **Food hygiene and cold chain** | | | | |
| 23 | Every food business operator registers each establishment with the competent authority | [852/2004 Art. 6(2)][852-uk]; [BVL/Bayernportal][bayernportal] | **Butcher** (already registered as a Metzgerei); **Courier Service** if it transports food as a business; **platform**: unclear | Onboarding asks the Butcher to confirm registration or approval. Whether a platform that never touches the food is a *Lebensmittelunternehmer* needing registration is unsettled. **⚖️ Lawyer / ask the München Lebensmittelüberwachung (KVR)**. |
| 24 | Retail meat products: Butchers supplying final consumers are mostly outside the 853/2004 approval regime, which "shall not apply to retail" | [853/2004 Art. 1(5)][853-uk] | **Butcher** | No action for the platform. Butchers who supply *other establishments* fall under 853. Customers are final consumers, so this is fine. |
| 25 | Cold chain must not be interrupted. Transport containers must keep food at appropriate temperatures that can be monitored. | [852/2004 Annex II Ch. IX(5), Ch. IV(7)][852-uk] | **Butcher** until handover; **Courier Service** during transport | Courier Service contract requires insulated or cooled boxes, used only for food. **Maximum transport time** from Accept/handover to Delivery (e.g. ≤ 30–45 min). |
| 26 | Numeric chill limits: minced meat +2 °C, meat preparations +4 °C, fresh meat +7 °C (offal +3 °C), also during transport. **None for Fleischerzeugnisse** (Weißwurst is a cooked Brühwurst). | [Tier-LMHV Anlage 5][tierlmhv5]; [853/2004 Annex III][853-uk] | **Butcher** (sets the limit in its HACCP, typically ≤ +7 °C for Brühwurst) | Keep raw meat (e.g. Hackfleisch) out of scope to avoid the +2/+4 °C transport duty. **Decide: deliver Weißwurst chilled (to heat at home) or hot (ready to eat).** Hot delivery means hot-holding, with no statutory number; common practice is ≥ 65 °C (DIN 10508, unverified). |
| 27 | Pickup | — | **Butcher** (normal counter sale) | Product data rules (rows 2–4) still apply because the Order was placed remotely. |
| **Competition, payments, trade** | | | | |
| 28 | Fixed or minimum resale price imposed by the platform on independent sellers = likely hardcore restriction | Art. 101 TFEU / § 1 GWB; VBER 2022/720 Art. 4(a); [VGL 2022 paras 193–194 (summary)][cms-vgl] | **Platform** (and Butchers that agree) | **⚖️ Lawyer before building the Platform Price.** Options to check: platform as genuine agent; a **maximum** price instead of a fixed one; or the platform as the buyer and reseller (which would make it the seller). This may change the map's "Butcher is the legal seller + one Platform Price" decision. |
| 29 | Collecting payments for third parties may be a licensable payment service | ZAG §§ 1, 2(1) Nr. 2, 10 (unverified) | **Platform** | Use a licensed PSP's marketplace or "connected accounts" product (split payment and payout to the Butcher, platform commission deducted). Payment *reservation* until Accept fits card authorisation and capture. |
| 30 | Trade registration (Gewerbeanmeldung) for the platform | GewO § 14 (unverified) | **Platform** | Admin task, not a spec item. |

## Must-haves for the MVP spec

1. **Product data per Product, owned by the Butcher, displayed by the platform.** Covers German name, the 14-allergen list, additive tags ("mit Phosphat", etc.) and sales unit (Stück/Paar). Shown on the Product page **and** in the checkout summary **before** the order button. Printed on a slip or label in the Delivery bag. Each Butcher confirms the data for its own recipes (recipes differ between Metzgereien, so one shared description does not cover all Butchers). Open question for the spec: one platform-wide Product with per-Butcher allergen variants, or Butchers forced to a common recipe spec?
   - Because the Customer doesn't choose the Butcher for Delivery, the allergens shown before checkout must be the **union across eligible Butchers**, or the Products must be standardised. **⚖️ Lawyer / food-law check.**
2. **Seller transparency at checkout.** Show which Butcher sells (or "assigned on acceptance" plus how the assignment works, row 10). The confirmation e-mail names the Butcher with its address and contact. A Butcher details page serves as its Impressum.
3. **Checkout compliance.** Gross prices, delivery fee and total. A "zahlungspflichtig bestellen" button. A no-withdrawal notice for perishable goods. Links to terms and privacy policy. Order confirmation e-mail on a durable medium.
4. **Butcher onboarding and terms (P2B).** Collect legal name, address, register/USt-IdNr., food-business registration and a contact person. Versioned Butcher terms with ranking explained (first to Accept). A 15-day change-notice process. Suspension with an e-mailed reason, kept separate from **Paused**.
5. **Platform legal pages.** Impressum (§ 5 DDG), customer terms, Butcher terms, privacy policy, DSA contact point, and a "report a listing" link.
6. **Privacy by design.** Data minimisation toward Butchers and the Courier Service, a consent banner only if non-essential storage is used, and deletion periods for Order data.
7. **Cold-chain requirements in the Courier Service integration.** Insulated food boxes, a maximum time from pickup at the Butcher to Delivery, and Orders handed over packed and chilled (or hot, per the decision in row 26). Plus a Customer complaint path for spoiled or wrong Orders.
8. **Payments via a licensed PSP's marketplace product.** The platform does not hold Customer funds itself. Authorise at order time, capture on Accept.
9. **Before building: get legal sign-off on the Platform Price** (row 28). This is the decision most likely to force a change in the map.

## Where a lawyer is needed

- **Competition law:** a single platform-set Platform Price for independent seller-Butchers (row 28).
- **Payments / ZAG:** the money flow and whether a PSP product fully covers it (row 29).
- **Food law:** platform liability for product data it publishes, how allergens should be shown when the Butcher is unknown at checkout, and whether the platform must register as a food business (rows 1, 23, must-have 1). Also ask the München Lebensmittelüberwachung (Kreisverwaltungsreferat) directly.
- **IT / consumer law:** customer and Butcher terms, checkout wording, the Courier Service's DSGVO role, and dispute-resolution wording.

## Sources

Primary texts read in this session:

- [LMIDV § 4][lmidv4], [§ 5][lmidv5]: gesetze-im-internet.de
- [Tier-LMHV Anlage 5][tierlmhv5]: gesetze-im-internet.de (Anlage 4 covers only game)
- [BGB § 312g][bgb312g], [§ 312l][bgb312l], [EGBGB Art. 246d § 1][egbgb246d]: gesetze-im-internet.de
- [DDG § 5][ddg5], [TDDDG § 25][tdddg25]: gesetze-im-internet.de
- [Reg. 1169/2011 (LMIV) Art. 8, 14, 44][lmiv-uk]: via legislation.gov.uk because EUR-Lex was unreachable. Art. 14 and 44(1)(a) quoted wording matches the EU text. The UK edition amends Art. 8(1) and 44(1)(b)–(c), so check those on EUR-Lex.
- [Reg. 2019/1150 (P2B)][p2b-uk], [Reg. 852/2004][852-uk], [Reg. 853/2004][853-uk]: via legislation.gov.uk (UK-revised editions; the cited points carry no UK amendment notes)
- [Reg. 2022/2065 (DSA)][dsa]: eu-digital-services-act.com (EU text)

Official guidance and secondary sources:

- [Bayernportal: Registrierung Lebensmittelunternehmen][bayernportal]; [BVL G@ZIELT][bvl-gezielt]
- [§ 5 LMZDV][lmzdv5]; [Stadt Chemnitz: Merkblatt Zusatzstoffe (Fernabsatz)][chemnitz-zus]
- [LGL Bayern: LMIV FAQ (StMUV-Vollzugshinweise)][lgl-faq]
- [CMS: 10 key aspects of the revised VBER/VGL][cms-vgl] (secondary, for VGL paras 193–194)
- [Lebensmittel Zeitung: "Die Plattformhaftung muss kommen" (BVL president)][lz-plattform] (paywalled; headline only)

Not verified this session (from background knowledge, confirm before relying on it): Art. 246a EGBGB details, §§ 312d/f/i/j BGB wording, PAngV, VSBG § 36 and the ODR repeal, BFSG, ZAG, GewO, DIN 10508, and the exact VGL paragraph text.

[lmidv4]: https://www.gesetze-im-internet.de/lmidv/__4.html
[lmidv5]: https://www.gesetze-im-internet.de/lmidv/__5.html
[tierlmhv5]: https://www.gesetze-im-internet.de/tier-lmhv/anlage_5.html
[bgb312g]: https://www.gesetze-im-internet.de/bgb/__312g.html
[bgb312l]: https://www.gesetze-im-internet.de/bgb/__312l.html
[egbgb246d]: https://www.gesetze-im-internet.de/bgbeg/art_246d__1.html
[ddg5]: https://www.gesetze-im-internet.de/ddg/__5.html
[tdddg25]: https://www.gesetze-im-internet.de/ttdsg/__25.html
[lmiv-uk]: https://www.legislation.gov.uk/eur/2011/1169/contents
[p2b-uk]: https://www.legislation.gov.uk/eur/2019/1150/contents
[852-uk]: https://www.legislation.gov.uk/eur/2004/852/contents
[853-uk]: https://www.legislation.gov.uk/eur/2004/853/contents
[dsa]: https://www.eu-digital-services-act.com/
[lmzdv5]: https://www.gesetze-im-internet.de/lmzdv/__5.html
[chemnitz-zus]: https://www.Chemnitz.de/chemnitz/media/unsere-stadt/ordnung-und-sicherheit/lebensmittel/merkblatt_zusatzstoffe_2022.pdf
[lgl-faq]: https://www.lgl.bayern.de/lebensmittel/kennzeichnung/lmiv_faq.htm
[bayernportal]: https://www.bayernportal.de/dokumente/leistung/3789038154157
[bvl-gezielt]: https://www.bvl.bund.de/EN/Remit/gezielt_safe_shopping/gezielt_node.html
[cms-vgl]: https://cms.law/en/deu/legal-updates/10-key-aspects-of-the-revised-EU-competition-law-in-the-field-of-distribution-VBER-VGL2
[lz-plattform]: https://www.lebensmittelzeitung.net/handel/nachrichten/Lebensmittelueberwachung-Die-Plattformhaftung-muss-kommen-150807
