# Kiro Webwright Power — TODO

## Contexto

Power adaptado de [Microsoft Webwright](https://github.com/microsoft/Webwright) (MIT License).
Repositorio: `github.com/JosSantamaria/Kiro-Webwright`

## Pendientes

- [ ] Probar el power instalado desde GitHub en Kiro (Add power from GitHub)
- [ ] Verificar que los keywords activan el power correctamente
- [ ] Verificar que los steering files se cargan con readSteering
- [ ] Probar un task real de web automation (ej: buscar vuelos, extraer datos)
- [ ] Investigar cómo publicar en el registry oficial de kiro.dev/powers
- [ ] Investigar si `icon` y `version` se soportan en el frontmatter o solo en el registry
- [ ] Agregar más steering files si se necesitan (ej: anti-detection, data extraction patterns)
- [ ] Considerar agregar un MCP server wrapper para Playwright (más complejo pero más integrado)

## Notas

- El power NO requiere MCP server — usa bash directamente (Playwright CLI)
- Firefox es preferido sobre Chromium (evita TLS fingerprinting)
- El power funciona sin API keys adicionales (usa las herramientas nativas de Kiro: bash, read, write)
- Basado en el SKILL.md original de Webwright para Claude Code/Codex
