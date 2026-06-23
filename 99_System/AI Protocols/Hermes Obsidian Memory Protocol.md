---
type: protocol
status: active
tags:
  - hermes
  - obsidian
  - memory
  - protocol
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


## Protocole 2026 pour résultats optimaux

1. Toujours considérer Obsidian comme mémoire longue structurée quand la demande touche un projet, une décision, une recherche, une réunion, une procédure, une personne ou un contexte durable.
2. Chercher et lire les notes pertinentes avant de demander à Hugo de répéter un contexte probablement déjà stocké.
3. Ne sauvegarder que les informations durables, utiles, sourcées, non dupliquées et non sensibles.
4. Classer la mémoire : semantic facts, episodic examples, procedural rules, summaries.
5. Utiliser `status`, `confidence`, `source`, `last_verified`, `review_after`, `supersedes` et `superseded_by` quand une note peut vieillir ou entrer en conflit.
6. En cas de conflit, préférer l'instruction utilisateur courante, puis les faits explicites récents, puis les notes actives high confidence ; marquer l'ancien contenu comme remplacé plutôt que garder deux vérités actives.
7. Créer des liens utiles `[[...]]` et vérifier backlinks pour que le graphe aide la récupération.
8. Préférer notes atomiques + index/résumés reliés aux sources plutôt qu'un gros fichier fourre-tout.
9. Maintenir des dashboards qualité : notes orphelines, notes sans type, notes sans updated, low confidence, review_after dépassé.
10. Après toute écriture MCP, relire le fichier ou la section pour vérifier.

Référence recherche : [[2026-06-22 - Bonnes pratiques mémoire Obsidian-Hermes 2026]].
Dashboard qualité : [[Memory Quality Dashboard]].


11. Configuration MCP recommandée sur cette machine : allowlist stricte des outils sûrs (`vault_list`, `vault_read`, `vault_append`, `vault_patch`, `vault_get_document_map`, `active_file_get_path`, `periodic_note_get_path`, `search_query`, `search_simple`, `tag_list`, `open_file`). Ne pas exposer par défaut `vault_delete`, `command_execute`, `vault_write` ou `vault_move`.

## Règles de liaison

- Ajouter des liens Obsidian `[[...]]` quand une note dépend clairement d'une autre note existante.
- Lier les notes projet vers leurs décisions, recherches, meetings et procédures associées.
- Lier les décisions vers le projet concerné et vers les protocoles/sources utilisés.
- Ne pas créer de liens artificiels juste pour densifier le graphe : chaque lien doit aider à retrouver le contexte.
- Quand une note pertinente existe déjà, préférer la mettre à jour et créer une liaison plutôt que créer un doublon.

Notes liées : [[2026-06-22 - Architecture mémoire Obsidian-Hermes]], [[2026-06-22 - Review câblage Obsidian Hermes MCP]], [[Hermes Launcher Obsidian Autostart]].
