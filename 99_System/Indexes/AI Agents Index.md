---
type: index
status: active
area: ai-agents
tags:
  - index
  - ai-agents
  - obsidian
  - mcp
created: 2026-06-22
updated: 2026-06-22
confidence: high
owner: hugo
ai_use: true
---

# AI Agents Index

Index des notes liées aux agents IA, à Obsidian comme mémoire longue, au MCP, et aux systèmes d'indexation/retrieval pour projets complexes.

## Notes clés

- [[Hermes Obsidian Memory Protocol]]
- [[2026-06-22 - Architecture mémoire Obsidian-Hermes]]
- [[2026-06-22 - Review câblage Obsidian Hermes MCP]]
- [[2026-06-22 - Bonnes pratiques mémoire Obsidian-Hermes 2026]]
- [[Memory Quality Dashboard]]

## Recherches AI agents

```dataview
TABLE area, status, confidence, updated
FROM "30_Resources/Research"
WHERE contains(tags, "ai-agents") OR area = "ai-agents"
SORT updated DESC
```

## Protocoles système agents

```dataview
TABLE status, updated
FROM "99_System/AI Protocols"
WHERE ai_use = true
SORT updated DESC
```
