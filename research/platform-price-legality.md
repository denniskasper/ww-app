# Can a marketplace set one retail price for independent Butchers?

Research for [#2](https://github.com/denniskasper/ww-app/issues/2), part of the map [#1](https://github.com/denniskasper/ww-app/issues/1).
Date: 2026-10-30. Terms follow [`GLOSSARY.md`](../GLOSSARY.md).

> **This is not legal advice.** It is a reading of primary sources so we can make a product decision. Before launch, a German competition lawyer (Kartellrecht) must review the chosen structure and the contracts. The points that most need a lawyer are marked **⚖ Lawyer**.

## Answer in short

- **No, not as the map currently states it.** If each Butcher is the legal seller and the platform makes every Butcher sell at one Platform Price, that is **resale price maintenance (RPM)**. It is a hardcore restriction under Art. 4(a) of the Vertical Block Exemption Regulation (VBER) and a restriction "by object" under Art. 101(1) TFEU and § 1 GWB. The EU guidance says so explicitly for providers of online intermediation services ([Guidelines §§ 67(c), 194](#sources)). Size does not help: neither the EU nor the German de minimis notice covers price fixing ([Bagatellbekanntmachung Rn. 13–14](#sources)).
- **The "genuine agency" escape route does not fit.** The Commission says that platforms generally do not qualify as agents ([Guidelines § 46](#sources)). The reverse arrangement, with the Butcher as the platform's agent, also fails, because the Butcher makes and owns the goods it sells ([Guidelines § 33(a)](#sources)).
- **What works: the platform is the seller and Butchers are fulfilment partners.** The Customer buys from the platform at the Platform Price, and the platform passes the Order to a Butcher to fulfil. The Commission's Guidelines name this "fulfilment contract" model and describe almost exactly our case. They say that setting the price is **not RPM when the platform, rather than the Customer, chooses the Butcher** ([Guidelines § 193](#sources)). This means **the legal seller changes from the Butcher to the platform.**
- **Residual risks.** (1) **Pickup**, where the Customer chooses the Butcher, falls into the part of § 193 that says imposing a price "may amount to RPM". (2) **Hub-and-spoke**: the platform must not become a channel for Butchers to coordinate with each other ([Guidelines § 44, fn. 103 and 109](#sources); [Eturas](#sources)).

**Recommendation for the map:** change "Selling: marketplace where the Butcher is the legal seller" to **"the platform is the legal seller (merchant of record). Butchers are fulfilment partners paid a per-Order fee; the platform sets the Platform Price and chooses the Butcher."** Keep the platform independent of the Butchers. Get a lawyer's view specifically on Pickup. See [Recommendation](#recommendation-for-the-map).

---

## 1. Which law applies

- **§ 1 GWB** prohibits agreements and concerted practices that have as their object or effect the restriction of competition ([§ 1 GWB](https://www.gesetze-im-internet.de/gwb/__1.html)). It mirrors Art. 101(1) TFEU but, unlike Art. 101, does **not** require an effect on trade between Member States. A München-only service is therefore caught by German law even if Art. 101 TFEU does not reach it.
- **§ 2(2) GWB** applies the EU block exemption regulations, including the VBER 2022/720, "entsprechend". This holds "auch, soweit die dort genannten Vereinbarungen … nicht geeignet sind, den Handel zwischen den Mitgliedstaaten … zu beeinträchtigen" ([§ 2 GWB](https://www.gesetze-im-internet.de/gwb/__2.html)). In practice, the EU vertical rules and Guidelines are the yardstick in Germany too.
- **§ 2(1) GWB / Art. 101(3) TFEU**: an individual exemption is possible if efficiencies, a fair share for consumers, indispensability and no elimination of competition are all shown ([§ 2 GWB](https://www.gesetze-im-internet.de/gwb/__2.html)).
- **No de minimis shelter.** The Bundeskartellamt's de minimis notice "gilt nicht für horizontale oder nicht-horizontale Vereinbarungen, die … Folgendes bezwecken oder bewirken: a) im Hinblick auf Dritte die Festsetzung von Preisen …" ([Bagatellbekanntmachung 2007, Rn. 13–14](https://www.bundeskartellamt.de/SharedDocs/Publikation/DE/Bekanntmachungen/Bekanntmachung-Bagatellbekanntmachung.pdf?__blob=publicationFile&v=2)). The EU De Minimis Notice (2014/C 291/01, point 13) likewise excludes by-object restrictions. *I could not retrieve that text in this session; the reference comes from the Guidelines' cross-references.*

## 2. The structure on the map: Butcher is the legal seller, platform imposes one price

How the VBER classifies the parties:

- A company providing online intermediation services is the **supplier** of those services. A business selling through it (the Butcher) is the **buyer** of those services ([Guidelines § 67](#sources), citing Art. 1(1)(d) VBER).
- Restrictions the platform imposes on its buyers "en ce qui concerne le prix … sont soumises aux dispositions de l'article 4 du règlement (restrictions caractérisées)". *Example given:* the exemption "ne s'applique pas à un accord en vertu duquel un fournisseur de services d'intermédiation en ligne impose un prix de vente fixe ou minimal pour une transaction qu'il facilite" ([Guidelines § 67(c)](#sources)).
- § 194 repeats the point: Art. 4(a) "est pleinement applicable dans l'économie des plateformes en ligne". A platform may encourage sellers to price competitively or to cut prices, but "l'imposition … d'un prix de vente fixe ou minimal pour les transactions réalisées par son intermédiaire constitue une restriction caractérisée" ([Guidelines § 194](#sources)).
- RPM includes contract terms that set the buyer's price directly, and indirect means such as fixing the margin or capping discounts. It also includes threats or termination when a price is not respected ([Guidelines §§ 185–187](#sources)).

**Conclusion:** a Platform Price that every Butcher must charge while it is the seller is a fixed resale price, and it is hardcore. "By object" does not mean automatically illegal, because Art. 101(3) / § 2(1) GWB can still be argued ([Guidelines § 195](#sources)). In that case, however, the platform would have to prove concrete efficiencies and that no less restrictive alternative exists ([Guidelines § 197](#sources)). Because the fulfilment model in §4 is such an alternative, the indispensability argument would be weak. **Not a safe basis for a business.**

## 3. Options and what each means for who the legal seller is

### 3a. Agency: platform as the Butchers' agent → does not work

- Under a genuine agency agreement, which falls outside Art. 101(1), the principal may set the price ([Guidelines § 192](#sources)). This only applies if the agent bears no, or only insignificant, contract-specific risks, market-specific investment risks, or risks from other activities ([Guidelines §§ 31–33](#sources)).
- **Platforms specifically:** "Les accords conclus par des entreprises actives dans l'économie des plateformes en ligne ne remplissent généralement pas les conditions pour être qualifiés de contrats d'agence". The reasons given are that platforms deal with very many sellers, have strong bargaining power, and make significant market-specific investments in software and advertising ([Guidelines § 46](#sources)).
- Even a genuine agency falls under Art. 101(1) where "un certain nombre de commettants font appel aux mêmes agents" and use them to coordinate strategy ([Guidelines § 44](#sources)). Our setup has many Butchers (principals) and one platform (agent) setting one price for all of them, which is that pattern.
- If the agency is not genuine, any restriction on the agent sharing its commission with the Customer is itself hardcore ([Guidelines § 192](#sources)).

**Legal seller:** the Butcher, with the platform as its agent. **Verdict: not viable.**

### 3b. Agency the other way round: Butcher as the platform's agent → does not work

A genuine agent "n'acquiert pas la propriété des biens … et ne fournit pas lui-même les services" and does not hold stock at its own risk ([Guidelines § 33(a), (c)](#sources)). A Butcher makes the Weißwurst, owns the stock and bears the risk of spoilage, so it cannot be a genuine agent. **Verdict: not viable.**

### 3c. Platform as seller, Butchers as fulfilment partners → the lawful route

- **Guidelines § 193 (fulfilment contracts):** a supplier makes a vertical agreement with a buyer to carry out a supply contract that the supplier has already concluded with a specific customer. "Lorsque le fournisseur sélectionne l'entreprise qui fournira les services d'exécution, l'imposition d'un prix de vente par le fournisseur n'est pas une pratique de prix de vente imposés." The Commission's own example: "lorsque des clients achètent des biens à une entreprise active dans l'économie des plateformes en ligne qui est exploitée par un groupe de détaillants indépendants sous une marque commune et que cette entreprise détermine le prix de vente des biens et transmet des commandes aux détaillants en vue de leur exécution". ([Guidelines § 193](#sources))
  - *My translation:* where the supplier chooses who fulfils, setting the price is not RPM. Example: customers buy from an online platform, the platform sets the price and forwards Orders to independent retailers to fulfil.
- **Mapping to our flows:**
  - **Delivery fits.** The Customer checks out with the platform before any Butcher is involved. The platform's dispatch (offer to all Butchers in the Delivery Radius, first to Accept) decides who fulfils, and "for Delivery, the Customer does not choose the Butcher" ([`GLOSSARY.md`](../GLOSSARY.md)).
  - **Pickup is the weak spot.** § 193 continues: "lorsque le client sélectionne l'entreprise qui fournira les services d'exécution, l'imposition d'un prix de vente par le fournisseur peut restreindre la concurrence … [et] peut équivaloir à une pratique de prix de vente imposés." On the map, the Customer sees the nearest Butcher and can switch, so the Customer chooses. **⚖ Lawyer.** Options to reduce the risk:
    - (i) The platform assigns the Pickup Butcher, and the Customer may only express a preference.
    - (ii) Butchers set their own fulfilment fee to the platform, so they still compete for fulfilment. They should not have to compete on the retail price.
    - (iii) Accept the risk on the argument that the Customer is choosing a location, not a seller.
- **Footnote 109:** § 193 is "sans préjudice de l'appréciation des accords horizontaux conclus entre les détaillants qui mettent en place et utilisent un tel modèle d'exécution". If Butchers jointly own or govern the platform, it is assessed as cooperation between competitors. **Keep the platform independent of the Butchers.**
- **Classification:** a platform that buys and resells is not an intermediation provider for that transaction ([Guidelines §§ 65, 68](#sources)). It is assessed as a reseller or supplier, and § 193 fulfilment logic applies.
- **The Butcher's price to the platform:** the platform is the buyer of fulfilment. Agreeing a per-Order fulfilment price or fee with each Butcher is ordinary purchasing, not RPM. RPM restricts the *buyer's* resale price, and here the platform is the buyer ([Guidelines § 185](#sources)). Do not publish one Butcher's terms to the others ([Guidelines § 44](#sources)).

**Legal seller:** the **platform**. It is the Customer's contract partner, issues the invoice, collects payment and is responsible for the goods toward the Customer. The Butcher is the platform's supplier or subcontractor.

**Knock-on effects to check, outside this ticket's competition-law scope:**

- Food-information duties fall on the operator under whose name the food is marketed, so the platform must provide allergen and ingredient information for Weißwurst, Brezn and Senf.
- Product liability and recourse against the Butcher.
- VAT on the full price, because the platform invoices the Customer rather than just taking a commission.
- Consumer-law rules for perishable goods.
- The Courier Service contract is held by the platform.

### 3d. Recommended or maximum price, Butcher stays seller → lawful but empty

- A **maximum** or **recommended** resale price is not hardcore. It is block-exempted if each party's market share is ≤ 30 %, unless pressure or incentives turn it into a fixed price ([Guidelines §§ 188, 198](#sources); Art. 4(a) VBER).
- How it would work: the Customer is charged the Platform Price as a maximum, and each Butcher may sell for less, with the difference refunded. This must be a real freedom. Ranking or dispatch that favours Butchers at the maximum, or delisting those who go lower, is indirect RPM ([Guidelines § 187(f), 188](#sources)). A recommended price that everyone follows can also act as a "point focal" ([Guidelines § 199](#sources)).
- **The product problem:** the map lists Butcher-specific prices as out of scope. A real right to price lower means showing or refunding different prices per Butcher.

**Legal seller:** the Butcher. **Verdict: legally possible, but it does not give the guaranteed single price the product needs.**

### 3e. Butchers agree among themselves on one price → cartel unless exempted

- An agreement between competing Butchers to charge one price is a horizontal price-fixing agreement.
- § 3 GWB (Mittelstandskartelle) exempts cooperation between competing SMEs that rationalises economic activity, does not substantially impair competition, and improves the competitiveness of SMEs ([§ 3 GWB](https://www.gesetze-im-internet.de/gwb/__3.html)). Joint selling at a common price by small Metzgereien could arguably qualify. **⚖ Lawyer.** This is not a sound base for a platform that wants to recruit any Butcher in München.
- **Analogous precedent:** the Luxembourg competition authority found price fixing among competing taxi firms on the Webtaxi booking platform, which set one algorithmic fare. It then **exempted** the arrangement on efficiency grounds: fewer empty runs, shorter waits, no viable alternative, and a 26 % share. This was the authority's first exemption of a restriction between competitors (Conseil de la concurrence, decision of June 2018; reported by [Arendt](https://www.arendt.com/wp-content/uploads/2024/04/2018.06.26_-_luxembourg_newsflash_-_webtaxi-_french.pdf) and [Concurrences](https://www.concurrences.com/en/bulletin/news-issues/june-2018/the-luxembourg-competition-authority-grants-an-exemption-to-a-price-fixing)). It shows that the efficiency argument can succeed. It is a foreign decision, however, and the burden of proof rests on us.

**Legal seller:** the Butcher. **Verdict: avoid.**

## 4. Hub-and-spoke: a risk in every structure

- The Commission notes that RPM can be linked to "une collusion horizontale prenant la forme d'un réseau en étoile" ([Guidelines fn. 103](#sources)).
- In **Eturas** (C-74/14, 21 Jan 2016, ECLI:EU:C:2016:42), a booking-platform administrator sent a system message capping discounts at 3 % and enforced the cap technically. The CJEU held that travel agencies **aware of the message** may be presumed to have taken part in a concerted practice. They could rebut this by publicly distancing themselves, reporting to the authorities, or showing they systematically applied larger discounts. Awareness of the technical restriction alone is not enough.
  - *Source note:* I could not open the judgment text from Curia or EUR-Lex in this session. The summary matches the Lithuanian authority's account ([LVAT](https://lvat.lt/en/news/court-of-justice-of-the-european-union-has-delivered-its-preliminary-ruling-in-the-travel-agencies-case/367)) and [Willkie](https://www.willkie.com/publications/2016/04/the-court-rules-on-the-burden-of-proof).
- **For us:**
  - Under 3c the price is the platform's own selling price, so there is nothing for the Butchers to coordinate on the retail side.
  - Keep each Butcher's fulfilment fee bilateral and confidential.
  - Do not run a Butcher forum or group channel about prices.
  - Do not let Butchers jointly control pricing ([Guidelines § 44, fn. 109](#sources)).

## 5. Related: platforms and price control in Germany

The Bundeskartellamt prohibited Amazon's "Preiskontrollmechanismen" on its marketplace. These mechanisms check marketplace sellers' prices and remove or demote offers that Amazon considers too high. The decision included a disgorgement of about €59 m (press release 5 Feb 2026, under § 19a GWB, [Bundeskartellamt](https://www.bundeskartellamt.de/SharedDocs/Meldung/DE/Pressemitteilungen/2026/02_05_2026_Amazon.html?nn=50118)).

§ 19a applies only to companies of "paramount significance", so it does not bind us. The decision does show that the authority closely examines any platform steering the prices of sellers who remain independent. *I read this decision only through a summary, not the full text.*

## Recommendation for the map

1. **Change the selling model.** Replace "marketplace where the Butcher is the legal seller … the platform takes a commission" with: **"the platform is the legal seller (merchant of record) at one Platform Price. Butchers are independent fulfilment partners paid a per-Order fulfilment fee. The platform's dispatch chooses the Butcher."** This is the structure that [Guidelines § 193](#sources) says is not RPM, and it keeps checkout-before-assignment and first-to-Accept unchanged.
2. **Pickup.** Have the platform assign the Butcher, with the Customer's choice treated as a preference, or get legal sign-off on Customer choice. **⚖ Lawyer.**
3. **Independence and hygiene.**
   - No Butcher ownership or governance of pricing.
   - Bilateral, confidential fulfilment terms.
   - No price communication between Butchers.
4. **New follow-up questions** created by the change of seller: food-information duties, product liability and recourse, VAT on the gross price, the shape of the Butcher fulfilment contract and fee, and consumer-law duties. These are likely a new research ticket.
5. **Glossary impact.** The definitions of Butcher ("sells to the Customer through the platform") and Platform Price ("price … every Butcher sells at") assume the Butcher is the seller. Revise both, for example: Butcher = "a partner Metzgerei that prepares the Orders it accepts on behalf of the platform"; Platform Price = "the single price at which the platform sells a Product".

**⚖ Lawyer, before launch:** a German Kartellrecht review of (a) the fulfilment contract with Butchers, (b) the Pickup flow, and (c) any growth past 30 % share of a relevant market, where the analysis changes.

## Sources

Primary sources, as read:

- **Guidelines on vertical restraints**, Commission Notice 2022/C 248/01, OJ C 248, 30.6.2022, p. 1. Paragraphs cited: 31–36, 44–46, 64–68, 185–199. Read in the official French OJ edition ([PDF copy of OJ C 248/1](https://www.franchise-fff.com/wp-content/uploads/2023/02/lignes-directrices-restrictions-verticales-30-06-2022.pdf)); quotes above are from that text and English renderings are mine. Official link: [EUR-Lex CELEX:52022XC0630(01)](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:52022XC0630(01)). EUR-Lex refused automated access in this session, so check the English wording there.
- **Regulation (EU) 2022/720** (VBER), Art. 1(1)(d)–(e), 2, 3, 4(a): [EUR-Lex](https://eur-lex.europa.eu/eli/reg/2022/720/oj/eng). Read through the Guidelines' restatement in §§ 67, 188 and 198, not directly.
- **GWB** [§ 1](https://www.gesetze-im-internet.de/gwb/__1.html), [§ 2](https://www.gesetze-im-internet.de/gwb/__2.html), [§ 3](https://www.gesetze-im-internet.de/gwb/__3.html).
- **Bundeskartellamt, Bagatellbekanntmachung** Nr. 18/2007 of 13 March 2007, Rn. 13–14: [PDF](https://www.bundeskartellamt.de/SharedDocs/Publikation/DE/Bekanntmachungen/Bekanntmachung-Bagatellbekanntmachung.pdf?__blob=publicationFile&v=2).
- **CJEU C-74/14 Eturas**, ECLI:EU:C:2016:42 ([EUR-Lex CELEX:62014CJ0074](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:62014CJ0074)). Not opened directly; see the note in §4.
- **Bundeskartellamt, Amazon Preiskontrollmechanismen**, press release 5 Feb 2026: [link](https://www.bundeskartellamt.de/SharedDocs/Meldung/DE/Pressemitteilungen/2026/02_05_2026_Amazon.html?nn=50118).
- **Conseil de la concurrence (Luxembourg), Webtaxi**, June 2018. Read through [Arendt](https://www.arendt.com/wp-content/uploads/2024/04/2018.06.26_-_luxembourg_newsflash_-_webtaxi-_french.pdf) and [Concurrences](https://www.concurrences.com/en/bulletin/news-issues/june-2018/the-luxembourg-competition-authority-grants-an-exemption-to-a-price-fixing); the decision text itself was not read.

**Considered but not relied on.** CJEU C-434/15 *Elite Taxi v Uber Systems Spain* (ECLI:EU:C:2017:981) holds that a platform which sets at least the maximum fare and controls the service exercises "decisive influence" and provides the underlying service itself. That is relevant to services and transport regulation, not directly to Art. 101. It does support the point that a platform which sets the price and controls the service looks like the real seller, which is consistent with option 3c. The judgment text was not accessible in this session.
