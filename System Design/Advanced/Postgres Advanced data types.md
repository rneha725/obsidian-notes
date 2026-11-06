## GIN indexes
Generalized Inverted Indexes: Postgres creates these indexes for searches in arrays, full-text, jsonb. Like the name suggests, these are invesrted indexes, pointing to a list of rows.
Postgres also provides, ranking, weightage to the fields in the queries for ranks.

Think of a JSON object, an array, or a paragraph of text — each row's value isn't one thing, it's many things packed together. B-tree can't search "inside" that bundle. GIN can, because it works like the index at the back of a textbook: instead of "page 5 talks about X, Y, Z," it stores "X appears on pages 5, 12, 47" — one entry per individual item, pointing back to every row that has it. So asking "which rows contain X?" becomes a direct lookup instead of opening every row's bundle one by one.

Use it when

- The column is JSON/JSONB, an array, or free text you search by word.
- Your query asks "does this row _contain_ X?" — not "does it equal X?"

Skip it when

- The column is just one plain value (a number, a status, a date) — B-tree is faster and cheaper here, no reason to reach for GIN.
- That column is written/updated constantly — GIN makes every write costlier, since one row can touch many index entries at once.
- Almost nobody queries that column — you'd be paying the write cost for a lookup speed nobody uses.


## GIS indexes

Numbers and dates sort cleanly — 1, 2, 3. But shapes, map locations, and date ranges don't: two ranges can overlap, two shapes can sit next to each other, and there's no single "bigger or smaller" order to sort them by. GiST handles that by grouping nearby things into rough boxes ("everything in this box is roughly around here"), and when you search, it finds which boxes could hold a match, then double-checks each real candidate to be sure — because the boxes are only an approximation.

Use it when

- You're storing locations, shapes, or ranges (dates, numbers-as-ranges).
- You need "does this overlap/is this near that?" — nearest-café, does-this-booking-clash.
- You want Postgres itself to block overlaps — an exclusion constraint, e.g. "no two bookings for this room can overlap."

Skip it when

- You just need plain equality — B-tree.
- You need JSON/array containment — GIN is the better fit, usually faster for that case.
