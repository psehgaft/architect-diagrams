# Repositorio de Diagramas de Arquitectura

Este repositorio centraliza diagramas de arquitectura, sus fuentes editables y sus exportaciones. Objetivos:
- Mantener las fuentes editables (PlantUML, Mermaid, diagrams.net/draw.io, etc.).
- Versionar diagramas junto al código y la arquitectura.
- Publicar exportaciones de alta calidad (SVG/PDF/PNG) para documentación.

Estructura recomendada
- diagrams/
  - <sistema>/
    - sources/
      - <nombre>.puml | <nombre>.mmd | <nombre>.drawio
    - exports/
      - <nombre>.svg | <nombre>.png | <nombre>.pdf
    - README.md (opcional: contexto + metadata)
- templates/ (plantillas reutilizables)
- .github/ (plantillas de issues/PRs, workflows)

Normas y buenas prácticas
- Guarda siempre la fuente editable junto con la exportación (p. ej. my-service.puml + my-service.svg).
- Prefiere SVG para exportaciones (vectoriales y diffeables). Usa PNG sólo si se requiere raster.
- Evita subir binarios grandes sin Git LFS. Usa Git LFS para .drawio, .vsdx y imágenes grandes.
- Nombra carpetas por sistema o bounded context: diagrams/auth, diagrams/billing, etc.
- Usa tags o un CHANGELOG por carpeta si hay cambios arquitectónicos significativos.

Herramientas recomendadas
- PlantUML (.puml) — basado en texto y fácil de automatizar.
- Mermaid (.mmd) — integrable con Markdown y varios renderers.
- diagrams.net / draw.io (.drawio) — visual; trátalo como binario/ZIP y usa LFS.
- Exporta también en PDF/SVG para compartir con stakeholders.

Flujo de trabajo sugerido
1. Crea una rama: `git checkout -b feature/diagrama-<tema>`.
2. Añade/actualiza la fuente en `diagrams/<sistema>/sources/`.
3. Genera la exportación en `diagrams/<sistema>/exports/` (SVG preferible).
4. Abre un PR con la descripción del cambio arquitectónico y capturas si aplica.
5. Los revisores validan la corrección del diagrama y la coherencia arquitectónica.

Si quieres que añada estos archivos al repositorio, indícame owner/repo y la rama destino (o que cree una rama nueva).
