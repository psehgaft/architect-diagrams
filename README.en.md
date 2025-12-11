# Architecture Diagrams Repository

This repository centralizes architecture diagrams, their editable sources, and exported artifacts. Its goals:
- Keep editable sources (PlantUML, Mermaid, diagrams.net/draw.io, etc.).
- Version diagrams alongside code and architecture.
- Publish high-quality exports (SVG/PDF/PNG) for documentation.

Recommended structure
- diagrams/
  - <system>/
    - sources/
      - <name>.puml | <name>.mmd | <name>.drawio
    - exports/
      - <name>.svg | <name>.png | <name>.pdf
    - README.md (optional: context + metadata)
- templates/ (reusable templates)
- .github/ (issue & PR templates, workflows)

Guidelines & best practices
- Always store the editable source together with the exported file (e.g. my-service.puml + my-service.svg).
- Prefer SVG exports for vector diffs. Use PNG only when raster is required.
- Avoid committing large binaries without Git LFS. Use Git LFS for .drawio, .vsdx, large .png/.jpg files.
- Name folders by system or bounded context: diagrams/auth, diagrams/billing, etc.
- Tag or add a CHANGELOG in a diagram folder for significant architectural changes.

Recommended tools
- PlantUML (.puml) — text-based, automatable.
- Mermaid (.mmd) — integrates well with Markdown and many renderers.
- diagrams.net / draw.io (.drawio) — visual; treat as binary/ZIP and use LFS.
- Export additionally to PDF/SVG for stakeholder consumption.

Suggested workflow
1. Create a branch: `git checkout -b feature/diagram-<topic>`.
2. Add or update source in `diagrams/<system>/sources/`.
3. Generate export in `diagrams/<system>/exports/` (SVG preferred).
4. Open a PR with a description of the architectural change and screenshots if helpful.
5. Reviewers validate diagram correctness and architectural consistency.

If you want, I can add these files to the repository — tell me owner/repo and the target branch (or I can create one).
