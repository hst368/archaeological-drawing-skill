# archaeological-drawing

A reusable AI skill for turning artifact photos, descriptions, or rough drafts into rigorous archaeological drawing workflows, generation prompts, execution guidance, and review standards.

The skill is designed around orthographic projection, evidence-bounded reconstruction, and archaeological publication conventions rather than generic "line art" styling.

## What It Does

- Converts artifact photos into archaeological drawing plans and prompts
- Enforces evidence limits to avoid invented sections, thickness, or fake scale bars
- Guides view selection for vessels, stone tools, openwork ornaments, and sculptural artifacts
- Provides drawing standards for contour, line weight, dot rendering, and hidden/reconstructed features
- Supports review and correction passes after image generation

## Repository Layout

```text
archaeological-drawing/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── prompt-templates.md
    └── standards.md
```

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
Use $archaeological-drawing to convert this artifact photo into a rigorous archaeological drawing. Only draw what the evidence supports. Do not invent sections, thickness, hidden structure, or a scale bar.
```

## Notes

- The skill prefers the strongest image-generation model available in the current environment, especially models that support reference-image editing.
- If no image-generation model is callable, the skill still returns a complete prompt package, evidence-bounded view plan, and review checklist.

## License

MIT
