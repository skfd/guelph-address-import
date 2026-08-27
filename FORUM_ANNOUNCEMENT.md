# Forum comment — draft

**Reply in the existing thread. Do not open a new topic.**

<https://community.openstreetmap.org/t/import-addresses-from-city-of-guelph-data/135103>
— ARandomThumbtack's original import thread, 13 posts, already tagged `import`
and `import-proposal`. Your posts are #11 (2026-08-20) and #13 (2026-08-21,
currently the last post in the thread).

This reply does two jobs: it **withdraws the `addr:full` suggestion** you left
in post #13, which you have since tested and disproved, and it makes the
continuous import **publicly announced** — ARandomThumbtack's go-ahead came by
ping, and the Import Guidelines want the announcement and the wiki link in the
open where other Ontario mappers can object.

Post it *after* `Guelph/Import/AddressPoints` is live, so the link resolves.
Check the two `<...>` placeholders, and check the third paragraph actually
matches what ARandomThumbtack agreed to — I only know that they were OK with it,
not in what words.

---

Following up on my own post above, with a correction and a proposal.

**First, the correction: withdraw the `addr:full` idea.** I suggested it as a compatibility shim so the postal string stays searchable. I tested it against live Nominatim on 2026-08-21 and it does nothing — Nominatim indexes the object but discards `addr:full` *and* `addr:unit` outright. Searching the exact `addr:full` value of an indexed node returns no relevant result; `/details` shows neither tag in extratags. So the shim buys no searchability while restating housenumber + street + unit + city in a second place that can drift. Don't add it. My apologies for floating it untested.

What the test does show, and it's the honest cost of the whole discussion: **unit-level search does not work under any tagging scheme available today.** "72 York Road Unit 5" returns the property with the unit silently ignored; "5-72 York Road" returns unrelated road segments. So the combined-housenumber form isn't buying searchability either — and it *loses* plain "714 Willow Rd" queries, because then no object carries the bare civic number. That's the trade-off in one line: correct tagging degrades unit search to property search, and the current form degrades property search too.

@ARandomThumbtack — one incidental find while testing: Guelph had 29 objects tagged with `addr:unit` and nothing else, no housenumber or street, which makes them invisible to every geocoder. 24 were mine from an earlier fumble, 5 were other mappers'. All fixed, changesets 187773424, 187776928 and 187822533. Guelph now has none.

**Second, the proposal — continuous maintenance of Guelph's addresses.**

I've spoken to @ARandomThumbtack and they're happy for me to pick this up; I'd rather say so here in the open than leave it in a DM, and they should feel free to correct me if I've overstated it.

The case for it is in their own post above: the City published an update adding 51 addresses on 2026-08-06, and nobody is currently signed up to carry those into OSM. Right now **2,523 of 40,634** civic addresses in the City dataset aren't in OSM (measured 2026-08-10 via Overpass) — diffuse, no unmapped district, mostly the tail explicitly parked in `RemainingAddresses.osm` plus ten months of new construction.

So rather than another bulk load: I track the City dataset with a differ, and when it changes I re-conflate, hand-review the handful of new candidates, and upload one changeset per area. Steady state should be small and boring.

To be explicit, because "continuous" makes people reasonably nervous:

- Nothing uploads on a timer. Every candidate is approved by a human in a review UI before a changeset opens.
- The recurring automated part is the **conflation**, which is read-only.
- **Create-only. No deletions, no edits to existing objects** — including no re-tagging of the 2025 import's nodes. The unit normalisation below is a separate question with its own consensus.
- Changesets stay `import=yes` / `bot=no`: a sequence of small reviewed imports, not an automated edit.

Same tooling as the [Toronto address import](https://wiki.openstreetmap.org/wiki/Toronto/Import/AddressPoints) in May (1,297 reviewed changesets, ~449k addresses, same dedicated `skfd imports` account). The Toronto page has the full detail on conflation, checks, audit log and revert plan; the Guelph page is deliberately short and covers only what differs:

**<https://wiki.openstreetmap.org/wiki/Guelph/Import/AddressPoints>**

Feedback window **14 days**, to <date + 14 days>. Then one pilot area tile, posted here with its counts and its changeset, then a week's hold before the rest.

**One tagging question I'd like settled before any of that.** Guelph writes `addr:province=Ontario` on essentially every address (3,699 against 178 `ON` in my sample) because the 2025 import wrote it as a constant. Toronto's import omits province per the usual Canadian convention. Local consistency and provincial convention disagree, and I'd rather be consistent with Guelph than with a convention — but say so if that's wrong.

**And to be clear about what I'm *not* proposing here:** the 5,422 objects carrying both `addr:housenumber=714-30` and `addr:unit=30` stay exactly as they are. @ARandomThumbtack and I don't agree on that one yet, it's a mechanical edit that needs its own consensus under the Automated Edits code of conduct, and it has nothing to do with adding missing addresses. Happy to keep arguing it separately.

Contact: toronto@comentality.com · tooling: [address-importer-friend](https://github.com/skfd/address-importer-friend), [guelph-address-import](https://github.com/skfd/guelph-address-import)
