[README.md](https://github.com/user-attachments/files/30153356/README.md)

# Case Study: Cleaning a Messy Nutrition Dataset

**The problem:** Public and self-collected nutrition data almost always looks
like this — the same food entered multiple times under slightly different
names, nutrient values mixed between units (grams written as "6g", "6.0",
or bare "6"; sodium split between mg and g), inconsistent serving sizes,
and missing values with no flag to say so. This is exactly the shape of
data you get from combining sources like USDA FoodData Central exports,
OpenFoodFacts pulls, or years of manual spreadsheet entry by different
staff.

Left messy, this kind of dataset can't safely power a product catalog,
a client-facing nutrition guide, or a recommendation feature — duplicate
or inconsistent entries produce wrong totals, and unit mismatches produce
numbers that are quietly wrong rather than obviously broken.

**The approach:**
1. **Standardize naming** — collapse casing, punctuation, and phrasing
   variants ("Almonds Raw", "almonds, raw", "ALMONDS (RAW)") into one
   canonical name per food.
2. **Standardize units** — convert every nutrient column to a single
   consistent unit (grams for macros, milligrams for sodium), so a "6"
   and a "6g" are recognized as the same value.
3. **Normalize serving sizes** — convert servings recorded in ounces,
   cups, or grams into a single comparable basis where possible.
4. **Flag rather than guess** — rows with missing nutrient data are
   marked `needs_review = True` instead of silently dropped or filled
   with a guessed value. Assumptions about missing health data are the
   one place automation shouldn't quietly decide for you.
5. **De-duplicate intelligently** — when the same food appears multiple
   times, keep the most complete record rather than just the first or
   last one seen.

**The result:**

| Metric | Before | After |
|---|---|---|
| Rows | 22 | 12 |
| Duplicate/variant entries for the same food | 10 | 0 |
| Inconsistent units | Mixed (g, mg, bare numbers) | Standardized |
| Missing data | Untracked | Flagged for review (2 rows) |

Two rows remain flagged rather than force-merged — one has a missing
serving size, one has a missing protein value. That's intentional: a
cleanup process that quietly guesses at missing health data is worse
than one that hands you a short, clear list of what still needs a human
look. Automated cleaning gets you 90% of the way; the flagged list is
what makes the last 10% safe to hand off.

**Files in this case study:**
- `raw_nutrition_data.csv` — the original messy sample data
- `clean_data.py` — the Python/pandas script that performs the cleanup
- `cleaned_nutrition_data.csv` — the standardized output
- `README.md` — this write-up

**Note on data:** this dataset is a representative sample built to
mirror the real, well-documented inconsistencies found in public
nutrition data sources (USDA FoodData Central, OpenFoodFacts). No
real client or patient data is used — this kind of cleanup applies
to product/ingredient data, not personal health records.

---

*This is the kind of work I do for small nutrition brands, health food
stores, and independent practitioners who need their product or
ingredient data cleaned, standardized, and ready to use — whether
that's for a website, a catalog, or a client-facing resource.*
