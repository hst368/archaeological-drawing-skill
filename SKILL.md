---
name: archaeological-drawing
description: "将器物照片、器物描述或已有草图转成符合考古报告规范的文物绘图工作流、生成提示词、出图执行和审图标准。Use when Codex needs to create, edit, critique, prompt, or generate cultural relic drawings, artifact report plates, orthographic line drawings, vessel profiles, half-section views, stone tool multi-view drawings, openwork ornament drawings, or black-and-white technical illustrations from photos or descriptions. Triggers include: 文物绘图、考古绘图、器物线图、器物剖面图、半剖面、正投影、文物照片转绘图、器物照片转绘图、cultural relic drawing, archaeological drawing, artifact illustration, orthographic artifact drawing, pottery profile drawing."
---

# Cultural Relic Drawing

Convert artifact photos, descriptions, or existing drafts into archaeological drawings that prioritize scientific record over visual drama.

Default to the highest-fidelity output the evidence supports. If the source is incomplete, lower the ambition of the drawing instead of inventing hidden structure, exact thickness, missing ornament, or a fake scale bar.

## Source Conditioning / Calibration Gate

Classify the source before planning the drawing. This gate decides the maximum defensible output tier.

- `Measured object + calibrated multi-view`
  - Source includes real measurements or trustworthy calibration plus multiple reliable views.
  - Eligible for `report-grade candidate`.
  - Final `report-grade measured drawing` still requires human review, proportion checking, and publication preparation.
- `Near-orthographic view with scale reference`
  - Source includes a scale or clearly trustworthy size reference and a near-orthographic face or profile.
  - Eligible for a single-face orthographic drawing and possibly `report-grade candidate` of that face only.
  - Do not extend report-grade claims to unseen sides, thickness, or unsupported sections.
- `Multi-view but uncalibrated`
  - Source includes several views, but no reliable calibration or measurement basis.
  - Eligible only for `evidence-bounded AI technical draft`.
  - Never present this directly as `report-grade measured drawing`.
- `Single oblique photo`
  - Source is one angled or partial photograph without trustworthy calibration.
  - Eligible only for `visible-face redraw` within `evidence-bounded AI technical draft`.
  - Never reconstruct a full orthographic object record from this alone.
- `Text-only description`
  - Source is verbal description with no reliable image evidence.
  - Eligible only for `view plan`, `prompt package`, or `schematic concept`.
  - Never present the result as a normative archaeological drawing.

If more than one class seems plausible, choose the weaker class unless calibration and geometry support the stronger claim unambiguously.

## Output Tiers

State or internally determine the output tier before prompt-writing.

- `Tier 1: report-grade measured drawing`
  - Reserved for measured or calibrated evidence plus human review.
  - AI may assist with drafting, cleanup, or layout, but this is not a one-pass pure generation output.
- `Tier 1.5: report-grade candidate`
  - Allowed when evidence is strong but not yet fully measured, calibrated, or publication-finalized.
  - Must state what is still missing, such as calibration, contour verification, line cleanup, or publication preparation.
- `Tier 2: evidence-bounded AI technical draft`
  - Default tier for most photo-based requests.
  - Intended for draft production, review, correction, vectorization, and later publication preparation.
- `Tier 3: archaeological-style illustration`
  - Allowed when evidence is weak, schematic, or primarily communicative.
  - Must not be presented as a report drawing.

## Workflow Modes

These modes sit underneath the evidence gate and output tier. They control execution only; they never upgrade the allowed output claim.

- `main-structure pass`
  - Responsible for overall contour, major structure, projection, baseline symmetry or asymmetry, and broad ornament zoning.
  - Must intentionally suppress or defer high-risk local detail such as scripts, dense repeated ornament, glare-heavy edges, openwork crossings, and damaged boundaries.
- `high-risk local pass`
  - Responsible only for isolated high-risk regions, such as scripts or symbols, dense ornament clusters, openwork intersections, reflective edges, break lines, and other ambiguity hotspots.
  - Must not alter the global contour or re-decide the overall projection.
  - Must not imply a separate exported local image unless the user explicitly asks for one.
- `review-and-merge pass`
  - Responsible for reconciling the main structure draft with local corrections, demoting unsupported detail, and issuing the final `Risk note`.
  - Must decide whether weak local detail is merged, simplified, omitted, or explicitly downgraded.

Relationship rules:

- The `Source Conditioning / Calibration Gate` decides the maximum defensible claim.
- The `Output Tier` decides how authoritative the deliverable can be.
- The `Workflow Mode` decides how execution proceeds.
- Workflow modes can make draft production safer and more auditable, but they cannot strengthen evidence or upgrade the output tier.

## Preprocessing / Region Triage

Run this triage before `main-structure pass`. It is not optional.

- Choose one `primary geometry reference`.
  - Use it to lock contour, projection, and the main face or profile.
- Choose any `secondary detail references`.
  - Use them only to confirm local detail that is compatible with the same observed structure.
- Crop or mentally isolate the relevant object, face, or local area before prompting when the source includes excess background, glare zones, or unrelated annotation.
- Name the high-risk regions before generation.
  - Typical categories: scripts or symbols, dense repeated ornament, openwork crossings, reflective edges, damaged or missing boundaries, severe overlap or occlusion.
- Decide whether the task should run as:
  - whole-object or whole-face drawing only
  - high-risk local handling only
  - split workflow with both
- Demote or ignore low-value inputs instead of averaging them into the result.
  - Typical demotion cases: blur, heavy glare, bad perspective, strong occlusion, shallow depth of field, or poor local focus.

Minimum triage outputs:

- `source class`
- `output tier`
- `primary geometry reference`
- `secondary detail references`
- `named high-risk regions`
- `selected mode sequence`

If the triage is weak or contradictory, lower the ambition of the drawing before writing prompts.

## Mode I/O Contract

Treat each workflow mode as a contract with clear inputs, outputs, and prohibitions.

- `main-structure pass`
  - Input:
    - `source class`
    - `output tier`
    - `primary geometry reference`
    - object or face scope
  - Output:
    - stable global contour
    - projection and baseline
    - major structure and broad ornament zoning
    - unresolved local-risk list
  - Must defer:
    - scripts or symbols
    - dense ornament micro-topology
    - micro-breaks and tiny damaged edges
  - Must not change:
    - evidence gate
    - output tier
- `high-risk local pass`
  - Input:
    - named high-risk regions
    - locked main-structure draft
    - compatible secondary detail references
  - Output:
    - per-region local decisions and localized corrections only
  - Must not change:
    - global contour
    - global orientation
    - overall projection
    - output tier
- `review-and-merge pass`
  - Input:
    - main draft
    - local-pass decisions
  - Output:
    - merged draft
    - downgrade decisions
    - final `Risk note`
  - Must report each named local region as:
    - `resolved`
    - `simplified`
    - `omitted`
    - `left uncertain`

User overrides such as `只跑主结构`, `只做高风险局部`, or `只做合并复核` may restrict the mode sequence, but they must not bypass the evidence gate, output tier, or `Risk note` rules.

## Workflow

1. Establish the evidentiary ceiling before drawing.
   - Prefer image editing with the source artifact photo as a reference when a photo exists.
   - Assign roles to the references before prompting: choose one `primary geometry reference` for the overall contour and view, and treat other images as `secondary detail references` for local relief, ornament, wear, or inscriptions only.
   - Prefer tightly cropped, well-focused images of a single artifact. Demote or ignore blurry, low-contrast, strongly reflective, or perspective-heavy photos instead of averaging them into the result.
   - Decide the `Source Conditioning / Calibration Gate` class first, then cap the allowed `Output Tier` before writing prompts.
   - Distinguish `report-grade measured drawing`, `report-grade candidate`, `evidence-bounded AI technical draft`, and `archaeological-style illustration`.
   - If the source is a single oblique or partial photo, limit the output to the visible face or visible profile rather than fabricating a full orthographic reconstruction.
   - Treat inscriptions, seals, stamped marks, maker's marks, numerals, alphabetic characters, monograms, ligatures, punctuation, and any text-like or symbol-like traces as high-risk structure. Never let the model translate, modernize, regularize, autocomplete, normalize, or typeset them.
   - Distinguish markings on the artifact from text or symbols outside the artifact. Preserve on-object scripts, letters, numbers, and symbols only when they are clearly visible and diagnostically important; remove museum labels, captions, watermarks, inventory numbers, and background writing unless the user explicitly asks to document them.
   - Never invent exact dimensions, wall thickness, or section geometry from an unrelated view.
   - Complete `Preprocessing / Region Triage` before entering `main-structure pass`.
   - Detect whether the artifact contains high-risk regions such as scripts or symbols, dense repeated ornament, openwork crossings, strong specular glare, damaged or missing boundaries, or severe overlap or occlusion.
   - If no high-risk region is present, skip `high-risk local pass`.
   - If high-risk regions are present, name them explicitly and route them into `high-risk local pass`.
   - Default to automatic mode switching, but honor an explicit user override such as `只跑主结构`, `只做高风险局部`, or `只做合并复核`.
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
   - Maintain a strict hierarchy: overall contour first, major structural boundaries second, diagnostic ornament third, incidental surface noise last or omitted.
   - Use a fixed light logic from upper left at roughly 45 degrees.
   - Use thin lines on lit convex edges and heavier lines on shadow-side convex edges.
   - Reverse that rule for incised or recessed features.
   - If script, letters, numbers, punctuation, or symbol strokes are present on the object, treat them as observed shapes, not readable text. Copy their visible geometry exactly when legible; if illegible, keep them as uncertain marks rather than replacing them with normalized modern text or symbols.
   - Use dashed lines only for hidden, missing, or reconstructed features when there is an evidentiary basis for inference.
   - Do not use painterly shading, wash effects, photographic shadows, gradients, or decorative background.

4. Write prompts in the correct order.
   - State the non-negotiable scientific constraints first.
   - State the chosen output tier near the beginning so the model is not free to over-claim precision.
   - State the current workflow mode explicitly near the beginning.
   - State the artifact type, view system, and orientation second.
   - State the line, point, and shading logic third.
   - State layout, delivery shape, scale-bar handling, risk-note requirement, and any required labels last.
   - Inject output conventions into the prompt itself: white background unless the user requests another plate ground, centered artifact, clean margins, legible line hierarchy at publication scale, and restrained labels.
   - Specify solid lines for visible structure, dashed lines only for hidden or reconstructed structure, and section fill or hatching only when a real section is justified.
   - If the user asks for a plate or provides multiple views or artifacts, specify a grid or aligned plate layout with consistent baselines and truthful scale handling.
   - Ban perspective, color, studio lighting, glossy reflections, speculative restoration, and decorative embellishment explicitly.

5. Review the result against the source before accepting it.
   - Check the overall contour and silhouette first. If the outer shape drifts, reject early.
   - Check the major structure next: view choice, voids, openwork, appendages, sections, and aligned baselines.
   - Check observed damage, wear, asymmetry, and missing areas next. If the result looks cleaner or more complete than the source, revise toward the source.
   - Check dense ornament and repeated motifs at zoomed-in local crops rather than only at full-plate scale. Look for breaks, merged loops, duplicated motifs, and invented closures.
   - If `high-risk local pass` was used, record whether each named local region was resolved, simplified, omitted, or left uncertain.
   - Check line logic last: hierarchy, convex-versus-concave treatment, dashed-line meaning, and whether the drawing still reads as technical rather than decorative.
   - Then review the image against the acceptance checklist in [references/standards.md](references/standards.md).

## Non-Negotiable Rules

- Treat `absolute fidelity to the original object` as the highest rule.
- Prefer orthographic projection over pictorial realism.
- Record what is visible; infer only when the inference is standard, minimal, and supported.
- Separate observed form from reconstructed form.
- Preserve breakage, asymmetry, wear, and irregularity when they are visible in the source.
- Add a scale bar only when real dimensions are known. If dimensions are unknown, omit the scale bar or reserve space for later annotation, but never fabricate numeric scale.
- Never let a stronger output claim exceed the evidence gate.

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
- When inscriptions or symbol-like markings matter, explicitly say `treat all characters, letters, numerals, and symbols as image geometry, not as text content` and `do not translate, simplify, regularize, replace with fonts, or repair missing strokes`.
- Explicitly say `scientific record first, artistic expression second`.

When the first pass is weak:

- Tighten geometry constraints before adding more stylistic language.
- Re-state the reference hierarchy explicitly: `follow the primary reference for contour and projection; use secondary references only to confirm local detail already visible on the same face or view`.
- Ask for less shading, fewer invented details, and stricter orthographic alignment.
- Reduce the scope from `full reconstruction` to `visible-face archaeological drawing` when evidence is limited.
- For dense ornament, ask the model to separate `outer contour`, `main motif`, and `ground pattern` with different line importance rather than rendering all details at equal strength.
- If the result still over-claims certainty, demote the output tier rather than piling on more stylistic language.

Read [references/prompt-templates.md](references/prompt-templates.md) when you need ready-made prompts for vessels, stone tools, openwork ornaments, or review passes.

## Execution

After the prompt is ready, choose an execution path that matches the output tier.

- Prefer the strongest image-generation model the current environment can call, especially one that supports reference-image editing and iterative revision.
- Prefer image editing from the source photos over text-only generation.
- Do not hardcode a local provider or skill. Choose the most capable available model at runtime.
- If multiple callable models exist, prefer the one that best preserves structure, contour, and damage while allowing prompt-based correction.
- Default to automatic mode switching:
  - Start with `main-structure pass`.
  - Invoke `high-risk local pass` only when a named high-risk region exists.
  - Finish with `review-and-merge pass`.
- If the user explicitly requests a subset mode, honor it, but keep the existing evidence gate, output tier, and `Risk note` requirements in force.
- Make mode transitions explicit in user-facing progress updates.
  - At start, state the full triage record, not just the mode name.
  - The opening update should include:
    - `source class`
    - `output tier`
    - `primary geometry reference`
    - `named high-risk regions`
    - `selected mode sequence`
    - current mode as `main-structure pass`
  - If local risk exists, state `high-risk local pass` and name the affected regions explicitly.
  - At the end, state `review-and-merge pass` and provide the final `Risk note`.
- `Tier 2: evidence-bounded AI technical draft`
  - AI generation may proceed directly.
  - Generate at least one correction pass when the first result violates the evidence ceiling, orthographic logic, or line-drawing conventions.
- `Tier 3: archaeological-style illustration`
  - AI generation may proceed directly, but the prompt and deliverable must not imply report-grade authority.
- `Tier 1` and `Tier 1.5`
  - AI may assist with drafting, but the output must go through review-and-refinement steps after generation: contour check, proportion check, view correction, line cleanup, and publication preparation.
  - Model availability alone never justifies `report-grade measured drawing` or `report-grade candidate`.
- If recurring work follows one house style, the highest-leverage improvement is not prompt length but domain adaptation: a small paired dataset of photos and accepted line drawings can support LoRA-style customization better than generic prompting alone.
- After generation, lightly clean the result if needed: remove isolated speckles and mend tiny discontinuities, but do not simplify or fuse fine diagnostic strokes.
- If no image-generation model is callable, return a prompt package, view plan, and review checklist rather than pretending the drawing was produced.

For Codex/OpenAI environments, UI metadata may exist in `agents/openai.yaml`. Claude Code does not need that file to trigger the skill; it relies on `SKILL.md` and the frontmatter description.

## Deliverables

### Draft output

- Default to a single PNG on a white background unless the user explicitly asks for another format or plate style.
- If the user provides multiple views of one artifact, default to one composed plate with aligned views rather than unrelated separate outputs.
- If the user provides multiple artifacts or explicitly asks for a `plate`, compose as a grid with consistent margins, aligned baselines where appropriate, and truthful scale cues only when measurements are known.
- If the user requests separate exports, keep the main composed plate plus individual views only when that split clearly helps publication or review.
- Draft output is suitable for review, markup, local correction, and later cleanup.

### Publication output

- If the user’s goal is publication, report production, catalogue work, or figure submission, prefer vectorizable or production-ready deliverables when the workflow actually supports them, such as SVG, PDF, high-resolution TIFF, or layered line-art source.
- Treat AI output as an initial line-art draft unless it has also gone through human review, correction, and publication preparation.
- Do not promise automatic vectorization or layered source output unless the current toolchain can genuinely produce it.
- Keep labels, numbering, and scale bars restrained and publication-oriented. Omit numeric scale when dimensions are unknown.

### Risk note

- Every output must end with a short `Risk note`.
- For strong evidence, keep the note brief and state the residual limits.
- For weaker evidence, name the uncertain areas explicitly, such as worn ornament, low-contrast relief, reflective glare, dense motif topology, damaged edges, or ambiguous scripts/symbols.
- If `high-risk local pass` was used, say whether each named local area was resolved, simplified, omitted, or left uncertain.

## LoRA Readiness

This section is a future-facing extension. It is not required for normal use.

Current backend reality:

- This skill does not currently provide a built-in trainable image-generation backend.
- If the available image backend is only a hosted general model, treat LoRA as external future work rather than something the skill can execute directly.
- In OpenAI-only image-generation environments, assume workflow control, reference discipline, and review passes are the primary optimization tools unless a separate trainable image backend is introduced.

Consider LoRA only when all of the following are becoming true:

- the workload is recurring rather than occasional
- a stable house style or review standard exists
- the same classes of failure keep recurring
- a small paired dataset is available

Minimum recommended data structure:

- input artifact photo
- accepted line drawing
- same view only
- no mismatched pairs such as oblique source photo plus idealized full reconstruction

Useful optional metadata:

- artifact type
- view
- `source class`
- `output tier`
- `risk regions`

Suggested collection buckets:

- `base set`
- `ornament hard set`
- `script/symbol hard set`

Do not train on examples such as:

- blurry or reflective photo paired with a clean idealized reconstruction
- ancient or damaged script paired with normalized modern text
- broken ornament paired with fully repaired ornament

Treat this as training-readiness guidance only. Do not assume a full training pipeline, commands, or tooling are already available inside this skill.

## Failure Modes

Avoid these common errors:

- Perspective distortion disguised as technical drawing.
- Symmetrizing an asymmetric object.
- Replacing observed damage with idealized edges.
- Converting ancient, foreign, symbolic, or damaged markings into normalized modern characters, letters, numerals, or punctuation.
- Presenting an `evidence-bounded AI technical draft` as `report-grade measured drawing`.
- Letting `high-risk local pass` overwrite the global contour or projection established by `main-structure pass`.
- Treating a local-pass correction as evidence strong enough to upgrade the output tier.
- Inventing unseen backs, interiors, or thickness.
- Using uniform line weight everywhere.
- Rendering the image like fantasy concept art, engraving art, or decorative poster art.
- Adding a fake scale bar or fake measurement marks.
