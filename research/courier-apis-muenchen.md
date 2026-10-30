# Courier Service APIs for München morning Deliveries

Research for [#3](https://github.com/denniskasper/ww-app/issues/3) (map: [#1](https://github.com/denniskasper/ww-app/issues/1)). Sources were read on 2026-10-30.

**Question:** Which Courier Services offer an API to book a Driver for a pickup at a Butcher and a Delivery to a Customer in München, before 12:00, for both ASAP Orders and Scheduled Orders, and what does a Delivery cost?

## Answer

- **Uber Direct** is the only candidate that names München as a market, publishes a price ("from €7,90 per Delivery"), offers self-serve API sign-up, and schedules up to 30 days ahead. It is the best first choice.
- **Wolt Drive** is the strongest second choice. Its API is very complete: an availability-and-price quote per address, scheduled drop-off times from 60 minutes to 7 days ahead, and rich webhooks. Prices and München coverage are not published. Credentials come only through Wolt's local sales team.
- **Stuart is out**: it operates only in the UK, France and Poland. I found no Lieferando/Just Eat or DoorDash Drive offering for non-marketplace merchants in Germany.
- **Local bike couriers** publish clear prices of about €13–14 for a 3 km run. However, they have no usable public API (Radkurier24 claims one but doesn't document it, and TransPedal takes phone or email orders). They also offer time windows ("within 2 h"), not a "by 12:00" commitment.
- **Not verifiable from any primary source:** courier supply between 07:00 and 09:00 and at weekends in München, and any food or temperature handling commitment. Both must be tested with live quotes before launch.

## Comparison

| | Uber Direct | Wolt Drive | Stuart | Radkurier24 (bike) | TransPedal (bike) |
|---|---|---|---|---|---|
| München coverage | Named: "including Munich" [U1] | Germany yes [W1]; München not named, check per address via API [W2] | **No**: UK, FR, PL only [S1] | Yes, München-based [R1] | Yes, city + S-Bahn area [T1] |
| Morning / weekend hours | Not published. "Courier availability … may be limited depending on the time of day" [U1] | Not published. API error `REQUEST_OUTSIDE_DELIVERY_HOURS` [W3] | n/a | "7 Tage … über 24 Stunden" (marketing) [R1] | Mo–Fr 8:00–18:30. Weekends +€10 net surcharge [T1] |
| Scheduled booking | Up to 30 days ahead [U1][U2] | `scheduled_dropoff_time` 60 min to 7 days ahead [W2] | Slots (n/a) [S2] | Windows "innerhalb 2 h / 2–6 h". Future days via app [R1] | Phone only [T1] |
| API | REST + OAuth, sandbox [U3] | REST + bearer token, dev env [W4] | REST [S2] | "moderne API" claimed, no docs [R2] | None [T1] |
| Price per Delivery | "From €7,90", varies by distance, speed, integration [U4] | Per contract, VAT incl., returned by API; no EUR figure published [W2] | "from £5.50" (UK) [S1] | €7,50 base + €2/km (bike, 5 kg, 2 h), gross [R1] | €9,50 net incl. 1 km + €1,70/km net [T1] |
| Typical time | ASAP: drop-off < 90 min after creation [U2]; 81 % within 45 min (global) [U4] | "30–60 minutes, or right on schedule" [W1] | — | 2 h or 2–6 h windows [R1] | — |
| Tracking / webhooks | `tracking_url`, status + courier webhooks (location every 20 s), HMAC-signed [U3][U5] | Tracking URL, SMS, ~13 order webhooks, JWT-signed [W4][W5] | Claimed [S1] | E-mail tracking link [R2] | "Dokumentierte Zustellung" +€5 [T1] |
| Food / cold | Not documented. Limits: 40×40×40 cm, 10 kg [U4] | Not documented; `bag_count` field only [W4] | Chilled (UK/FR/PL marketing) [S1] | Not offered; customer packs suitably [R3] | Not mentioned [T1] |
| Onboarding | Self-serve at direct.uber.com, billing info, pilot with real stores. No minimums [U3][U4] | Contact local Wolt Drive team for credentials; venue configured by Wolt [W2] | Direct API for 8+ stores [S2] | Book without registration; regulars use app [R1] | Monthly invoice or cash [T1] |

## Per provider

### Uber Direct

- **Coverage.** Uber's München courier page says Direct is "currently available in hundreds of cities, including Munich" and that items can "typically" be delivered "within a 20-kilometer radius in Munich" [U1]. The merchant page says "a radius of 10 kilometers" [U4], so plan with 10 km. Account creation is "currently available only in select regions", so confirm with Uber [U3].
- **Hours.** No courier hours are published. "You can schedule deliveries anytime … Courier availability, however, may be limited depending on the time of day" [U1]. **Unverified:** supply at 07:00–09:00 and on weekends.
- **ASAP Orders.** If `dropoff_deadline_dt` is left blank, the expected drop-off is "< 90 minutes in the future from the order creation time" [U2].
- **Scheduled Orders.** `pickup_ready_dt` "must be less than 30 days in the future" [U2]. For a same- or next-day scheduled Delivery, `pickup_ready_dt` must be at least 60 min after creation and `pickup_deadline_dt` at least 60 min after that. The drop-off window must span at least 240 min [U2]. My reading, to confirm with Uber: a window ending at 12:00 would have to open by about 08:00. The doc also describes a shorter "windows" variant (drop-off deadline ≥ pickup-ready + 90 min). Booking the evening before (for example, at Accept) fits the 30-day rule.
- **Price.** "From €7,90 per delivery—no commissions." Footnote: "Your rates may be higher or lower depending on your order type, distance, speed of delivery (express, same-day, next-day, scheduled), integration type (channel partner, API, dashboard), market region, and other factors." "No startup costs, commissions, or monthly minimums" [U4]. The real price comes from `POST /v1/customers/{customer_id}/delivery_quotes`; a quote is valid for 15 minutes [U3][U5]. **Unverified:** an actual München quote.
- **Tracking.** `tracking_url` per Delivery. The webhooks are `event.delivery_status` and `event.courier_update` (location "every 20 seconds"), plus an "imminent" flag at about 80 m. They are signed with `x-uber-signature` (HMAC-SHA256) and retried on 5xx and timeouts [U3][U5][U6].
- **Food.** No food, temperature or insulated-bag policy is published. The parcel limit is "40x40x40 cm and a weight of up to 10 kg"; pharmaceuticals, alcohol and tobacco are restricted [U4]. A Weißwurstfrühstück fits easily.
- **Onboarding.** Self-serve: log in with an Uber account, accept the terms, and add billing details before production credentials are issued. Test mode is the default. Production needs "actual stores to participate in a pilot run" [U3]. The first 5 Deliveries are free (promo) [U4].

### Wolt Drive

- **Coverage.** The Wolt Drive merchant page is the Germany edition [W1]; Wolt operates in München (city page exists). Drive coverage is checked per address: a shipment promise is invalid "if the recipient is located outside the delivery area or request is outside Wolt's operating hours" [W2]. `GET /merchants/{id}/delivery-areas` returns polygons [W4]. **Unverified:** München Drive coverage and morning hours.
- **Hours.** Not published. The API returns `REQUEST_OUTSIDE_DELIVERY_HOURS` and `VENUE_CLOSED` [W3].
- **ASAP Orders.** "Orders are delivered in 30–60 minutes, or right on schedule" [W1]. The pickup ETA is reliable only after about 90 s, and it "should not be considered accurate enough to time sensitive venue operations" [W2].
- **Scheduled Orders.** "The requested scheduled drop off time can vary from 60 minutes to 168 hours, or 7 days at the time of delivery creation." Wolt "will aim to deliver the order at exactly the requested time" [W2]. The target is a time, not a window. To book "before 12:00", ask for a target such as 11:30 (my inference).
- **Price.** "You are charged on a per-delivery basis and invoiced once a month" [W1]. The API price "is the price based on merchant's contract with Wolt… Prices include the local VAT… should not be hardcoded" [W2]. No EUR figure is published. **Pricing is behind a sales contact.**
- **Tracking.** A tracking URL per order and optional SMS. Webhooks include `order.received`, `order.pickup_started`, `order.picked_up`, `order.dropoff_arrival`, `order.delivered`, `order.not_delivered` and ETA updates, as HS256 JWTs [W4][W5]. Cancelling is possible until a courier accepts [W4].
- **Food.** No food or thermal policy is published. A `bag_count` field exists [W4].
- **Onboarding.** "Before beginning the development, get in touch with the local Wolt Drive team to receive the development credentials." Wolt configures the venues (one per Butcher, "venueful") or allows a venueless mode after you consult Wolt [W2]. A dev environment is provided [W4]. **Contract terms are unpublished.**

### Stuart

The help centre lists UK zones only and says: "Outside the UK, we are also operating in France and Poland" [S1]. The country selector offers only the UK, France and Poland [S1]. The Direct API targets businesses "with 8+ stores" [S2]. **Not usable in München.**

### Local bike couriers

- **Radkurier24** (München). Its own price table (gross, "Stand 02/25") lists a bike courier for up to 5 kg at a €7,50 Anfahrt plus €2/km (2 h window), or €1,50/km (6 h window). A 3 km run costs about €13,50 [R1]. It claims "eine moderne API für die automatische Übertragung aller Sendungsdetails", but no docs are public [R2]. Tracking is by e-mail link [R2]. The AGB (2021) don't cover food; goods must be suitably packed. The AGB also bar automated data reading from the portal [R3]. Booking is by window, not clock time.
- **TransPedal** (München since 1988). Orders are by phone or e-mail only, Mo–Fr 8:00–18:30. Prices are €9,50 net incl. 1 km plus €1,70/km net. Saturdays, Sundays and holidays cost €10 net extra within München [T1]. There is no API.
- **Not checked:** other München couriers. I found no official sites with API docs, and directory searches turned up no München courier with a documented booking API.

### Not found or not available

- **Lieferando / Just Eat Takeaway:** I found no first-party delivery-as-a-service offer for orders outside Lieferando. The partner page could not be fetched (403). **Unverified.**
- **DoorDash Drive:** The docs and landing page could not be fetched (404/403). In Europe, DoorDash's delivery-as-a-service is Wolt Drive (DoorDash owns Wolt). **Unverified** from a primary source.
- **tiramizoo** (München): the homepage now presents B2B SaaS and 3PL services ("Kontakt aufnehmen"). The API docs did not load. **Not usable without a sales conversation.**

## Recommendation for the map

1. **Build against Uber Direct first, with Wolt Drive as the second Courier Service behind the same internal interface.** Both offer quote → create → webhook → cancel, a tracking URL, per-Delivery pricing and scheduling far enough ahead for evening-before Scheduled Orders. Uber can be started self-serve today, and Wolt needs a sales call. Pursue both, because neither publishes morning courier supply.
2. **Before the spec, run a supply test.** In Uber's sandbox and production quote flow, and Wolt's once credentials arrive, request quotes for Butcher → Customer pairs across München at 07:00, 08:00, 09:00 and 10:30, on weekdays and Saturday/Sunday. This settles the earliest delivery slot and the real price per Delivery, which no source publishes.
3. **Planning assumptions until then:**
   - Assume about **€8–14 per Delivery** (Uber "from €7,90"; bike couriers €13–14 for 3 km).
   - Assume a **Delivery Radius ≤ 10 km**; keep it much smaller in practice to protect price and freshness.
   - **ASAP Orders** need roughly 30–90 min after Accept, so the ASAP cut-off should be around 10:30.
   - Book **Scheduled Orders** with the Courier Service at least 60 min ahead (both APIs require it). Booking at Accept the evening before works for both.
4. **Food handling is the platform's and Butcher's job.** No Courier Service commits to temperature or insulated bags. Weißwurst must be packed hot-and-sealed (insulated packaging) by the Butcher; treat this as a Butcher-journey requirement.
5. **Skip bike couriers for the MVP integration.** Keep Radkurier24 only as a manual fallback, because it has no documented API and no clock-time guarantee.

## Sources

- [U1] Uber, Courier services München: https://www.uber.com/de/en/b/courier-services/munchen-by-de/
- [U2] Uber Direct, delivery window guide: https://developer.uber.com/docs/deliveries/guides/delivery-window
- [U3] Uber Direct, get started: https://developer.uber.com/docs/deliveries/get-started
- [U4] Uber Direct merchant page (DE): https://merchants.ubereats.com/de/en/services/uber-direct/
- [U5] Uber Direct, FAQ: https://developer.uber.com/docs/deliveries/faq
- [U6] Uber Direct, webhooks: https://developer.uber.com/docs/deliveries/guides/webhooks
- [W1] Wolt Drive merchant page (Germany): https://merchant.wolt.com/wolt-drive (and web app: https://merchant.wolt.com/en/deu/wolt-drive/web-app)
- [W2] Wolt Drive docs overview: https://developer.wolt.com/docs/wolt-drive
- [W3] Wolt Drive error handling: https://developer.wolt.com/docs/wolt-drive/error-handling
- [W4] Wolt Drive API reference and endpoints: https://developer.wolt.com/docs/api/wolt-drive, https://developer.wolt.com/docs/wolt-drive/endpoints
- [W5] Wolt Drive webhooks: https://developer.wolt.com/docs/wolt-drive/webhooks
- [S1] Stuart: https://stuart.com/ and https://help.stuart.com/en/articles/6967288-zones-and-distances-covered-by-couriers
- [S2] Stuart developers: https://stuart.com/developers/ and https://stuart.com/developers/best-practices/scheduled-offering/
- [R1] Radkurier24 München prices: https://radkurier24.com/fahrradkurier-muenchen/
- [R2] Radkurier24 services: https://radkurier24.com/services/
- [R3] Radkurier24 AGB: https://radkurier24.com/agb-kunden/
- [T1] TransPedal prices and terms: https://www.transpedal.de/preise-und-konditionen/ and https://transpedal.de/
