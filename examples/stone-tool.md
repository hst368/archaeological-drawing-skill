# Example: Stone Tool Multi-View Drawing

Use this example when a stone artifact needs front, back, and side views.

```text
Use $archaeological-drawing to plan and draft a standard archaeological multi-view drawing of this stone tool.

Triage assumptions:
- Source class: multi-view but uncalibrated unless scale and calibrated views are provided.
- Output tier: evidence-bounded AI technical draft.
- Primary geometry reference: the clearest face view.
- Secondary detail references: back and side views only where they are compatible with the same object geometry.
- Named high-risk regions: low-contrast scar boundaries, damaged working edge, unclear striking platform.
- Selected mode sequence: main-structure pass -> high-risk local pass -> review-and-merge pass.

Requirements:
- Use orthographic views only.
- Arrange front, back, and side views in aligned projection.
- Preserve striking platform, point of percussion, bulb of percussion, ripple marks, negative scar boundaries, edge retouch, and breakage where visible.
- Use line and dot rendering to explain flake scar relief; do not beautify or regularize the knapping pattern.
- Omit scale marks unless real dimensions are supplied.
- End with a Risk note.
```

Expected agent behavior:

- Use the strongest view to control contour and projection.
- Keep side-view thickness conservative unless measured or clearly visible.
- Do not invent hidden scar systems from a single face.
