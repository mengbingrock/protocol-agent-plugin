# Curated Protocol Source Websites

A vetted list of 22 websites to consult during **Plan mode** when verifying the high-level method (which procedures are standard, which method variants exist) — not to harvest step-by-step detail. Prefer these over generic web searches. Each searchable site below has a verified search-URL template — replace `{query}` with a URL-encoded keyword.

> Last verified: 2026-05-22. Sites flagged `[bot-blocked]` return HTTP 403 to plain `curl` but work in a real browser — use `WebFetch` (which uses a real user agent) when searching them programmatically.

## A. Searchable protocol & methods databases

| # | Site | Homepage | Search URL template |
|---|------|----------|---------------------|
| 1 | Cold Spring Harbor Protocols | https://cshprotocols.cshlp.org/ | `https://cshprotocols.cshlp.org/search?fulltext={query}&submit=yes` |
| 5 | Bio-protocol | https://bio-protocol.org/en | `https://bio-protocol.org/searchsite.aspx?word={query}` |
| 17 | Nature Protocols | https://www.nature.com/nprot | `https://www.nature.com/search?q={query}&journal=nprot` |
| 18 | JOVE | https://www.jove.com/cn/ | `https://www.jove.com/search?query={query}` |
| 19 | protocols.io *(replaces the discontinued Nature Protocol Exchange)* | https://www.protocols.io/ | `https://www.nature.com/search?q={query}&type=protocolexchange` (legacy content via Nature) — protocols.io's own search requires the SPA; for new submissions browse directly |
| 20 | Current Protocols (Wiley) `[bot-blocked]` | https://currentprotocols.onlinelibrary.wiley.com/ | `https://currentprotocols.onlinelibrary.wiley.com/action/doSearch?AllField={query}` |
| 22 | Springer Nature Experiments | https://experiments.springernature.com/ | `https://experiments.springernature.com/search?query={query}` |

> Note on #19: `protocolexchange.researchsquare.com` now 301-redirects to protocols.io; the standalone Nature Protocol Exchange is discontinued. Legacy content is still indexed under Nature's site search.

## B. Searchable literature & analysis tools

| # | Site | Homepage | Search URL template |
|---|------|----------|---------------------|
| 2 | CiteXs (塞特新思) | https://www.citexs.com/Paperpicky | `https://www.citexs.com/Paperpicky?keyword={query}` |
| 3 | Connected Papers | https://www.connectedpapers.com/ | `https://www.connectedpapers.com/search?q={query}` *(seed is typically a paper title or DOI)* |
| 4 | Paper Digest | https://www.paperdigest.org/ | `https://www.paperdigest.org/search/?topic={query}` |
| 12 | Elsevier Journal Finder | https://journalfinder.elsevier.com/ | `https://journalfinder.elsevier.com/results?paperTitle={query}` *(query = abstract or title)* |

## C. Searchable gene & expression databases

| # | Site | Homepage | Search URL template |
|---|------|----------|---------------------|
| 13 | ArrayExpress (EBI BioStudies) | https://www.ebi.ac.uk/biostudies/arrayexpress | `https://www.ebi.ac.uk/biostudies/arrayexpress/studies?query={query}` |
| 14 | Ensembl | https://www.ensembl.org/ | `https://www.ensembl.org/Multi/Search/Results?q={query}` *(mirror-redirect may rate-limit/404 transient — retry or use `useast.ensembl.org` / `asia.ensembl.org` directly)* |
| 15 | GeneCards `[bot-blocked]` | https://www.genecards.org/ | `https://www.genecards.org/Search/Keyword?queryString={query}` |
| 16 | R Graph Gallery | https://r-graph-gallery.com/ | `https://r-graph-gallery.com/?s={query}` |

## D. Tools & platforms (browse directly — no keyword search)

These provide on-site tools or templates rather than a searchable corpus. Don't try to URL-search them; link to the homepage and let the user pick a tool.

| # | Site | URL | Use for |
|---|------|-----|---------|
| 6 | Wordvice AI | https://wordvice.ai/cn | AI grammar/spelling check for academic English |
| 7 | BioGDP | https://biogdp.com/ | Diagram templates (mechanism, cell structure, pathway). Client-side SPA — no deep links |
| 8 | BioInCloud (微科盟) | https://www.bioincloud.tech/ | 65 visualization tools (scatter, bar, bubble, violin, network, heatmap) |
| 9 | OmicShare Tools | https://www.omicshare.com/tools/ | Tool index browsed by category (`/tools/Home/Index/index/type/{category}`), not by keyword |
| 10 | Benchling | https://www.benchling.com/ | DNA/protein design, CRISPR, plasmid maps — login-gated SaaS |
| 11 | Meinverse (觅应) | https://meinverse.cn/ | Sequence visualization, protein rendering, primer design |
| 21 | Morimoto Lab | https://www.morimotolab.org/protocols | Single page of curated cell-culture / WB / PCR protocols — read linearly |

## Plan-mode search order (toolkit search for method selection)

1. **For specific lab techniques** (WB, qPCR, RNA extraction, IF, cell culture, CRISPR): start with **Bio-protocol**, **Current Protocols**, **Cold Spring Harbor Protocols**, **Nature Protocols**, **JOVE**.
2. **For video / visual learning needs**: **JOVE**, then **BioGDP** for schematic diagrams.
3. **For novel or niche techniques**: **protocols.io**, **Springer Nature Experiments**, **Morimoto Lab**.
4. **For literature context before picking a protocol**: **Connected Papers**, **Paper Digest**, **CiteXs**.
5. **For sequence / primer design tasks within a protocol**: **Benchling**, **Meinverse**, **Ensembl**, **GeneCards**.
6. **For data-analysis or plotting follow-up**: **OmicShare Tools**, **BioInCloud**, **R Graph Gallery**, **ArrayExpress**.
7. **For manuscript writing after the experiment**: **Wordvice AI**, **Elsevier Journal Finder**.

When using a search-URL template:
- URL-encode the query before substitution (`PCR primer design` → `PCR%20primer%20design`).
- Bot-blocked sites (`[bot-blocked]`) require `WebFetch` rather than raw `curl`.
- Always include the resolved URL in the final plan's **References** section.
