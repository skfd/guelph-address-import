# Forum announcement — draft

Venue: <https://community.openstreetmap.org> → **Communities / Canada**.
New topic, tags: `import`, `canada`, `ontario`, `addresses`.
Post *after* the wiki page `Guelph/Import/AddressPoints` is live — which is
itself gated on the `CONTACT_PRIOR_IMPORTER.md` message per the README's
publish order. Fill in the `<date + 14 days>` placeholder before posting.

**Title:** `Guelph addresses — continuous gap-fill over the 2025 import (proposal)`

---

First, a thank-you that should have been posted last year: **@ARandomThumbtack_Import** imported Guelph's civic addresses solo between 2025-09-16 and 2025-10-23, from the City of Guelph open data, and [wrote it up properly](https://wiki.openstreetmap.org/wiki/Guelph/Address_Import) — `import=yes`, `import:page`, `source`, `source:license` on the changesets. In a 42-dataset survey of Ontario address data I ran this month, Guelph came out as one of only *two* municipalities above 90% coverage. That is entirely their work, and it is the reason this post is not an import proposal in the usual sense.

**What I'd like to propose is the boring part nobody signed up for: keeping it current.**

## What's actually missing

Measured 2026-08-10 against a fresh Overpass query, on a City snapshot of 40,634 civic addresses:

- **2,523 addresses (6.2%)** are in the City's dataset and not in OSM. Diffuse — no unmapped district. Partly the tail the original import [explicitly parked](https://wiki.openstreetmap.org/wiki/Guelph/Address_Import) in `RemainingAddresses.osm`, partly ten months of new construction the City has published since.
- **6,289 of 13,162** source unit rows made it into OSM as `addr:unit` (~48%). I'm **not** proposing to import units — flagging it as a QA finding.
- Street-name disagreements and OSM-vs-source position drift, which I'd report rather than fix unilaterally.

## What "continuous" means

I track the City's dataset in a [separate project](https://github.com/skfd/ontario-address-changes) that snapshots and diffs it. So instead of one big upload, the proposal is: **when the City publishes new addresses, re-conflate, review the handful of new candidates by hand, upload one changeset per area.** Steady state is expected to be small.

To be explicit, because "continuous" makes people reasonably nervous:

- Nothing uploads on a timer. Every candidate is approved by a human in a review UI before a changeset opens.
- The recurring, automated part is the **conflation**, which is read-only.
- Create-only. **No deletions, no edits to existing objects** — including no re-tagging of the 2025 import's nodes.
- Changesets stay `import=yes` / `bot=no`. This is a sequence of small reviewed imports, not an automated edit.

## Tooling and precedent

Same tooling that ran the [Toronto address import](https://wiki.openstreetmap.org/wiki/Toronto/Import/AddressPoints) in May 2026 — [that thread](https://community.openstreetmap.org/t/address-import-for-toronto/119368), 1,297 changesets, ~449k addresses, same dedicated `skfd imports` account. The Toronto wiki page carries the full detail on conflation, checks, audit log and revert plan; I haven't repeated any of it.

The Guelph page is deliberately short and only covers what's different: **<https://wiki.openstreetmap.org/wiki/Guelph/Import/AddressPoints>**

## The one question worth arguing about

`addr:province=Ontario`. The 2025 import wrote it on every node and it's now dominant in Guelph (3,699 vs 178 `ON` in my sample). Toronto's import omits province per the usual Canadian convention. Local consistency and provincial convention disagree. My plan is to follow local consistency and keep writing it — **tell me if that's wrong.**

Three smaller ones on the wiki page: 75 addresses the City publishes inside Guelph/Eramosa Township that Wellington County also publishes; whether unit-level import is wanted here at all; and how often a maintenance batch should be announced here.

## Asks

- **@ARandomThumbtack_Import** — this is your city and your work. You're invited onto the reviewer roster with a veto on anything touching Guelph addresses, and if you'd rather run the maintenance yourself with this tooling, I'd genuinely prefer that outcome. Say the word.
- **Guelph mappers** — anything in the local address data you already know is wrong, tell me before I start rather than after.
- Feedback window: **14 days**, to <date + 14 days>. After that, one pilot area tile, posted here with its counts and changeset, then a one-week hold before the rest.

Contact: toronto@comentality.com · tooling: [address-importer-friend](https://github.com/skfd/address-importer-friend), [guelph-address-import](https://github.com/skfd/guelph-address-import)
