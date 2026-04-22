---
name: archaeological-drawing
description: "将器物照片、器物描述或已有草图转成符合考古报告规范的文物绘图工作流、生成提示词、出图执行和审图标准。Use when Codex needs to create, edit, critique, prompt, or generate cultural relic drawings, artifact report plates, orthographic line drawings, vessel profiles, half-section views, stone tool multi-view drawings, openwork ornament drawings, or black-and-white technical illustrations from photos or descriptions. Triggers include: 文物绘图、考古绘图、器物线图、器物剖面图、半剖面、正投影、文物照片转绘图、器物照片转绘图、cultural relic drawing, archaeological drawing, artifact illustration, orthographic artifact drawing, pottery profile drawing."
---

# Cultural Relic Drawing

Convert artifact photos, descriptions, or existing drafts into archaeological drawings that prioritize scientific record over visual drama.

Default to the highest-fidelity output the evidence supports. If the source is incomplete, lower the ambition of the drawing instead of inventing hidden structure, exact thickness, missing ornament, or a fake scale bar.

## Workflow

1. Establish the evidentiary ceiling before drawing.
   - Prefer image editing with the source artifact photo as a reference when a photo exists.
   - Distinguish `report-grade drawing` from `archaeological-style illustration`.
   - Use `report-grade drawing` only when the visible geometry supports it.
   - If the source is a single oblique or partial photo, limit the output to the visible face or visible profile rather than fabricating a full orthographic reconstruction.
   - Never invent exact dimensions, wall thickness, or section geometry from an unrelated view.
   - Use this evidence matrix:
     - Single oblique or partial photo: allow only a visible-face archaeological drawing or a cautious technical illustration.
     - Single clear side/front photo with near-orthographic view: allow one visible orthographic view of that face only.
     - Multiple clear photos of opposite sides: allow a two-side plate with matched scale and aligned baselines.
     - Multiple views plus genuine evidence for lip, break, section edge, or thickness: allow section or profile information only where that evidence exists.
     - Real dimensions from the user or trusted metadata: allow a truthful scale bar; otherwise omit it.

2. Choose the view system that matches the artifact.
   - For rotationally symmetric vessels, use the standard left-half section and right-half exterior view only when profile and thickness are actually supported by the source.
   - For asymmetrical or complex artifacts, use multiple aligned orthographic views.
   - For openwork plaques, fittings, pendants, and similar flat artifacts, use the frontal orthographic face as the main view and add side or section information only when thickness is evidenced.
   - Keep all views in orthographic alignment. Do not allow perspective, camera angle, or dramatic foreshortening.

3. Encode the drawing language as hard constraints.
   - Use black lines, dots, hatching, and white negative space only.
   - Use a fixed light logic from upper left at roughly 45 degrees.
   - Use thin lines on lit convex edges and heavier lines on shadow-side convex edges.
   - Reverse that rule for incised or recessed features.
   - Use dashed lines only for hidden, missing, or reconstructed features when there is an evidentiary basis for inference.
   - Do not use painterly shading, wash effects, photographic shadows, gradients, or decorative background.

4. Write prompts in the correct order.
   - State the non-negotiable scientific constraints first.
   - State the artifact type, view system, and orientation second.
   - State the line, point, and shading logic third.
   - State layout, delivery shape, scale-bar handling, and any required labels last.
   - Inject output conventions into the prompt itself: white background unless the user requests another plate ground, centered artifact, clean margins, legible line hierarchy at publication scale, and restrained labels.
   - Specify solid lines for visible structure, dashed lines only for hidden or reconstructed structure, and section fill or hatching only when a real section is justified.
   - If the user asks for a plate or provides multiple views or artifacts, specify a grid or aligned plate layout with consistent baselines and truthful scale handling.
   - Ban perspective, color, studio lighting, glossy reflections, speculative restoration, and decorative embellishment explicitly.

5. Review the result against the source before accepting it.
   - Check the overall contour and silhouette first. If the outer shape drifts, reject early.
   - Check the major structure next: view choice, voids, openwork, appendages, sections, and aligned baselines.
   - Check observed damage, wear, asymmetry, and missing areas next. If the result looks cleaner or more complete than the source, revise toward the source.
   - Check line logic last: hierarchy, convex-versus-concave treatment, dashed-line meaning, and whether the drawing still reads as technical rather than decorative.
   - Then review the image against the acceptance checklist in [references/standards.md](references/standards.md).

## Non-Negotiable Rules

- Treat `absolute fidelity to the original object` as the highest rule.
- Prefer orthographic projection over pictorial realism.
- Record what is visible; infer only when the inference is standard, minimal, and supported.
- Separate observed form from reconstructed form.
- Preserve breakage, asymmetry, wear, and irregularity when they are visible in the source.
- Add a scale bar only when real dimensions are known. If dimensions are unknown, omit the scale bar or reserve space for later annotation, but never fabricate numeric scale.

## View Selection

Choose the narrowest view set that the evidence supports:

- Symmetric vessels: use the conventional half-section system only when profile and thickness are genuinely supported.
- Asymmetric vessels: orient the main view to the most diagnostic side and add projected views only when needed.
- Stone tools: use front, back, and side views when morphology requires them.
- Openwork or flat ornaments: prioritize the visible face and preserve silhouette plus voids exactly.
- Sculptural objects: prefer left/right orthographic side views before expanding to more views.

Read [references/standards.md](references/standards.md) for the full view-and-layout rules, artifact-specific conventions, and the acceptance checklist.

## Prompting Guidance

When using an image model:

- Prefer reference-image editing over text-only generation.
- Tell the model to `convert`, `redraw`, or `regularize into archaeological drawing`, not to `reimagine` or `stylize`.
- Tell the model exactly which visible structures must be preserved.
- Tell the model what it must remove from the source photo, such as background cloth, labels, museum captions, glare, or cast shadow.
- Explicitly say `scientific record first, artistic expression second`.

When the first pass is weak:

- Tighten geometry constraints before adding more stylistic language.
- Ask for less shading, fewer invented details, and stricter orthographic alignment.
- Reduce the scope from `full reconstruction` to `visible-face archaeological drawing` when evidence is limited.

Read [references/prompt-templates.md](references/prompt-templates.md) when you need ready-made prompts for vessels, stone tools, openwork ornaments, or review passes.

## Execution

After the prompt is ready, execute generation instead of stopping at prompt-writing when an image model is actually available.

- Prefer the strongest image-generation model the current environment can call, especially one that supports reference-image editing and iterative revision.
- Prefer image editing from the source photos over text-only generation.
- Do not hardcode a local provider or skill. Choose the most capable available model at runtime.
- If multiple callable models exist, prefer the one that best preserves structure, contour, and damage while allowing prompt-based correction.
- Generate at least one correction pass when the first result violates the evidence ceiling, orthographic logic, or line-drawing conventions.
- If no image-generation model is callable, return a prompt package, view plan, and review checklist rather than pretending the drawing was produced.

For Codex/OpenAI environments, UI metadata may exist in `agents/openai.yaml`. Claude Code does not need that file to trigger the skill; it relies on `SKILL.md` and the frontmatter description.

## Deliverables

- Default to a single PNG on a white background unless the user explicitly asks for another format or plate style.
- If the user provides multiple views of one artifact, default to one composed plate with aligned views rather than unrelated separate outputs.
- If the user provides multiple artifacts or explicitly asks for a `plate`, compose as a grid with consistent margins, aligned baselines where appropriate, and truthful scale cues only when measurements are known.
- If the user requests separate exports, keep the main composed plate plus individual views only when that split clearly helps publication or review.
- Keep labels, numbering, and scale bars restrained and publication-oriented. Omit numeric scale when dimensions are unknown.

## Failure Modes

Avoid these common errors:

- Perspective distortion disguised as technical drawing.
- Symmetrizing an asymmetric object.
- Replacing observed damage with idealized edges.
- Inventing unseen backs, interiors, or thickness.
- Using uniform line weight everywhere.
- Rendering the image like fantasy concept art, engraving art, or decorative poster art.
- Adding a fake scale bar or fake measurement marks.
