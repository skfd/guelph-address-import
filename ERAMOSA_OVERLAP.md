# The 75 `Guelph/Eramosa Twp` rows: City vs. Wellington County

Measured 2026-08-31 against City snapshot 46 (`guelph-2026-08-30`, 53,846 rows)
and Wellington snapshot 63 (`wellington-2026-08-29`, 42,928 rows), plus a live
Overpass read of the bbox `43.53,-80.33 .. 43.595,-80.235`.

Open question §5 of `IMPORT_PROPOSAL.mediawiki` asks which source should own
these. **The answer is already on the ground: 73 of the 75 are in OSM, imported
from the City source by `ARandomThumbtack_Import` in changeset 172765115
(2025-10-02).** They are not a pending import decision; they are an existing
city-sourced cluster with two loose ends.

## The two sources barely disagree

| | |
|---|---|
| City rows with `PLACE='Guelph/Eramosa Twp'` | 75 |
| Same `(number, street)` in Wellington's `Municipality='Guelph-Eramosa'` | **73** |
| Unit disagreements among those 73 | **0** |
| Street-name disagreements among those 73 | **0** — both publish OSM long form (`Speedvale Avenue East`) |
| Coordinate offset, City vs. County | median **9.2 m**, mean 11.5 m, p90 17.9 m, max 102.9 m (595 Governors Road) |

Per street, City count vs. County count on the same street:

| Street | City | County (whole township) |
|---|---|---|
| Eramosa Crescent | 18 | 18 |
| Promenade Road | 18 | 18 |
| Hillside Drive | 15 | 15 |
| Gazer Crescent | 13 | 13 |
| Eramosa Road | 5 | 9 |
| Speedvale Avenue East | 4 | 26 |
| Governors Road | 1 | 1 |
| Woodlawn Road East | 1 | **0** |

The County's larger Eramosa Road and Speedvale counts are the rural stretch
further out, not richer coverage of the same houses — except for four addresses,
below. Neither source duplicates a civic address inside this cluster, and
Wellington publishes **nothing** that collides with the City's other 53,771 rows:
zero County rows share a `(number, street)` with a `PLACE='Guelph'` row within
100 m. The overlap really is just these 75.

## The four genuine differences

1. **`672` vs. `7667` Speedvale Avenue East — same point, two house numbers.**
   The City publishes `672 Speedvale Avenue East`; Wellington publishes
   `7667 Speedvale Avenue East` **1 m away**. One is city-style numbering, one
   is the County's rural 4-digit numbering, and only one can be right on the
   sign. OSM already carries `672` (from the 2025 import); `7667` is absent.
   Wellington itself carries the city-style `676`, `688`, `692` on the same
   strip, so `7667` looks like a stale rural number the County never retired.

2. **`664 Woodlawn Road East` — City only, and legitimately so.** It is the
   *Guelph Lake Sports Fields* (the City's row carries `NAME`). Wellington has
   no `Woodlawn Road East` anywhere in its dataset — outside the city that road
   is `Wellington Road 124` — and its nearest address is 662 m away. Not in OSM.

3. **`2 Promenade Road` — City only in OSM terms.** Both sources publish it;
   the 2025 import missed it. OSM has 4–25 on Promenade Road, not 2.

4. **Four County-only addresses beside the cluster:** `750`, `756`, `762`,
   `768 Eramosa Road`, 70–83 m from the City's nearest point, on the even side
   of a stretch where the City publishes only odds (745–759). These are
   Wellington's to own, not this project's. Three are already in OSM from a
   2016 StatCan import (`source=StatCan 92-500-X`); `762` is missing.

## Attribute richness

The City's rows carry `ADDID`, `GPID`, `PIN`, `SEGMENTID`, `ROLL_NO` (73/75),
`PARITY`, `STATUS`, `WARD` (empty on all 75) and `POSTCODE` (**1** of 75).
Wellington's carry `RoadClass`, `UrbanCentre` (`Promenade Park` on 71 of the 73
matches) and the `WC_*` street decomposition. Nothing either side publishes
becomes an OSM tag under this project's projection, so richness does not decide
it.

## What this means for the proposal

Ownership is settled by precedent, not by data quality: the cluster is
city-sourced in OSM already, the two sources agree on every civic address they
share, and the City is the only one of the two that covers `664 Woodlawn Road
East`. Keeping them in the City's pipeline costs nothing and changes nothing on
the ground.

Three concrete follow-ups, none of them blocking:

- **Stop holding the 75 back.** They are already imported; continuing to fence
  them off only hides `2 Promenade Road` and `664 Woodlawn Road East` from
  gap-fill.
- **`addr:city=Guelph/Eramosa Twp` on 73 objects** is a source artifact — the
  City's `PLACE` string leaking into OSM. The township is
  `Guelph/Eramosa`; OSM's other 177 objects in the bbox say
  `Township of Guelph/Eramosa` (2012 CanVec). Worth folding into the
  `addr:province` mechanical edit, which touches the same 73 objects anyway.
- **`7667` vs `672` Speedvale Avenue East** is the one thing a survey could
  settle. Low stakes: OSM already says `672`.
