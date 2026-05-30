# Example: Openwork Plaque Or Fitting

Use this example for flat openwork metal, jade, bone, or similar ornaments.

```text
Use $archaeological-drawing to convert the provided openwork plaque photo into an evidence-bounded archaeological drawing of the visible face.

Triage assumptions:
- Source class: single oblique photo unless the image is near-orthographic.
- Output tier: evidence-bounded AI technical draft.
- Primary geometry reference: the visible frontal face in the photo.
- Named high-risk regions: dense openwork crossings, worn motif edges, reflective highlights, damaged frame edges.
- Selected mode sequence: main-structure pass -> high-risk local pass -> review-and-merge pass.

Requirements:
- Treat the object as a flat openwork artifact, not as a vessel.
- Preserve the outer frame, internal voids, motif boundaries, breakage, asymmetry, and visible relief transitions.
- Remove museum captions, labels, non-artifact glare, background, and cast shadows.
- Treat reflective highlights on the artifact body as high-risk local regions; do not inpaint or complete hidden motif edges.
- Do not invent a back view, section, thickness, restoration, or scale bar.
- Use black linework, restrained stipple or short hatching, and a white background.
- End with a Risk note naming unresolved high-risk regions.
```

Expected agent behavior:

- Demote the output if the source is oblique, reflective, or too low-resolution.
- Keep uncertain internal topology simplified or left uncertain rather than regularized.
- Do not convert decorative marks into readable modern symbols or text.
