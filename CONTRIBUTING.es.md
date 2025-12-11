# Contribuir con diagramas de arquitectura

Gracias por contribuir. Sigue estos pasos para añadir o actualizar diagramas:

1. Crea una rama: `git checkout -b feature/diagrama-<tema>`.
2. Añade la fuente editable en `diagrams/<sistema>/sources/`.
3. Genera la exportación en `diagrams/<sistema>/exports/`. Preferible: SVG.
4. Añade un README/metadata en la carpeta del sistema si el diagrama necesita contexto.
5. Abre un Pull Request con:
   - Resumen del cambio arquitectónico.
   - Qué se actualizó en el diagrama y por qué.
   - Cambios relacionados (APIs, infra) si aplica.
6. Etiqueta revisores apropiados.

Checklist del PR
- [ ] Fuente editable incluida y compilable (si aplica).
- [ ] Exportación (SVG/PDF) incluida.
- [ ] Cambios documentados en la descripción del PR.
- [ ] Usa Git LFS para archivos binarios grandes.

Cómo probar localmente
- PlantUML: `plantuml -tsvg diagrams/<sistema>/sources/*.puml`
- Mermaid CLI: `mmdc -i input.mmd -o output.svg`
- diagrams.net: abrir .drawio y exportar SVG/PDF
