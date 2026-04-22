# Prompt Templates

## Contents

1. Generic photo to archaeological drawing
2. Symmetric vessel
3. Stone tool
4. Openwork plaque or fitting
5. Review and correction pass

## 1. Generic Photo To Archaeological Drawing

Use when the user provides a photo and wants a strict archaeological-style redraw.

```text
Use the provided source image as a reference and convert the artifact into a rigorous archaeological drawing.

Hard constraints:
- Scientific record first, artistic expression second.
- Use orthographic drawing logic only. No perspective, no camera-angle distortion, no dramatic lighting.
- Preserve the observable contour, internal structure, damage, perforations, asymmetry, and visible ornament exactly.
- Remove photographic background, museum captions, labels, glare, and cast shadows.
- Use black linework, dots, hatching, and white negative space only.
- Use a fixed light logic from upper left at about 45 degrees.
- Do not invent unseen structure, missing ornament, exact thickness, or a fake scale bar.

Output:
- Clean white background
- Centered artifact
- Publication-style archaeological plate quality
- Technical line hierarchy, not decorative illustration
```

## 2. Symmetric Vessel

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

## 3. Stone Tool

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

## 4. Openwork Plaque Or Fitting

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

## 5. Review And Correction Pass

Use after a weak first generation.

```text
Revise the generated drawing to be more archaeologically rigorous.

Corrections:
- Reduce stylization and decorative rendering.
- Restore the source object's real asymmetry, damage edges, and contour irregularities.
- Tighten orthographic geometry and remove any remaining perspective cues.
- Simplify shading into technical line and dot logic only.
- Remove invented details and any unsupported reconstruction.
- Make the result look like a scientific archaeological plate, not an art print.
```
