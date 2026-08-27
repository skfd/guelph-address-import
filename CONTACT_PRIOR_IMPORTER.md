# Contact the prior importer — draft

**Send this first.** Before the wiki page goes live and before the forum post.
House rule from `05-prior-import-detection.md`: *prior-import detection produces
a person to talk to, and that output is as important as the gap number.*

Venue: OSM message to
<https://www.openstreetmap.org/message/new/ARandomThumbtack_Import>
(their personal account is `ARandomThumbtack`; the import account is the one
tagged on the changesets, so use whichever they appear to read).

**Subject:** `Guelph addresses — thanks, and an offer to help keep them current`

---

Hi,

You imported Guelph's civic addresses last autumn — first changeset 2025-09-16, declared complete 2025-10-23, ahead of your own deadline — and wrote it up on the wiki with `import=yes`, `import:page`, `source` and `source:license` on the changesets. I've spent this month surveying 42 Ontario address datasets against OSM, and Guelph is one of only two municipalities in the province above 90% coverage. That's your doing. Thank you — genuinely, it made a real difference to the map and it's better documented than most of what I looked at.

I maintain address-import tooling that ran the Toronto import in May (~449k addresses, 1,297 reviewed changesets, wiki page at `Toronto/Import/AddressPoints`), and I'm bringing more Ontario cities onto it. Guelph came up in the survey and I want to be clear that I am **not** proposing to re-import your city or to touch what you uploaded.

What I found, in case it's useful to you regardless of what happens next:

- **2,523 of 40,634** civic addresses in the current City dataset aren't in OSM (6.2%, measured 2026-08-10 via Overpass). Diffuse, not clustered — I assume this is the pile you mentioned parking in `RemainingAddresses.osm`, plus what the City has published since October.
- **6,289 of 13,162** source unit rows landed as `addr:unit` (~48%). I suspect that's your conflation rule — "conflation is only done when the number of reference objects selected matches the number of source objects selected" — declining to guess on stacked units, which is a reasonable call.
- A handful of street-name disagreements between OSM and the source that I can send you as a list.

The offer: I track the City's dataset with a differ, so I can re-conflate whenever Guelph publishes an update and hand-review the small number of new candidates. Continuous maintenance rather than another bulk load. Three ways that could go, in my order of preference:

1. **You run it.** The tooling is open source and I'll help you set it up. It's your city and your import; I'd rather it stayed that way.
2. **I run it, you review.** You get a standing veto on anything touching Guelph addresses, and I post batch results where you can see them.
3. **I just send you the diff** periodically and you do whatever you like with it.

If you'd rather I left Guelph alone entirely, that's an acceptable answer too — say so and I'll drop it.

I've drafted a wiki page (`Guelph/Import/AddressPoints`, separate from yours — `Guelph/Address_Import` is your record and I won't edit it) and a forum announcement, but I'd like your reply before either goes up. Happy to send you both drafts first if you want to see them.

Thanks again for the work.

— skfd · toronto@comentality.com
