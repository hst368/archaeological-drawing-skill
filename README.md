# archaeological-drawing

A reusable AI skill for turning artifact photos, descriptions, or rough drafts into evidence-bounded cultural relic drawing workflows, prompt packages, execution guidance, and review standards.

This skill is built for archaeological drawing logic rather than generic "line art" styling. It prioritizes orthographic projection, evidence limits, explicit risk disclosure, and reviewable workflow over one-shot visual polish.

## What It Does

- Classifies source quality before drawing with a `Source Conditioning / Calibration Gate`
- Separates outputs into explicit tiers such as `report-grade candidate`, `evidence-bounded AI technical draft`, and `archaeological-style illustration`
- Uses three workflow modes:
  - `main-structure pass`
  - `high-risk local pass`
  - `review-and-merge pass`
- Supports `Preprocessing / Region Triage` before generation
- Provides prompt templates, drawing standards, and acceptance checks for archaeological-style output
- Forces a closing `Risk note` so uncertain areas are disclosed rather than hidden

## What It Is Good For

At its current maturity, this skill is best suited for:

- Fast standardized draft drawing of a single artifact
- Low- to medium-complexity artifacts
- Evidence-bounded redraws where a human is actively reviewing the result during the same turn
- Converting a photo into a more disciplined archaeological-style technical draft
- Planning view systems, review passes, and correction strategy even when the final drawing still needs human cleanup

## What It Is Not Good For

At its current maturity, this skill is not yet a safe replacement for human archaeological illustrators in these cases:

- Batch production of complex gold objects, dense bronze ornament, or similarly high-risk material
- "No human review" scenarios involving inscriptions, damaged characters, seals, monograms, stamped marks, or fine chased/engraved detail
- Situations where a single generation is expected to be submission-ready without manual correction
- Full orthographic reconstruction from one oblique photo
- Automatic publication-grade output from weak, partial, blurry, reflective, or contradictory evidence

## Current Capability Boundary

The current version is best understood as an evidence-constrained workflow skill, not as a guaranteed report-grade image generator.

- Strongest current value:
  - preventing overclaiming
  - narrowing view choice to what the evidence supports
  - separating global structure from high-risk local detail
  - making uncertainty explicit
- Current ceiling:
  - usually `Tier 2: evidence-bounded AI technical draft`
  - sometimes `Tier 1.5: report-grade candidate` when evidence is genuinely strong
- Current limitation:
  - complex objects still need human checking, cleanup, and sometimes manual redrawing

## Current Backend Reality

- This repository does not include a trainable image-generation backend.
- If your available image backend is only a hosted general model, this skill should be used as workflow control and review discipline, not as a built-in LoRA training system.
- `LoRA Readiness` is included as future-facing guidance for paired-data preparation, not as a promise that this repository can train or run LoRA by itself.

## Repository Layout

```text
archaeological-drawing/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
└── references/
    ├── prompt-templates.md
    └── standards.md
```

## Main Concepts

### 1. Source Conditioning / Calibration Gate

Before drawing, classify the source:

- `Measured object + calibrated multi-view`
- `Near-orthographic view with scale reference`
- `Multi-view but uncalibrated`
- `Single oblique photo`
- `Text-only description`

This gate decides the strongest defensible output claim.

### 2. Output Tiers

The skill distinguishes:

- `Tier 1: report-grade measured drawing`
- `Tier 1.5: report-grade candidate`
- `Tier 2: evidence-bounded AI technical draft`
- `Tier 3: archaeological-style illustration`

The default practical target for most photo-based work is `Tier 2`.

### 3. Workflow Modes

- `main-structure pass`
  - locks contour, projection, and broad structure
- `high-risk local pass`
  - isolates scripts, dense ornament, glare-heavy edges, break lines, and other ambiguity hotspots
- `review-and-merge pass`
  - merges validated local corrections and issues the final `Risk note`

## Repository Files

- [SKILL.md](./SKILL.md)
  - the main workflow and execution contract
- [references/standards.md](./references/standards.md)
  - archaeological drawing standards and acceptance checklist
- [references/prompt-templates.md](./references/prompt-templates.md)
  - composable prompt blocks and pass-specific templates

## Install

### Codex

Copy or symlink this folder into:

```text
~/.codex/skills/archaeological-drawing
```

### Claude Code

Copy or symlink this folder into:

```text
~/.claude/skills/archaeological-drawing
```

Claude Code mainly relies on `SKILL.md` and the frontmatter description. `agents/openai.yaml` is included for Codex/OpenAI-side UI metadata.

## Example Invocation

```text
Use $archaeological-drawing to convert this artifact photo into an evidence-bounded archaeological drawing. Only draw what the evidence supports. Do not invent sections, thickness, hidden structure, unreadable inscription content, or a scale bar.
```

## Notes

- The skill prefers the strongest available image-generation model that supports reference-image editing and iterative correction.
- If no image-generation model is callable, the skill should still return a prompt package, evidence-bounded view plan, and review checklist.
- The skill is designed to reduce hallucination and overclaiming, not to guarantee zero-error output.

## License

MIT
