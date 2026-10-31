# Hotel8 - Hospitality Platform

**Secure Post-Quantum-Safe Hospitality Platform That Heals and Scales Itself.**

Hotel8 is the hospitality vertical of the Sentin8 platform - an end-to-end hotel management system: from rate shopping and direct booking through front-desk arrival, restaurant meals charged to the room, upsell offers, loyalty points, channel distribution, to the nightly close. All running on one tenant-isolated model with one identity plane and one event stream.

> **A note on our history.** If you found your way here looking for
> **PRIME** - `authPrime`, `exchangePRIME`, and family - you're in the
> right place. Those projects are this one: our open-source
> work is now rebranded to **Sentin8** (this vertical is **Hotel8**) to
> avoid confusion with other brand names and products. At the same
> time we left our self-hosted **Apache SVN** behind and moved
> everything to GitHub. The lineage goes back further than you might
> expect - here's a fun LinkedIn read from 2017, when the platform was
> still called PRIME and still lived on SVN:
> [Advanced Hybrid Securities Exchange](https://www.linkedin.com/pulse/advanced-hybrid-securities-exchange-hamid-jahanzad/).

## Services

| Service | Domain | What it does |
|---|---|---|
| **Hotel8:PMS** | Property Management System | Properties, rooms/types, rate plans, reservations (create/edit/cancel/split), front desk, folios, housekeeping, groups & banquet events, night audit, digital keys, reporting |
| **Hotel8:POS** | Restaurant & Room Service | Table service, menus with modifiers, splitting/merging transfers, manager approvals, room charges into the PMS folio, offline queue, guest room-service |
| **Hotel8:Upsell** | Revenue Growth | Rules-as-data offers (upgrades, early check-in, amenities) generated on booking events, A/B tested, guest one-tap accept with bill8 payment |
| **Hotel8:Distrib** | Channel Distribution | OTA/GDS/wholesaler onboarding, mappings, per-channel markup, ARI push, reservation intake from channels |
| **Hotel8:Meta** | Metasearch Presence | Partner channels (Google Hotel Ads et al.), rate ads derived from live availability, click attribution feeding eBooking |
| **Hotel8:Loyalty** | Loyalty Program | Programs, tiers, points earning on stays, redemptions at hotel and restaurant touchpoints, catalog management |

## Flutter Apps

| Repository | Description |
|---|---|
| `hotel8-ebooking` | Commission-free guest booking funnel under the hotel's brand with metasearch attribution |
| `hotel8-ecorpbooking` | Negotiated-rate portal with policy-guarded search, traveler rosters, credit methods |
| `hotel8-eguest` | Pre-registration, mobile check-in, room service ordering, upsell accepts, digital key |
| `hotel8-ekiosk` | Self-service lobby: check-in/out with add-on sales and payment, express walk-in booking |
| `hotel8-epms` | Staff console: reservations, front desk, housekeeping, cashiering, rates, events |
| `hotel8-epos` | Restaurant & room service terminal app |
| `hotel8-admin` | Vertical admin console (RFW) |

## Platform Dependencies

Hotel8 depends on these Sentin8 platform services for cross-cutting concerns:

| Platform Service | What Hotel8 gets from it | Dependency direction |
|---|---|---|
| **Sentin8:Auth** | Identity, sign-in, consent | hotel8 → Auth (verification + M2M identity) |
| **Sentin8:BFF** | Client token custody & proxying | hotel8 ← BFF (user tokens proxied to services) |
| **Sentin8:Bus** | Durable event transport & audit store | hotel8 → Bus (produce + consume domain events) |
| **Sentin8:Flow** | Workflow orchestration (retries, wait states) | hotel8 → Flow (workflow execution) |
| **Sentin8:Cron** | Scheduled & workflow execution timing | hotel8 → Cron (schedule registration) |
| **Sentin8:Func** | Tenant-authored executable hooks | hotel8 ← Func (proxy-back handlers for hooks) |
| **Sentin8:Storage** | Object storage (documents, images) | hotel8 → Storage (document PUT/GET) |
| **Sentin8:Stat** | Business metrics aggregation and telemetry | hotel8 → Stat (event ingestion) |
| **Sentin8:Sys** | Organization/environment lifecycle, service enablement | hotel8 → Sys (env-init events) |
| **Bill8:Payments** | Card capture, payment settlement, ledger | hotel8 → Bill8 (payment processing) |
| **Bill8:Ledger** | Balance management & financial postings | hotel8 → Bill8 (financial data) |
| **Sentin8:Post** | Email transport (confirmation emails, receipts) | hotel8 → Sentin8:Post (email delivery) |

## Key Invariants

- **Idempotent creation** - a reservation, check, offer, or member record is created exactly once per request (idempotency keys on every event-triggered path)
- **Money moves only through bill8** - charges + balance transactions; no vertical service computes or stores balances
- **No integration tax** - reservations, folios, menus, offers, rewards, and channel availability live in one tenant-isolated model with one identity plane and one event stream
- **One source of truth** - staff/guest apps share the same authorized domain model; no per-app copies of business rules

---

**Architecture & Design:** hotel8 and its underlying concepts were designed by [Hamid Jahanzad](https://www.linkedin.com/in/hamidjahanzad/).
