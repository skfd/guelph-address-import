# guelph-address-import

The **Guelph city checkout** of the address-import family — city #3,
scaffolded 2026-08-15. Status: **pre-start, drafts written, nothing published**.
`ARandomThumbtack_Import` imported Guelph's addresses solo in 2025 (first
changeset 2025-09-16, declared complete 2025-10-23 on the
[wiki page](https://wiki.openstreetmap.org/wiki/Guelph/Address_Import)),
leaving only ~2,523 of 40,634 civic addresses missing (6.2%, diffuse — 2026-08-10
survey). What is proposed here is **continuous gap-fill and QA over that
finished import**, not a re-import: create-only, human-reviewed per batch,
re-run when the City publishes.

**Prior importer contacted — go-ahead given.** `ARandomThumbtack` was asked
directly and is content for this project to take on continuous upkeep. (The
draft message that was written for this, `CONTACT_PRIOR_IMPORTER.md`, was
superseded by that conversation and removed; it is in git history at `0c268bb`.)

Guelph is **not a cold start**, and neither is the forum. Two things the
scaffold docs predate:

- The import's own forum thread is
  [#135103](https://community.openstreetmap.org/t/import-addresses-from-city-of-guelph-data/135103),
  tagged `import` / `import-proposal`, and we are already in it — posts #11 and
  #13 (2026-08-20/21). Announcements go **there**, not in a new topic.
- We have already edited Guelph addresses: changesets 187773424, 187776928,
  187822533 fixed all 29 `addr:unit`-only objects (2026-08-21/22).

Remaining drafts, unpublished, in this order:

1. `IMPORT_PROPOSAL.mediawiki` — wiki page `Guelph/Import/AddressPoints`
   (deliberately short; defers to `Toronto/Import/AddressPoints` for the shared
   machinery). Their `Guelph/Address_Import` page is theirs — link, never edit.
2. `WIKI_CATALOGUE_ENTRY.mediawiki` — the `Import/Catalogue` row, same sitting.
3. `FORUM_ANNOUNCEMENT.md` — a **reply on thread #135103**, posted once the wiki
   page is live so its link resolves. It also withdraws the `addr:full`
   suggestion left in post #13, which was tested against Nominatim and does not
   work. That withdrawal is the time-sensitive part: it is currently the last
   word in the thread.

The unit-tagging convention is a **separate** track with its own consensus
requirement, written up outside this repo at
`~/Code/obsidian/skfd/OSM Research/Guelph Unit Addresses (tagging convention).md`.
Guelph's units are mostly in OSM already, double-encoded (`addr:housenumber=714-30`
plus `addr:unit=30`) on 5,422 objects; the original importer defends that form.
Do not let it entangle the import proposal — the import is create-only and
touches none of those nodes.

**Blocker before the pilot upload** (not before publishing — the wiki page states
intent): the engine writes only `addr:housenumber`, `addr:street`, `source` and
the enriched `addr:postcode` (`t2/conflate.py`), and no `source:license` on the
changeset (`t2/osm_export.py`). The wiki tagging table also promises
`addr:city=Guelph` and `addr:province=Ontario`, kept to stay consistent with the
2025 import. Constant node tags and a changeset `source:license` are an engine
change that must land before the first Guelph changeset, or the pilot will
contradict its own published tagging plan.

The pipeline lives in the engine repo,
[`address-importer-friend`](https://github.com/skfd/address-importer-friend)
(see its README for setup). This repo carries only Guelph's `config.toml`,
credentials (`.env.*`, gitignored, from the `.example` files), and — locally,
gitignored — `data/` with the OSM extract and `data/guelph/tool.db`.

```bash
cd ../address-importer-friend
python run.py --city-dir ../guelph-address-import
```

Source data: City of Guelph address points, consumed via the sibling
[`ontario-address-changes`](https://github.com/skfd/ontario-address-changes)
tracker (`data/guelph/guelph.db`, 53,846 active rows / 40,634 civic addresses
at snapshot 39, 2026-08-13). 24.4% of rows carry units, collapsed to civic per
the `[units]` policy. Tiles subdivide the city's 23 "Guelph Areas" polygons
(99.47% point coverage, probed 2026-08-15).

Entry state re-confirmed 2026-08-15 by `scripts/entry_state_probe.py`
(evidence: `onboarding/entry-state-2026-08-15.json`): the importer holds 85.6%
of sampled elements, 2026 activity is 49 elements and the latest 100
changesets carry no import tags — complete and quiet, not active. Tag
conventions to stay consistent with: `addr:province=Ontario` (3,699 vs 178
`ON`), postcode on the dominant combo, 725 sampled elements already carry
`addr:unit`.

Baseline conflation runs in full regardless of entry state (house rule —
entering a brownfield city in maintenance-only mode inherits prior errors
invisibly). Expect the review queue to be MATCH/MATCH_FAR-dominated, the
opposite of Hamilton; the interesting outputs are street-name disagreements,
the ~52% unit gap (source 13,162 unit rows vs OSM's 6,289 `addr:unit`), and
OSM-only addresses.

MIT licensed.
