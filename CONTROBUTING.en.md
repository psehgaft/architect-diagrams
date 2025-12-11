# Contributing architecture diagrams

Thanks for contributing. Follow these steps to add or update diagrams:

1. Create a branch: `git checkout -b feature/diagram-<topic>`.
2. Add the editable source under `diagrams/<system>/sources/`.
3. Generate the export under `diagrams/<system>/exports/`. Prefer SVG.
4. Add a README/metadata in the system folder if the diagram needs context.
5. Open a Pull Request including:
   - Summary of the architectural change.
   - What was updated in the diagram and why.
   - Related changes (APIs, infra) if applicable.
6. Tag appropriate reviewers.

PR checklist
- [ ] Editable source included and buildable (when applicable).
- [ ] Export (SVG/PDF) included.
- [ ] Changes documented in PR description.
- [ ] Use Git LFS for large binary files.

How to test locally
- PlantUML: `plantuml -tsvg diagrams/<system>/sources/*.puml`
- Mermaid CLI: `mmdc -i input.mmd -o output.svg`
- diagrams.net: open .drawio and export SVG/PDF
