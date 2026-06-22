---
type: index
status: active
tags: [index, projects]
created: 2026-06-22
updated: 2026-06-22
ai_use: true
---

# Projets actifs

```dataview
TABLE status, area, updated
FROM "10_Projects"
WHERE type = "project" AND status != "archived"
SORT updated DESC
```
