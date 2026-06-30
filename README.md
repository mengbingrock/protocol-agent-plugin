# Protocol Agent Plugin

AI-powered laboratory protocol planning and reagent kit discovery.

Compatible with **Claude Code** and **OpenClaw**.

## Skills Included

### `/protocol-agent:protocol-plan`

Plan laboratory workflows in **two sequential modes**:

**Plan mode** (first) — produces a *high-level* plan: an ordered list of procedures / methods (e.g. RNA extraction → reverse transcription → qPCR). It drafts the method from domain knowledge, **verifies it against the curated 22-site protocol-source list** (a "toolkit search" to choose the method, not to harvest step detail), and confirms the method for each procedure with the user via **AskUserQuestion** (pick a listed method or choose "Other" to supply your own).

**Build mode** (second) — takes the confirmed plan and, **procedure by procedure**, presents reagent-kit options. You **select a kit per procedure with AskUserQuestion** (or fill in your own preferred catalog number via "Other"), and it assembles a **Bill of Materials** with verified catalog numbers and direct product links — optionally downloading kit documentation.

The Build-mode workflow is embedded as a reference (`references/build-mode.md`); to expand any single procedure into a detailed step-by-step SOP (volumes, temperatures, timings), just ask.

**Example:**
```
/protocol-plan "RNA extraction and qPCR"          # → Plan mode, then offers Build mode
/protocol-plan "RNA extraction and qPCR" --mode build
```

### `/protocol-agent:kit-finder`

Find specific reagent kit part numbers and catalog numbers for protocol steps.

- Identifies all reagents needed from protocol steps
- Asks for your preferred kits before searching
- Searches vendor websites (Thermo Fisher, QIAGEN, NEB, Bio-Rad, Sigma-Aldrich, Takara, Promega, IDT)
- Presents 2–3 options per reagent with verified catalog numbers and direct product links
- Generates a final Bill of Materials after confirmation

**Example:**
```
/kit-finder TRIzol extraction; Reverse transcription; qPCR with SYBR Green
```

## Supported Workflows

- RNA extraction (TRIzol, column-based, magnetic beads)
- Reverse transcription / cDNA synthesis
- qPCR (SYBR Green and probe-based)
- DNA/RNA purification
- Cell culture protocols
- CRISPR workflows
- And other molecular biology / biomedical procedures

## Installation

### Claude Code
```
/plugin install mengbingrock/protocol-agent-plugin
```
Skills are in `skills/` with Claude Code-specific frontmatter (`allowed-tools`, `argument-hint`).

### OpenClaw
Copy the OpenClaw-compatible skills to your OpenClaw skills directory:
```bash
cp -r openclaw/skills/protocol-plan ~/.openclaw/skills/
cp -r openclaw/skills/kit-finder ~/.openclaw/skills/
```
Skills are in `openclaw/skills/` with simplified frontmatter (no `allowed-tools` or multi-line YAML).

### Differences Between Versions

| Feature | `skills/` (Claude Code) | `openclaw/skills/` (OpenClaw) |
|---------|------------------------|-------------------------------|
| `allowed-tools` | Yes — restricts tool access | Removed — not supported |
| `argument-hint` | Yes — shows usage hint | Removed — not supported |
| Frontmatter format | Multi-line YAML | Single-line values only |
| Skill content | Identical | Identical |
| Reference data | Identical | Identical |

## Reference Data

The plugin includes curated reference materials:
- **Protocol Planning Guide** — principles of reproducibility, safety-first design, time management, and QC checkpoints
- **Medical Domain Knowledge** — gene expression analysis, RT-qPCR best practices, sample handling, troubleshooting
- **Vendor Catalog Reference** — common kits with verified catalog numbers, vendor URLs, and product specifications

## License

MIT
