---
type: protocol
status: active
project: Hermes Obsidian Memory
area: AI Agents
tags: [hermes, obsidian, launcher, mcp]
created: 2026-06-22
updated: 2026-06-22
confidence: high
owner: hugo
ai_use: true
---

# Hermes Launcher Obsidian Autostart

## Objectif

Les commandes shell `hm` et `hy` doivent lancer Hermes avec la mémoire Obsidian disponible. Si Obsidian n'est pas ouvert, elles démarrent automatiquement le vault mémoire avant de lancer Hermes.

## Fichiers

- Wrapper : `/Users/hugo/.local/bin/hermes-with-obsidian`
- Configuration shell : `/Users/hugo/.zshrc`
- Vault : `/Users/hugo/Documents/Obsidian/Hugo-Knowledge`
- Endpoint health : `https://127.0.0.1:27124/`

## Fonctionnement

Le wrapper :

1. vérifie le health endpoint Local REST API with MCP ;
2. si l'endpoint n'est pas prêt, ouvre Obsidian sur le vault Hugo-Knowledge ;
3. attend jusqu'à 25 secondes que REST/MCP réponde ;
4. lance `hermes --yolo` avec les arguments reçus ;
5. si Obsidian ne devient pas prêt, lance quand même Hermes mais affiche un warning.

## Commandes

Vérifier seulement Obsidian sans lancer Hermes :

```bash
hm --obsidian-check-only
```

Lancer Hermes normalement :

```bash
hm
hy
```

Lancer Hermes sans vérifier Obsidian, seulement si nécessaire :

```bash
hm --obsidian-skip-check
```

## Validation

- `hm --obsidian-check-only` retourne `✓ Obsidian memory vault REST/MCP ready`.
- `hy --version` vérifie Obsidian puis affiche la version Hermes.
- `hermes mcp test obsidian` connecte et découvre 16 outils.

## Notes

Cette intégration évite de lancer Hermes avec le toolset `mcp-obsidian` alors que le serveur MCP Obsidian est absent. Elle garde le système local-first, simple et maintenable sans daemon LaunchAgent supplémentaire.
