# Build Mode Workflow (second mode of protocol-plan)

This document is loaded by `protocol-plan` when it enters **Build mode** — after a
high-level plan has been confirmed in Plan mode (or when the user comes in with a
purely procurement request). Build mode turns the confirmed plan into a **Bill of
Materials** by letting the user pick a reagent kit for each procedure. It was
previously a standalone `kit-finder` skill and was merged in.

Enter Build mode whenever:
- A confirmed Plan-mode plan exists and the user says "build it" / "find the kits" / "make the BOM".
- The user passes `--mode build`.
- The request is purely procurement (e.g. "find catalog numbers for: TRIzol, RT master mix, SYBR mix") — treat the listed reagents as a one-procedure plan.

The driving idea: **walk the confirmed plan procedure by procedure, and for each
kit ask the user to choose with `AskUserQuestion` — letting them pick a found
option or fill in their own preference via "Other".**

## Workflow

### Phase 1 — Load the confirmed plan

1. Read the confirmed high-level plan (the ordered procedures + chosen methods from Plan mode). If the user supplied a bare reagent list, treat it as the plan.
2. Read `references/vendor-catalog-reference.md` (same skill folder) for known catalog numbers.
3. Read any protocol/plan files in the project directory (`.md`, especially under `.claude/plans/`) for reagents already mentioned or lab-preferred.

### Phase 2 — Extract kits per procedure

For **each procedure** in the plan, identify the reagents/kits its chosen method requires:
- Primary kit or reagent (e.g. "TRIzol Reagent", "SYBR Green Master Mix")
- Supporting reagents (e.g. chloroform, ethanol, RNase-free water)
- Consumables with specific requirements (e.g. optical plates, filter tips)

Keep the list grouped by procedure — that grouping drives the questions in Phase 4.

### Phase 3 — Search for options

For each kit the user did **not** already specify, find **2–4 real options** (from different vendors where possible). Priority domains:

- `https://www.cell.com/star-protocols/home`
- `https://www.nature.com/nprot/`
- `thermofisher.com`
- `qiagen.com`
- `neb.com`
- `bio-rad.com`
- `sigmaaldrich.com` / `emdmillipore.com`
- `takarabio.com`
- `promega.com`
- `idtdna.com` (primers/oligos)

If the user passed `--vendor`, prioritize that domain. Use targeted queries —
`"[reagent name]" site:[vendor-domain] catalog number`. For each option collect:

- **Product name** (exact, as listed by the vendor)
- **Catalog number** (specific — never a product-family code)
- **Vendor name**
- **Direct product URL** (the specific product page, not a search result)
- **Pack size / unit** (e.g. "500 mL", "200 rxns", "100 µg")
- **Key specifications** (concentration, format, compatibility)

Verify links with `WebFetch` where possible: catalog # matches the page, product not discontinued, pack size and specs correct. **Never guess a catalog number** — if you cannot verify one, say so and provide the best product-page link found.

### Phase 4 — Select each kit with AskUserQuestion

Walk the plan **procedure by procedure**. For each kit, ask the user to choose using the **`AskUserQuestion` tool** — **one question per kit**:

- `header`: a short chip for the kit (e.g. "Lysis reagent", "RT kit", "qPCR mix").
- `question`: which product to use for that reagent.
- `options` (2–4): each `label` = product + Cat #, each `description` = vendor · pack size · one-line trade-off (most-cited / all-in-one / best value / same-vendor compatibility). Put the **recommended** option first and suffix its label with "(Recommended)".
- The tool's automatic **"Other"** is how the user **fills in their own preferred product or catalog number** — always remind them it is available.
- Batch up to **4 kit questions per call**; make more calls until every kit in every procedure has a selection.

Prefer cross-procedure compatibility (e.g. an RT kit and a qPCR master mix validated together, or from the same vendor) and surface that in the recommended option's description.

### Phase 5 — Assemble the Bill of Materials

After all selections are in, produce the BOM:

```
# Bill of Materials: [Plan / Task Name]

**Date:** [current date]
**Confirmed by:** User (via AskUserQuestion)

| # | Procedure | Product Name | Catalog # | Vendor | Pack Size | Product Link | Notes |
|---|-----------|--------------|-----------|--------|-----------|--------------|-------|
| 1 | [Proc 1] | [Exact product name] | [Cat#] | [Vendor] | [Size] | [URL] | [Notes] |
| 2 | ... | ... | ... | ... | ... | ... | ... |

## Ordering Notes
- [Special ordering instructions — e.g. "Ships on dry ice", "Requires hazmat shipping"]
- [Storage requirements upon receipt]
- [Items likely already in common lab stock — ethanol, isopropanol, water]
```

### Phase 6 — Download kit documentation (optional)

After the BOM is finalized, offer to fetch product docs:

1. For each confirmed kit, search for its documentation:
   - Query: `"[product name]" "[catalog number]" manual PDF site:[vendor-domain]`
   - Look for: product manual, user guide, protocol sheet, technical data sheet. Also check the product page for download links.
2. Save each PDF (or the product page as markdown if no PDF) to `references/kit-docs/` in the project workspace:
   - PDF filename: `[Vendor]_[ProductName]_[CatalogNumber].pdf` (spaces → hyphens)
   - Example: `ThermoFisher_TRIzol-Reagent_15596026.pdf`
   - Fallback: `[Vendor]_[ProductName]_[CatalogNumber]_product-page.md`
3. Report:

```
# Kit Documentation Downloaded

| # | Product | Catalog # | File | Status |
|---|---------|-----------|------|--------|
| 1 | [Product] | [Cat#] | [filename] | ✅ Downloaded |
| 2 | [Product] | [Cat#] | [filename] | ⚠️ PDF not found — product page saved |
| 3 | [Product] | [Cat#] | — | ❌ No documentation found |

Files saved to: `references/kit-docs/`
```

## Important Guidelines

- **One AskUserQuestion per kit** — the user selects an option or chooses "Other" to enter their own product/catalog number.
- **Catalog numbers must be specific** — never product-family codes. Use `4472908` (SYBR Select Master Mix, 10 × 5 mL), not the family page.
- **Links must point to the specific product page** — not search results, not family overviews; the URL should contain or directly lead to the catalog number.
- **Distinguish pack sizes** — the same product often has multiple Cat #s (50 rxns vs. 200 rxns). Default to common lab scale unless the user specifies.
- **Flag discontinued products** and suggest replacements.
- **Note kit contents** — list what's bundled so the user knows what they don't need to order separately.
- **Consider compatibility** across procedures — prefer products validated together or from the same vendor.
- **Common lab stock items** — for commodities (ethanol, isopropanol, chloroform, water), still give Cat #s but note they may already be on the shelf.
- **Do not guess catalog numbers** — if you cannot verify one through web search, say so and provide the best product-page link found.
- **Always include web references** with direct URLs.
