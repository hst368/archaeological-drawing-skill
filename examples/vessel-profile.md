# Example: Vessel Profile Or Half-Section

Use this example for pottery, bronze, or other vessel-like objects where a profile or half-section is requested.

```text
Use $archaeological-drawing to produce a report-style vessel drawing plan and, if evidence supports it, an archaeological line-art draft.

Triage assumptions:
- Source class: near-orthographic view with scale reference only if the image and measurement evidence support it; otherwise multi-view but uncalibrated or single oblique photo.
- Output tier: report-grade candidate only with strong calibrated evidence; otherwise evidence-bounded AI technical draft.
- Primary geometry reference: the clearest profile or frontal view.
- Named high-risk regions: rim profile, foot/base line, handle or spout projection, wall thickness, surface ornament.
- Selected mode sequence: main-structure pass -> high-risk local pass -> review-and-merge pass.

Requirements:
- Use the conventional left-half section and right-half exterior view only when profile and wall-thickness evidence are reliable.
- Align the drawing to a shared vertical centerline.
- Preserve visible asymmetry, wear, repair, corrosion, and surface traces.
- Show wall thickness only when known or safely supported by the source.
- Omit scale bars and exact measurements unless supplied.
- End with a Risk note explaining any missing calibration, profile uncertainty, or unsupported section detail.
```

Expected agent behavior:

- Prefer a view plan or exterior profile if the source cannot support a half-section.
- Never upgrade weak image evidence into a measured report drawing.
- Separate observed contour from inferred or reconstructed structure.
