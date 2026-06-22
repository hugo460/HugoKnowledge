---
type: protocol
status: active
tags: [hermes, obsidian, memory, protocol]
created: 2026-06-22
updated: 2026-06-22
ai_use: true
---

# Hermes Obsidian Memory Protocol

## Objectif

Utiliser Obsidian comme mémoire longue structurée, lisible par humain et exploitable par agent.

## Ce qui va dans Hermes memory

- Préférences stables de Hugo.
- Faits courts sur l'environnement.
- Chemins importants.

## Ce qui va dans Obsidian

- Recherches longues.
- Décisions.
- Specs.
- Réunions.
- Contexte projet.
- Notes atomiques.

## Règles d'écriture agent

1. Chercher avant de créer une note doublon.
2. Utiliser le frontmatter standard.
3. Mettre à jour `updated`.
4. Ne jamais supprimer sans confirmation explicite.
5. Ne jamais écrire de secrets/API keys/tokens.
6. Préférer les patchs ciblés aux réécritures complètes.
