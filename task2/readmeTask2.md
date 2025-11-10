# Task 2: Calculating Nutritive Values — Approach

> This problem-solving explanation only applies to `task2_second_approach.ipynb` 

Use the **UR Nährwertrechner** API:

```
POST https://smarthome.uni-regensburg.de/naehrwertrechner/api/1.0/recipe_info_optifast
Body: {"recipe": "<amount> <unit> <ingredient>"}
```

## Inputs (from Task 1)

* `norm_value` (e.g., `190.0`)
* `norm_unit` ∈ `{g, liter, stück}`
* `ingredient` (text)
* `amount_annotation` (JSON, e.g. `{"zutat":"Traube","eigenschaft":"kernlos"}`)

## Prompt construction

* Prefer structured name from annotation:
  `ingredient_text = "<zutat> <eigenschaft>"` (e.g., `Traube kernlos`).
* Fallback to `ingredient` if annotation is missing.
* Prompt format:
  `"<norm_value> <norm_unit> <ingredient_text>"`

## Response validation

1. **Unrecognized**: missing name or name starts with `Nicht erkannt` → mark as unrecognized.
2. **Name match**: robust comparison between our text and API `bezeichnung`

   * Unicode-safe normalization (keeps `ä/ö/ü/ß`)
   * Diacritic/ASCII fallbacks (`Hähnchen` ≈ `Hahnchen`/`Haehnchen`)
   * Optional soft regex/token fallback to ignore fillers (e.g., colors like `grün`)
3. **Nutrition present**: accept if `GCAL > 0` (water-like items are accepted even if `GCAL == 0`).

## Retry policy (minimal but effective)

* **Attempt 1:** annotated name (`zutat + eigenschaft` when available).
* **Attempt 2:** *only if recognized but no kcal* → retry with plain `ingredient`.
* **Attempt 3 (unit fix):** if unit is **`stück` or `liter`** and last status ∈ `{unrecognized, mismatch, no_nutrition}`, retry with **`g`** (same amount; prefer annotated name).

## Logging & outputs

* For **`Nicht erkannt`**, log the **full raw JSON** response (plus prompt) to a JSONL file (e.g., `nicht_erkannt.jsonl`).
* Show progress with **tqdm**.
* Write two CSVs:

  * **`nutri_found_2ndApproach.csv`** — successful rows (`status == "ok"`), includes `nutrition` (full API JSON), `prompt_used`, `recognized_name`.
  * **`nutri_failed_2ndApproach.csv`** — all other rows, includes `status`, `prompt_used`, optional `recognized_name`.

### Example JSONL log entry

```json
{"prompt":"6.0 stück Traube kernlos","response":{ ... full API JSON ... }}
```

## Notes

* Units from Task 1 are kept as-is; only the targeted **stück/liter → g** fallback is applied.
* The matcher tolerates umlauts, spacing, and minor morphology (e.g., `trauben grün kernlos` vs `trauben kernlos`).


