---
type: index
status: active
tags: [index, decisions]
created: 2026-06-22
updated: 2026-06-22
ai_use: true
---

# Décisions

```dataview
TABLE project, area, confidence, updated
FROM "60_Decisions"
WHERE type = "decision"
SORT updated DESC
```
