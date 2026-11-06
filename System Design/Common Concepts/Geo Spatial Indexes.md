### When to use which?
Ask these questions in order. Stop at the first clear answer.

**1. Am I storing shapes or points?**
- Shapes (zones, roads, boundaries) → **R-tree** (use PostGIS)
- Points (restaurants, drivers, users) → go to question 2

**2. Do the points move often?**
- Yes, every few seconds (drivers, delivery partners) → **Geohash**
- No, they rarely change (restaurants, hotels, shops) → go to question 3

**3. Is the density very uneven?**
- Yes, busy cities and empty rural areas in the same system → **Quadtree**
- No, fairly even, or just one city → **Geohash**

**4. Do I need it in a database or cache I already use?**
- Using Redis or need easy sharding by region → **Geohash**
- Already on Postgres → **PostGIS (R-tree)** handles points too, so no custom index is needed

---

**As a quick flow:**

```
Shapes?          → R-tree
Moving points?   → Geohash
Uneven density?  → Quadtree
Otherwise        → Geohash (simplest)
```

**Practice with examples:**

| System                       | Answer path              | Pick     |
| ---------------------------- | ------------------------ | -------- |
| Swiggy "do we deliver here?" | Shapes                   | R-tree   |
| Uber nearby drivers          | Points → moving          | Geohash  |
| Yelp nearby restaurants      | Points → static → uneven | Quadtree |
| Find friends nearby          | Points → moving          | Geohash  |
| Hotels in one city           | Points → static → even   | Geohash  |
|                              |                          |          |

**One habit for interviews:** say your question out loud, like "Restaurants don't move, but density varies a lot, so I'll pick a quadtree." It shows your reasoning, not just the answer.

### What are they?

##### Geo hash
Converts a location to a string:

![[Pasted image 20261012185820.png|737]]

We case base32/64 for the geo hash value.
Now when we have these, we can use b-trees on top of these string to apply our range queries. Most nearby locations will have the same prefix.
*Problem*: nearby locations might have different prefixes in case they reside on a different division. But generally this is not a problem.
[[Redis]] uses this.

##### Quad trees
- Less common for prod systems these days.

![[Pasted image 20261012195338.png|481]]

Keep in creating children if the number of points present in an area is more than the threshold.
For proximity queries: go up below the current level.

##### R-trees
Allows overlapping rectangles.
Not rigid as the above implementations.

![[Pasted image 20261012195820.png|584]]

- R-trees can efficiently handle both points and larger shapes in the same index structure. Quadtrees struggle with this mixed data - they keep subdividing until they can isolate each shape, leading to deeper trees and more complex traversal.
- The *trade-off* for this flexibility is that overlapping rectangles sometimes force us to search multiple branches of the tree. Modern R-tree implementations use smart algorithms to balance this overlap against tree depth, tuning for how databases actually read data from disk. This balance of flexibility and disk efficiency is why R-trees have become the standard choice for production spatial indexes.

### Use Cases


**1. Geohash — Redis GEO (used in food delivery, ride apps)**

**The story:** You open a food app. It wants restaurants near you.

- Your location becomes a string like `tdr1y`.
- Every restaurant also has a string.
- Restaurants near you share the same start, like `tdr1...`.
- So "find nearby" becomes "find strings starting with `tdr1`."

**Real use:** Redis's `GEOADD` and `GEOSEARCH` commands use geohash internally. Many apps store locations in Redis this way.

**Why geohash here:** it's just a string. Strings are easy to store, cache, and split across servers.

**Close cousin:** Uber uses **H3**, which is the same idea but with hexagons instead of squares.

---

**2. Quadtree — map tiles (Bing Maps, Google Maps)**

**The story:** You zoom into a map.

- At zoom 1, the whole world is 1 square.
- Zoom in, and it splits into 4 squares.
- Zoom in again, and each of those splits into 4.
- The app only loads the squares you're looking at.

**Real use:** Bing Maps names its tiles with "quadkeys," and each extra digit means one more split into 4. This is a quadtree.

**For nearby search (Yelp-style):** Bangalore's Koramangala has thousands of restaurants, so its square splits many times. A rural area has a few, so its square stays big. Every square ends up with roughly the same number of places.

**Why quadtree here:** it adapts. Busy areas get small squares, empty areas get big ones.

---

**3. R-tree — delivery zones (Swiggy, Zomato, Amazon)**

**The story:** You enter your address. The app checks, "Do we deliver here?"

- Each delivery zone is a **shape** (an irregular polygon), not a point.
- The R-tree wraps each shape in a box, then groups nearby boxes into bigger boxes.
- To check your address, it skips every box you're not inside and only checks the few that matter.

**Real use:** PostGIS (Postgres), SQLite, and Oracle Spatial all use R-tree-style indexes. Any app storing zones, roads, or city boundaries in these databases relies on it.

**Why R-tree here:** geohash and quadtree are great for points. R-tree handles shapes.

---

**Summary**

|Index|Real system|What it finds|
|---|---|---|
|Geohash|Redis GEO, Uber H3|Points near me (cheap, simple)|
|Quadtree|Map tiles, Yelp-style search|Points, with uneven density|
|R-tree|PostGIS, delivery zones|Shapes, "am I inside this area?"|

**Easy way to remember:**

- Geohash → **string** (easy to store)
- Quadtree → **map zoom** (splits into 4)
- R-tree → **boxes around shapes**