---
type: research
status: active
project: Hermes Obsidian Memory
area: AI Agents
tags: [research, obsidian, hermes, memory, rag, pkm, mcp]
created: 2026-06-22
updated: 2026-06-22
confidence: high
source: web + official docs + synthesis agent
owner: hugo
ai_use: true
---

# 2026-06-22 - Bonnes pratiques mémoire Obsidian-Hermes 2026

## Question

Comment configurer et utiliser la mémoire Obsidian connectée à Hermes pour obtenir une mémoire pertinente, durable, sûre et réellement utile aux agents en 2026 ?

## Synthèse courte

La meilleure approche est une architecture en trois couches :

1. **Hermes memory** : faits courts, stables, injectés au démarrage.
2. **Hermes skills** : procédures réutilisables qui guident le comportement agent.
3. **Obsidian** : mémoire longue structurée, lisible par humain, riche en contexte, liens, sources, décisions, recherches et notes projet.

Obsidian doit être traité comme une base de connaissance Markdown structurée : frontmatter stable, notes atomiques, liens utiles, sources, confiance, statut, dates de revue, tableaux Dataview/Bases, et protocole de consolidation.

## Recommandations principales

### 1. Utiliser un frontmatter stable comme API agent

Champs recommandés pour les notes importantes :

```yaml
---
type: project | decision | skill | concept | meeting | person | source | research | daily | protocol | index | semantic | episodic | procedural | summary
scope: user | project | tool | repo | system | preference | area
status: active | draft | review | archived | superseded | deprecated
project:
area:
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
last_verified:
review_after:
confidence: low | medium | high
source: user_explicit | user_implicit | tool_output | web_source | meeting | agent_inferred | synthesis
source_url:
source_note:
supersedes:
superseded_by:
owner: hugo
ai_use: true
---
```

Règles :
- garder les noms de propriétés stables ;
- utiliser les dates ISO ;
- ne pas multiplier les tags inutilement ;
- utiliser `status`, `confidence`, `source`, `review_after` pour éviter la mémoire polluée ou périmée ;
- marquer les anciennes notes `superseded` plutôt que garder deux vérités actives.

### 2. Garder la taxonomie de mémoire explicite

- **Semantic memory** : faits durables, préférences, environnement, contexte projet stable.
- **Episodic memory** : exemples de workflows passés ou incidents réutilisables.
- **Procedural memory** : règles/procédures ; dans Hermes skills quand cela doit guider l'agent.
- **Summaries** : résumés de routage, jamais substituts aux notes sources.

### 3. Notes atomiques mais pas fragmentées

Bon niveau de granularité :
- une décision ;
- une préférence durable ;
- un contexte projet ;
- une recherche synthétisée ;
- un profil personne ;
- une procédure.

À éviter :
- un énorme fichier fourre-tout ;
- des micro-notes sans provenance ;
- dupliquer le même fait dans plusieurs notes actives.

### 4. Liens utiles, pas décoratifs

Créer des liens `[[...]]` quand ils servent la récupération :
- projet → décisions, recherches, meetings, procédures ;
- décision → projet, protocole, sources ;
- recherche → décisions/projets qui l'utilisent ;
- meeting → projet, personnes, décisions ;
- mémoire utilisateur → source/provenance.

Les backlinks doivent permettre à Hermes de reconstruire le contexte sans relire tout le vault.

### 5. Recherche hybride et récupération minimale

Pour une tâche future, l'agent doit :
1. classifier la demande ;
2. chercher dans Obsidian quand le sujet concerne projet/décision/recherche/procédure/contexte long ;
3. filtrer mentalement ou via frontmatter : `status=active`, bon projet/scope, confiance suffisante ;
4. lire les notes les plus pertinentes ;
5. injecter seulement le contexte utile ;
6. citer les chemins de notes quand pertinent.

Principe RAG : ne pas compter uniquement sur recherche sémantique ; les chemins, tags, titres, aliases, liens, commandes et noms de fichiers sont souvent mieux retrouvés par recherche lexicale/hybride.

### 6. Pipeline de consolidation

```text
Capture brute / daily / inbox
  ↓
Note source ou meeting
  ↓
Synthèse durable : project, decision, concept, semantic memory
  ↓
Liens + frontmatter + index Dataview
  ↓
Revue périodique / supersession / archivage
```

Ne pas transformer chaque détail de conversation en mémoire durable.

### 7. Sécurité MCP

Pour Obsidian via MCP :
- préférer `vault_read`, `vault_list`, `vault_get_document_map`, `vault_patch`, `vault_append`, `search_simple`, `search_query`, `tag_list`, `open_file` ;
- éviter par défaut `vault_delete`, `vault_move`, `vault_write`, `command_execute` ;
- préférer allowlist à blacklist quand possible ;
- garder la clé API dans `~/.hermes/.env`, jamais dans le vault ;
- privilégier HTTPS localhost `https://127.0.0.1:27124/mcp/` ;
- vérifier après écriture par relecture MCP.

### 8. Maintenance qualité

Tableaux de contrôle recommandés :
- notes sans `type` ;
- notes sans `updated` ;
- notes actives avec `confidence=low` ;
- notes `review_after` dépassé ;
- notes orphelines sans liens ;
- notes `superseded` encore liées comme actives ;
- projets actifs non mis à jour récemment.

Cadence :
- par session : capturer seulement ce qui est durable ;
- hebdomadaire : traiter inbox, orphelins, low confidence ;
- mensuelle : revoir les notes stale, fusionner doublons, archiver projets inactifs.

## Protocole agent recommandé

Avant de répondre sur un sujet durable :
1. Charger le skill pertinent si disponible.
2. Chercher Obsidian si le sujet touche un projet, une décision, une recherche, une réunion, une procédure, une personne ou un contexte long.
3. Lire les notes pertinentes, pas seulement les snippets.
4. Appliquer priorité : instruction actuelle utilisateur > mémoire explicite utilisateur > sortie outil récente > note active high confidence > note ancienne/inférée.
5. Sauver seulement si la nouvelle information est durable, utile, sourcée et non dupliquée.
6. En cas de conflit, marquer l'ancien contenu `superseded` ou créer un lien `superseded_by`.
7. Vérifier chaque écriture par relecture.

## Sources

- Hermes MCP docs : https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp
- Hermes MCP guide : https://hermes-agent.nousresearch.com/docs/guides/use-mcp-with-hermes
- Hermes MCP config reference : https://hermes-agent.nousresearch.com/docs/reference/mcp-config-reference
- Hermes memory docs : https://hermes-agent.nousresearch.com/docs/user-guide/features/memory
- Hermes skills docs : https://hermes-agent.nousresearch.com/docs/user-guide/features/skills
- Hermes security docs : https://hermes-agent.nousresearch.com/docs/user-guide/security
- Obsidian Local REST API README : https://github.com/coddingtonbear/obsidian-local-rest-api
- Obsidian Local REST API OpenAPI : https://coddingtonbear.github.io/obsidian-local-rest-api/openapi.yaml
- Obsidian Properties : https://help.obsidian.md/properties
- Obsidian links : https://help.obsidian.md/links
- Obsidian backlinks : https://help.obsidian.md/plugins/backlinks
- Obsidian Bases : https://help.obsidian.md/bases
- Dataview docs : https://blacksmithgu.github.io/obsidian-dataview/
- PARA : https://fortelabs.com/blog/para/
- Zettelkasten intro : https://zettelkasten.de/introduction/
- Evergreen atomic notes : https://notes.andymatuschak.org/Evergreen_notes_should_be_atomic
- LangChain memory concepts : https://docs.langchain.com/oss/python/concepts/memory
- LlamaIndex RAG optimization : https://developers.llamaindex.ai/python/framework/optimizing/basic_strategies/basic_strategies/
- LlamaIndex retrieval evaluation : https://docs.llamaindex.ai/en/stable/examples/evaluation/retrieval/retriever_eval/
- Ragas metrics : https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/
- Pinecone chunking strategies : https://www.pinecone.io/learn/chunking-strategies/
- Pinecone metadata filtering : https://docs.pinecone.io/guides/search/filter-by-metadata
- Qdrant filtering : https://qdrant.tech/documentation/concepts/filtering/
- Weaviate hybrid search : https://weaviate.io/developers/weaviate/search/hybrid
- Anthropic effective agents : https://www.anthropic.com/engineering/building-effective-agents

## Notes liées

- [[Hermes Obsidian Memory Protocol]]
- [[2026-06-22 - Architecture mémoire Obsidian-Hermes]]
- [[2026-06-22 - Review câblage Obsidian Hermes MCP]]
- [[Hermes Launcher Obsidian Autostart]]
