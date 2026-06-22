---
type: index
status: active
tags: [obsidian, memory, hermes]
created: 2026-06-22
updated: 2026-06-22
ai_use: true
---

# Hugo Knowledge Vault

Ce vault est la mémoire longue structurée de Hugo pour Hermes, Obsidian et les autres agents.

## Règles

- Les secrets ne doivent jamais être écrits dans les notes.
- Toute note importante doit avoir un frontmatter YAML.
- Les décisions durables vont dans `60_Decisions/`.
- Les procédures réutilisables vont dans `50_Skills/` ou dans les skills Hermes.
- Les captures rapides vont dans `00_Inbox/` puis sont triées.
- Les notes de projet vivantes vont dans `10_Projects/`.

## Connexion agent

Le plugin `Local REST API with MCP` expose ce vault à Hermes via `https://127.0.0.1:27124/mcp/`.
La clé API est stockée dans `~/.hermes/.env`, pas dans les notes.
