# Prompt Templates

## Contents

1. Generic photo to archaeological drawing
2. Evidence gate prompt prefix
3. Preprocessing / region triage block
4. Main-structure pass prompt prefix
5. High-risk local pass prompt prefix
6. Review-and-merge pass prompt prefix
7. Symmetric vessel
8. Stone tool
9. Openwork plaque or fitting
10. Review and correction pass

## 1. Generic Photo To Archaeological Drawing

Use when the user provides a photo and wants a strict archaeological-style redraw.

```text
Use the provided source image as a reference and convert the artifact into a rigorous archaeological drawing.

Hard constraints:
- Scientific record first, artistic expression second.
- Declare the output tier explicitly at the start: `report-grade candidate`, `evidence-bounded AI technical draft`, or `archaeological-style illustration`.
- Use orthographic drawing logic only. No perspective, no camera-angle distortion, no dramatic lighting.
- Use one primary reference image for contour and projection. Use any additional reference images only to confirm local detail visible on the same face or view.
- Preserve the observable contour, internal structure, damage, perforations, asymmetry, and visible ornament exactly.
- Remove photographic background, museum captions, labels, glare, and cast shadows.
- Treat any text outside the artifact as background noise unless explicitly requested.
- If the artifact bears inscription, script, letters, numerals, punctuation, or symbol-like marks, treat every mark as image geometry, not language: do not translate, simplify, regularize, typeset, autocomplete, or convert it into modern text or symbols.
- If these strokes or marks are incomplete or unclear, copy only the visible parts or omit them; do not repair them into legible characters, digits, or symbols.
- Keep a strict detail hierarchy: overall contour first, major structure second, diagnostic ornament third, incidental texture last or omitted.
- In dense ornament, preserve topology and spacing of loops, cells, and motif junctions. Do not merge adjacent lines, duplicate units, or regularize repeated motifs into identical patterns.
- Use black linework, dots, hatching, and white negative space only.
- Use a fixed light logic from upper left at about 45 degrees.
- Do not invent unseen structure, missing ornament, exact thickness, or a fake scale bar.

Output:
- Clean white background
- Centered artifact
- Technical archaeological plate quality appropriate to the chosen output tier
- Technical line hierarchy, not decorative illustration
- End with a short `Risk note` naming residual limits or uncertain areas
```

## 2. Evidence Gate Prompt Prefix

Use before any task-specific prompt when the evidence class needs to be stated clearly.

```text
Source conditioning:
- Source class: [measured object + calibrated multi-view / near-orthographic view with scale reference / multi-view but uncalibrated / single oblique photo / text-only description]
- Allowed output tier: [report-grade candidate / evidence-bounded AI technical draft / archaeological-style illustration / view plan only]
- Do not exceed this evidence class.
```

## 3. Preprocessing / Region Triage Block

Use before generation when the workflow needs an explicit triage record.

```text
Preprocessing / Region Triage:
- Source class: [source class]
- Output tier: [output tier]
- Primary geometry reference: [primary reference]
- Secondary detail references: [secondary references]
- Named high-risk regions:
  - [region 1]
  - [region 2]
- Selected mode sequence:
  - [main-structure pass]
  - [high-risk local pass if needed]
  - [review-and-merge pass]

Demote or ignore weak inputs such as blur, glare, bad perspective, shallow focus, or severe occlusion.
If triage remains contradictory, lower the ambition of the drawing before generation.
```

In user-facing progress updates, expose at least:
- `source class`
- `output tier`
- `primary geometry reference`
- `named high-risk regions`
- `selected mode sequence`

## 4. Main-Structure Pass Prompt Prefix

Use when the goal is to lock the global drawing safely before touching high-risk detail.

```text
Workflow mode: main-structure pass.

Input:
- source class
- output tier
- primary geometry reference
- object or face scope

Focus only on:
- overall contour
- projection and baseline
- major structure
- broad ornament zoning

Defer or suppress high-risk local detail such as scripts, dense repeated ornament, glare-heavy edges, openwork crossings, and damaged boundaries.
Do not force local completeness if the source evidence is weak.

Handoff to next mode:
- unresolved local-risk list
- locked global contour and projection
```

## 5. High-Risk Local Pass Prompt Prefix

Use when one or more named local regions need isolated handling.

```text
Workflow mode: high-risk local pass.

Input:
- named local regions
- locked main-structure draft
- compatible secondary detail references

Named local regions:
- [region 1]
- [region 2]

Redraw only these local regions.
Do not alter the global contour, overall projection, or broad structural zoning established by the main-structure pass.
If a local detail remains unsupported, simplify it, omit it, or leave it uncertain rather than inventing a clean solution.

Handoff to next mode:
- per-region result: resolved / simplified / omitted / left uncertain
```

## 6. Review-And-Merge Pass Prompt Prefix

Use when the main draft and local corrections need to be reconciled.

```text
Workflow mode: review-and-merge pass.

Input:
- main structure draft
- local-pass outcomes

Merge the validated local corrections into the main structure draft.
Do not upgrade the output tier.
If any local area remains weak, explicitly demote, simplify, or omit it.
End with a short `Risk note` that states whether each named high-risk region was resolved, simplified, omitted, or left uncertain.
```

## 7. Symmetric Vessel

Use only when a reliable profile is visible or otherwise supported.

```text
Generate a standard archaeological orthographic vessel drawing for [artifact name].

Requirements:
- Use the conventional composition: left half vertical section, right half exterior view, sharing a single vertical centerline.
- Render the profile with strict orthographic geometry. No perspective.
- Show wall thickness only if supported by the source. If not supported, keep the drawing to exterior profile only.
- Use black section fill or disciplined section hatching on true cut surfaces only.
- Use upper-left light logic on the exterior half.
- Preserve wheel marks, surface ornament, and damage visible in the source.
- Add a scale bar only if real dimensions are available. Otherwise omit it.
- White background, black line drawing, report-ready layout.
```

## 8. Stone Tool

```text
Generate a standard archaeological multi-view drawing of this stone artifact.

Requirements:
- Use orthographic views only.
- Arrange front view, back view, and side view in aligned projection.
- Keep the point upward and the working edge downward unless the source suggests another standard orientation.
- Accurately record striking platform, point of percussion, bulb of percussion, ripple marks, and flake-scar boundaries where visible.
- Use parallel and curved linework that follows scar direction; denser in deeper shadowed areas, sparser in shallow areas.
- Do not beautify or regularize the flaking pattern.
- White background, black technical drawing only.
```

## 9. Openwork Plaque Or Fitting

Use for openwork metal, jade, bone, or similar flat decorative objects.

```text
Use the provided source image as a reference and convert this object into a strict archaeological drawing of the visible face.

Requirements:
- Treat it as a flat openwork artifact, not as a vessel.
- Use the frontal orthographic face as the primary view.
- Preserve the outer frame, all internal voids, motif edges, relief transitions, breaks, and irregularities exactly.
- Keep the composition vertical and centered.
- Use black contour lines with restrained stipple or short hatching only where needed to explain relief.
- Use upper-left light logic for convex and concave details.
- Remove photo background, captions, and reflections.
- Do not invent a reconstructed back, exact thickness, or a fake section.
- White background, publication-style archaeological line drawing.
```

## 10. Review And Correction Pass

Use after a weak first generation.

```text
Revise the generated drawing to be more archaeologically rigorous.

Corrections:
- Reduce stylization and decorative rendering.
- Restore the source object's real asymmetry, damage edges, and contour irregularities.
- Tighten orthographic geometry and remove any remaining perspective cues.
- Restore ornament topology in local areas: reopen merged loops, remove duplicated curves, and separate main motif from ground pattern.
- Simplify shading into technical line and dot logic only.
- Remove any text or symbol hallucination or normalization. Any script, letter, numeral, or symbol must remain copied stroke geometry, not readable modernized content.
- Remove invented details and any unsupported reconstruction.
- If the source evidence is weaker than the current claim, demote the output tier instead of polishing the wording.
- End with a short `Risk note`.
- Make the result look like a scientific archaeological plate, not an art print.
```
