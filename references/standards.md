# Archaeological Drawing Standards

## Contents

1. Core principles
2. Source conditioning / calibration gate
3. Output tiers
4. Workflow modes
5. Preprocessing / region triage
6. Mode input/output contract
7. View and layout system
8. Light and relief logic
9. Texture and stroke system
10. Artifact-specific rules
11. Conventions and annotation
12. Acceptance checklist

## 1. Core Principles

- Treat the drawing as a scientific record first.
- Use orthographic projection, not perspective.
- Preserve observable irregularities, breakage, wear, and asymmetry.
- Distinguish observed, inferred, and reconstructed information.
- Do not beautify, complete, or regularize unsupported details.
- Do not fabricate dimensions, scale, wall thickness, or hidden structure.
- Rank evidence sources before drawing: one primary view controls contour and projection; secondary views may clarify only those details that are actually compatible with that same observed structure.
- Never let the claimed output tier exceed the evidence class of the source.

## 2. Source Conditioning / Calibration Gate

Classify the source before drawing and use the weakest defensible class when uncertain.

- `Measured object + calibrated multi-view`
  - Allows `report-grade candidate`.
  - Still requires human review before `report-grade measured drawing`.
- `Near-orthographic view with scale reference`
  - Allows single-face orthographic output and possibly `report-grade candidate` of that face only.
- `Multi-view but uncalibrated`
  - Allows only `evidence-bounded AI technical draft`.
- `Single oblique photo`
  - Allows only `visible-face redraw` inside `evidence-bounded AI technical draft`.
- `Text-only description`
  - Allows only `view plan`, `prompt package`, or `schematic concept`.

## 3. Output Tiers

- `Tier 1: report-grade measured drawing`
  - Requires measured or calibrated support plus human review and publication preparation.
- `Tier 1.5: report-grade candidate`
  - Requires strong evidence, but must disclose what is still missing.
- `Tier 2: evidence-bounded AI technical draft`
  - Default AI output for most image-based requests.
- `Tier 3: archaeological-style illustration`
  - Use when evidence is weak or schematic intent dominates.

## 4. Workflow Modes

- `main-structure pass`
  - Lock the overall contour, projection, baseline, and major structure first.
  - Defer high-risk local detail rather than forcing premature completeness.
- `high-risk local pass`
  - Work only on named high-risk regions.
  - Do not alter the global contour, orientation, or overall view choice.
- `review-and-merge pass`
  - Reconcile local detail with the main structure draft.
  - Simplify, omit, or downgrade unsupported detail instead of overfitting local noise.
- Modes never strengthen the output tier beyond what the source gate allows.
- Automatic switching is the default; explicit user requests may restrict the mode sequence, but not bypass the evidence gate.

## 5. Preprocessing / Region Triage

- Run triage before `main-structure pass`.
- Identify:
  - `source class`
  - `output tier`
  - `primary geometry reference`
  - `secondary detail references`
  - `named high-risk regions`
  - `selected mode sequence`
- Prefer cropping or isolating the relevant object, face, or local region before prompting when background, glare, or labels would dilute the evidence.
- Demote low-value inputs rather than averaging them into the decision.
- If triage remains contradictory, lower the ambition of the drawing.
- In user-facing execution, report the triage explicitly rather than implying it silently.
  - Minimum visible triage report:
    - `source class`
    - `output tier`
    - `primary geometry reference`
    - `named high-risk regions`
    - `selected mode sequence`

## 6. Mode Input/Output Contract

- `main-structure pass`
  - Input:
    - `source class`
    - `output tier`
    - `primary geometry reference`
    - overall object or face scope
  - Output:
    - stable global contour
    - projection and baseline
    - major structure and broad zoning
    - unresolved local-risk list
  - Must not finalize scripts, dense ornament micro-topology, or tiny damage edges.
- `high-risk local pass`
  - Input:
    - named local regions
    - locked main-structure draft
    - compatible secondary references
  - Output:
    - local corrections or downgrade decisions only
  - Must not alter global contour, global orientation, overall projection, or output tier.
- `review-and-merge pass`
  - Input:
    - main draft
    - local-pass outcomes
  - Output:
    - merged draft
    - final downgrade decisions
    - final `Risk note`
  - Must report each named local region as `resolved`, `simplified`, `omitted`, or `left uncertain`.

## 7. View And Layout System

### Symmetric vessels

- Use the conventional left-half section and right-half exterior view only when the source supports a reliable profile.
- Align both halves to a shared vertical centerline.
- Show wall thickness only when it is known or safely inferable from the profile and section evidence.
- Use filled black section or disciplined section hatching for cut surfaces, depending on the local house style.

### Asymmetric or complex vessels

- Choose the main view that shows the most diagnostic profile.
- Place spouts, handles, lugs, ears, and appendages in the orientation that best exposes their shape.
- Add projected side, back, top, or bottom views when one view would hide important structure.
- Keep all views aligned by projection, not by visual balancing.

### Stone tools

- Use at least front, back, and side views when morphology requires it.
- Keep the point upward and the edge downward unless the typological convention differs.
- Preserve flake scars, striking platform, bulb of percussion, ripples, and negative scar boundaries.

### Openwork plaques, fittings, pendants, and thin ornaments

- Use the visible face as the primary orthographic view.
- Preserve the external frame, all perforations, internal voids, and motif boundaries exactly.
- Add side or section views only when the thickness or profile is genuinely evidenced.
- Do not force a vessel-like section system onto flat openwork pieces.

## 8. Light And Relief Logic

Use a fixed imaginary light source from the upper left at about 45 degrees.

### Convex features

- Use thinner line treatment on lit upper-left edges.
- Use heavier line treatment on lower-right edges.
- Increase line density gradually into shadow.

### Concave or incised features

- Reverse the convex rule.
- Use heavier treatment on upper-left edges of recesses.
- Use lighter treatment on lower-right edges of recesses.

### General rule

- Use line weight and density to explain relief.
- Do not use painterly gradients, airbrush shading, or photographic cast shadow.

## 9. Texture And Stroke System

Use only line, point, hatching, and white space as the core rendering language.

### Line rendering

Use line rendering for rough, hard, or sharply faceted surfaces.

- Vary line thickness and spacing according to relief.
- Follow the actual direction of the surface when rendering flake scars or ridges.
- Keep contour lines thinner on light-facing edges and heavier on shadow-facing edges.

### Dot rendering

Use dot rendering for fine, smooth, or subtly curved surfaces.

- Keep dots sparse in bright areas.
- Increase dot density gradually toward shadow.
- Avoid decorative stipple fields that obscure morphology.

### Mixed rendering

Mix line and dot rendering only when it clarifies form rather than beautifying the plate.

## 10. Artifact-Specific Rules

### Stone artifacts

Capture the diagnostic knapping evidence:

- striking platform
- point of percussion
- bulb of percussion
- scar negatives and radiating lines
- ripple marks

### Pottery

- Preserve wheel marks, hand-built irregularities, joins, and surface finishing traces when visible.
- Render cord mark, basket mark, lattice, comb mark, or incised ornament with their actual overlap and compression logic.
- Do not make repetitive texture unnaturally regular.

### Bronze and cast metal objects

- Render main ornament with clean single-line structure.
- Render ground pattern with finer treatment than the main motif.
- Preserve corrosion loss, softened edges, and damaged borders when visible.
- Avoid chrome-like sheen, metallic reflections, and poster-like contrast.
- In densely decorated areas, keep ornament topology continuous and separated: do not let adjacent loops merge, collapse, or duplicate.

### Openwork decorative plaques or fittings

- Prioritize silhouette, negative space, and motif articulation.
- Preserve the relationship between frame and internal motif exactly.
- Show breaks, missing corners, and deformation rather than smoothing them away.
- Use very restrained stipple or short hatching in recessed or overlapping zones.

### Jade or highly polished stone

- Prefer cleaner contour and restrained dot rendering.
- Avoid heavy rough-texture lines unless the source actually shows them.

### Inscriptions, scripts, symbols, and text-like marks

- First decide whether the marks belong to the artifact or to the photographic context.
- Remove background captions, museum labels, watermarks, inventory numbers, and exhibition text unless the user explicitly wants them documented.
- If the artifact itself bears inscription, seal script, scratched marks, stamped symbols, letters, numerals, punctuation, maker's marks, monograms, or other symbol-like traces, treat them as observed graphic form rather than readable language.
- Never translate, simplify, regularize, typeset, autocomplete, or normalize ancient or damaged markings into modern script, letters, digits, or punctuation.
- Preserve stroke placement, proportion, spacing, breakage, and asymmetry exactly where visible.
- If a mark is only partly visible or worn, render only the observed strokes or contours. Do not complete the missing parts into a legible character or symbol.
- If the marking is too unclear to record faithfully at the current evidence level, omit it or reduce it to restrained uncertain marks rather than fabricating readable text or symbols.

## 11. Conventions And Annotation

- Use solid lines for visible edges and visible ornament.
- Use dashed lines for hidden or reconstructed structure only when a basis exists.
- Use irregular observed break lines as solid visible lines when the break itself is visible.
- Reserve section fill or hatching for actual cut surfaces.
- Add a scale bar only when dimensions are known from the user or source metadata.
- If dimensions are unknown, do not fabricate a `0-5 cm` bar. Leave a clean zone for later addition if needed.
- If on-object scripts or symbols are recorded, state or imply that the strokes are copied from observation, not editorially normalized text.
- When repeated motifs are present, preserve actual variation between units. Do not normalize them into identical repeated modules.
- Every deliverable must end with a brief `Risk note`.
- For `report-grade measured drawing` and `report-grade candidate`, the `Risk note` should state residual limits, remaining checks, or missing publication-preparation steps.
- For `evidence-bounded AI technical draft` and `archaeological-style illustration`, the `Risk note` should name the weak zones and why they are uncertain.
- If `high-risk local pass` was used, the `Risk note` should say whether each named local area was resolved, simplified, omitted, or left uncertain.

## 12. Acceptance Checklist

Accept the drawing only if the answer to every required question is yes:

- Does the overall contour match the source?
- Does the view system match the artifact type?
- Is the image orthographic rather than pictorial?
- Are voids, breaks, and asymmetries preserved?
- In dense ornament, do local motif junctions remain topologically correct without merged or hallucinated loops?
- Are line weights consistent with the upper-left light rule?
- Does the texture language clarify form instead of decorating it?
- Are hidden or reconstructed parts clearly distinguished from observed parts?
- If scripts or symbols are present, do they remain image-faithful stroke records rather than readable modernized text?
- Is any scale bar truthful and data-backed?
- Does the claimed output tier stay within the source conditioning gate?
- If the output is `report-grade candidate`, are the missing requirements explicitly named?
- Did `Preprocessing / Region Triage` happen before generation?
- Were primary and secondary references used according to their roles?
- If a local pass was used, did it avoid altering the global contour or projection?
- If a user forced a subset mode, were output-tier and risk-note rules still preserved?
- If progress updates were shown, did the opening update expose the minimum triage report rather than only the current mode name?
- Is uncertainty communicated in a short risk note?
- Does the result read as a technical archaeological drawing rather than a fantasy illustration or poster?
