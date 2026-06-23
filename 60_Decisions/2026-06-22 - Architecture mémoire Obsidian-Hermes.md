---
type: decision
status: active
project: Hermes Obsidian Memory
area: AI Agents
tags: [decision, obsidian, hermes, mcp, memory]
created: 2026-06-22
updated: 2026-06-22
confidence: high
owner: hugo
ai_use: true
---

# 2026-06-22 - Architecture mémoire Obsidian-Hermes

## Décision

Utiliser Obsidian comme mémoire longue structurée de Hugo, connectée à Hermes via le plugin Obsidian `Local REST API with MCP` et la configuration MCP HTTP native de Hermes.



Protocole associé : [[Hermes Obsidian Memory Protocol]].

## Chemins

- Vault : `/Users/hugo/Documents/Obsidian/Hugo-Knowledge`
- Configuration Hermes : `/Users/hugo/.hermes/config.yaml`
- Secret API Obsidian : stocké hors vault dans l'environnement Hermes
- Endpoint MCP : `https://127.0.0.1:27124/mcp/`

## Pourquoi

- Obsidian reste lisible et maintenable par humain.
- Hermes garde sa mémoire interne légère et rapide.
- Les notes longues, recherches, décisions et contextes projets vivent dans un vault versionné.
- MCP permet à l'agent de lire, chercher et patcher précisément le vault sans serveur Python intermédiaire.

## Options considérées

1. MCP intégré du plugin Local REST API with MCP — retenu.
2. Serveur Python `mcp-obsidian` séparé — rejeté pour éviter une dépendance stdio/uvx supplémentaire.
3. Simple accès filesystem sans Obsidian API — rejeté car moins riche pour search/patch/active file/commands.

## Plugins installés

- Local REST API with MCP
- Dataview
- Templater
- Tasks
- Obsidian Git
- Smart Connections

## Règles sécurité

- Ne jamais écrire de secrets dans les notes.
- Ne jamais supprimer de fichier du vault sans confirmation explicite.
- Préférer les patchs ciblés aux réécritures complètes.
- Versionner le vault avec Git, mais exclure la config locale contenant la clé API du plugin REST.

## État de vérification

- Vault créé.
- Plugins téléchargés dans `.obsidian/plugins/`.
- Configuration MCP Hermes ajoutée.
- Skill Hermes `obsidian-memory-workflow` créé.
- Obsidian a été ouvert et le plugin Local REST API with MCP répond sur `127.0.0.1:27124`.
- `hermes mcp test obsidian` connecte et découvre 16 outils.
- Les outils dangereux `vault_delete` et `command_execute` sont exclus de la configuration runtime.
- Le toolset `mcp-obsidian` est activé dans `~/.hermes/config.yaml` avec `hermes-cli`.
- Vérification finale réussie dans une session Hermes fraîche : l'agent a utilisé l'outil MCP Obsidian pour lister la racine du vault.
