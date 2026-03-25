# Protocol Agent Plugin

AI-powered laboratory protocol planning and reagent kit discovery.

Compatible with **Claude Code** and **OpenClaw**.

## Skills Included

### `/protocol-agent:protocol-plan`

Generate structured, safety-conscious execution plans for laboratory workflows.

**With steps provided** — produces a detailed execution plan with:
- Sub-steps with specific volumes, temperatures, and times
- Safety notes (PPE, chemical hazards, waste disposal)
- Quality checkpoints and troubleshooting
- Pause/stop points with storage conditions
- Timeline summary and materials checklist

**Without steps** — searches the web for reference protocols and presents 3–5 plan options to choose from.

**Example:**
```
/protocol-plan "RNA extraction and qPCR" --steps 'TRIzol extraction; Reverse transcription; qPCR with SYBR Green'
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
