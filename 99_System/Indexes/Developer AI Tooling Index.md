---
type: index
status: active
area: developer-tooling
tags:
  - index
  - developer-tooling
  - ai-agents
  - code-indexing
  - project-indexing
  - mcp
created: 2026-06-22
updated: 2026-06-22
confidence: high
owner: hugo
ai_use: true
---

# Developer AI Tooling Index

Index dédié aux recherches et ressources sur les outils de développement augmentés par IA : indexation de codebase, retrieval projet, MCP, agents de code, impact analysis, réindexation automatique, graphes de dépendances et workflows pour projets complexes.

Ce n'est pas un index sur la mémoire Hermes. Obsidian peut être mentionné ici uniquement comme outil de documentation/retrieval pour les projets de développement.

## Notes clés

- [[2026-06-22 - Outils indexation projet pour agents IA]]
- [[2026-06-22 - Rapport détaillé - Indexation projet Obsidian agents IA]]

## Recherches liées à l'outillage dev IA

```dataview
TABLE area, status, confidence, updated
FROM "30_Resources/Research"
WHERE contains(tags, "code-indexing") OR contains(tags, "project-indexing") OR contains(tags, "developer-tooling")
SORT updated DESC
```
