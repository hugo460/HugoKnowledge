---
type: index
status: active
project: Hermes Obsidian Memory
area: AI Agents
tags: [index, dataview, obsidian, memory, quality]
created: 2026-06-22
updated: 2026-06-22
confidence: high
owner: hugo
ai_use: true
---

# Memory Quality Dashboard

Ce dashboard sert à maintenir une mémoire Obsidian propre, utile et exploitable par Hermes.

## Notes à revoir

```dataview
TABLE type, status, confidence, updated, review_after, source
FROM ""
WHERE ai_use = true AND review_after AND review_after <= date(today)
SORT review_after ASC
```

## Notes actives avec faible confiance

```dataview
TABLE type, project, updated, source, file.inlinks, file.outlinks
FROM ""
WHERE ai_use = true AND status = "active" AND confidence = "low"
SORT updated DESC
```

## Notes sans type

```dataview
TABLE status, updated, file.folder
FROM ""
WHERE ai_use = true AND !type
SORT file.folder ASC
```

## Notes sans date updated

```dataview
TABLE type, status, file.folder
FROM ""
WHERE ai_use = true AND !updated
SORT file.folder ASC
```

## Notes orphelines AI

```dataview
TABLE type, status, updated
FROM ""
WHERE ai_use = true AND length(file.inlinks) = 0 AND length(file.outlinks) = 0
SORT updated DESC
```

## Décisions actives

```dataview
TABLE project, status, confidence, updated, file.outlinks
FROM "60_Decisions"
WHERE status = "active"
SORT updated DESC
```

## Recherches actives

```dataview
TABLE project, confidence, updated, source, file.outlinks
FROM "30_Resources/Research"
WHERE status = "active"
SORT updated DESC
```

## Notes liées

- [[Hermes Obsidian Memory Protocol]]
- [[2026-06-22 - Bonnes pratiques mémoire Obsidian-Hermes 2026]]
