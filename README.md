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

Announcement drafts live in this repo and are **unpublished**. Send them in this
order:

1. `CONTACT_PRIOR_IMPORTER.md` — OSM message to `ARandomThumbtack_Import`
   first, thanking them and offering the tooling. Wait for a reply.
2. `IMPORT_PROPOSAL.mediawiki` — wiki page `Guelph/Import/AddressPoints`
   (deliberately short; defers to `Toronto/Import/AddressPoints` for the shared
   machinery). Their `Guelph/Address_Import` page is theirs — link, never edit.
3. `WIKI_CATALOGUE_ENTRY.mediawiki` — the [[Import/Catalogue]] row, same sitting.
4. `FORUM_ANNOUNCEMENT.md` — Community Forum topic, tagged `import`, opening the
   14-day feedback window.

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
