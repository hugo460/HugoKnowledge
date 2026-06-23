---
type: research
status: done
project: null
area: developer-tooling
tags:
  - ai-agents
  - agents-ia
  - developer-tooling
  - code-indexing
  - project-indexing
  - obsidian
  - mcp
  - claude-code
  - codex
  - sourcegraph
  - serena
  - rag
aliases:
  - Rapport indexation projet agents IA
  - Meilleurs outils indexation Obsidian agents IA
  - Codebase indexing Claude Codex Hermes
  - Stack Obsidian pour agents de code
  - Outillage indexation codebase projets IA
created: 2026-06-22
updated: 2026-06-22
confidence: high
source: Recherche web approfondie via Hermes; sources officielles/GitHub/docs
  principalement
owner: hugo
ai_use: true
related:
  - "[[2026-06-22 - Outils indexation projet pour agents IA]]"
  - "[[Developer AI Tooling Index]]"
---

# Rapport détaillé — Outils d'indexation projet, Obsidian et agents IA

> Rapport long terme pour retrouver plus tard la recherche sur les meilleurs outils d'indexation de projet/codebase afin d'améliorer les performances de Claude Code, Codex, Hermes et autres agents IA sur des projets complexes.

## Emplacement et liens

Cette note est classée dans `30_Resources/Research/` car c'est une recherche technique long terme sur l'outillage d'indexation de code/projets pour les développements de Hugo.

Elle ne doit pas être traitée comme une note sur la mémoire Hermes. Obsidian apparaît ici comme un outil possible dans la stack de développement, au même titre que Serena, Sourcegraph, Dataview, Omnisearch, MCP, etc.

Liens pertinents :

- Synthèse courte : [[2026-06-22 - Outils indexation projet pour agents IA]]
- Index thématique outillage dev IA : [[Developer AI Tooling Index]]

## Résumé exécutif

La conclusion principale est nette : il n'existe pas aujourd'hui un seul outil magique qui remplace toute la chaîne d'indexation projet. Les meilleurs résultats viennent d'une architecture en couches qui combine :

1. Une mémoire projet humaine et agent-readable dans Obsidian.
2. Une interface MCP pour exposer cette mémoire et les outils aux agents.
3. Une intelligence sémantique locale du code via Serena MCP/LSP.
4. Un index code durable et multi-repo via Sourcegraph/SCIP si les projets deviennent nombreux ou complexes.
5. Une recherche exacte + sémantique + symbolique + historique, pas seulement des embeddings.
6. Des conventions de notes Obsidian propres : frontmatter, tags, liens, dashboards Dataview, sources et décisions.

Pour Hugo, la meilleure stratégie est progressive :

- Court terme : consolider Obsidian + MCP + Dataview + Omnisearch + Obsidian Git + Smart Connections.
- Court/moyen terme : ajouter Serena MCP pour donner aux agents des capacités proches d'un IDE.
- Moyen terme : utiliser aider repo-map ou logique équivalente pour résumer les repos.
- Moyen/long terme : évaluer Sourcegraph/SCIP comme cerveau code multi-repo.
- Avancé : ajouter Semgrep/OpenGrep, tree-sitter, Qdrant, Joern/CPG ou Neo4j GraphRAG selon les besoins.

## Problème à résoudre

Les agents IA de code échouent souvent sur de gros projets car ils travaillent avec un contexte incomplet :

- ils ne savent pas toujours où chercher ;
- ils scannent trop de fichiers inutilement ;
- ils confondent fichiers similaires ;
- ils ne voient pas toutes les références d'un symbole ;
- ils comprennent mal les dépendances implicites ;
- ils perdent le contexte business/produit stocké hors du repo ;
- ils ne savent pas quelles décisions passées expliquent l'architecture ;
- ils ne savent pas quels fichiers seront impactés par un changement ;
- ils oublient les conventions de projet entre deux sessions.

Le besoin réel est donc un système de contexte multi-couche :

- mémoire long terme ;
- recherche exacte ;
- recherche sémantique ;
- navigation symbolique ;
- graphe de dépendances ;
- historique Git ;
- indexation automatique ou incrémentale ;
- interface standard pour les agents.

## Principe fondamental

Les embeddings seuls ne suffisent pas.

Une vector DB ou une recherche sémantique peut retrouver un fichier qui parle d'un concept, mais elle ne garantit pas :

- la définition exacte d'un symbole ;
- toutes ses références ;
- la chaîne d'appel ;
- les imports impactés ;
- les usages cross-repo ;
- les fichiers à modifier ensemble ;
- les tests concernés ;
- l'historique de décisions.

Un bon système pour agents doit combiner plusieurs index :

| Besoin | Meilleure couche |
|---|---|
| retrouver un terme exact | ripgrep, Omnisearch, Sourcegraph |
| retrouver un concept flou | embeddings, Smart Connections, Qdrant, Copilot |
| trouver définition/références | LSP, SCIP, Serena, Sourcegraph |
| comprendre structure fichier | tree-sitter, LSP, repo-map |
| comprendre impact changement | références, graphe dépendances, Sourcegraph, CPG |
| comprendre décisions projet | Obsidian, ADR, Dataview, backlinks |
| comprendre historique | Git, Sourcegraph, git MCP |
| exposer aux agents | MCP |

## Architecture recommandée pour Hugo

```text
Claude Code / Codex / Hermes
        |
        | MCP tools/resources
        v
Contexte agent orchestré
        |
        |-- Obsidian MCP: mémoire projet, décisions, recherches, protocoles
        |-- Serena MCP: symboles, références, édition sémantique locale
        |-- Git/filesystem tools: diff, historique, fichiers réels
        |-- Sourcegraph/SCIP: recherche/navigation cross-repo si activé
        |-- Omnisearch/Dataview: recherche et structure vault
        |-- Semgrep/OpenGrep: analyse statique, sécurité, qualité
        |-- Vector DB optionnelle: recherche sémantique code/docs
        |-- GraphRAG optionnel: relations architecture/docs/services
```

Objectif : avant de modifier du code, l'agent doit pouvoir :

1. lire la note projet Obsidian ;
2. lire les décisions/ADR liées ;
3. inspecter les symboles/fichiers via Serena/LSP ;
4. chercher références/usages ;
5. regarder git diff/historique ;
6. modifier précisément ;
7. tester ;
8. mettre à jour Obsidian seulement si une information durable a changé.

## Classement global des outils

### 1. Serena MCP

Source : https://github.com/oraios/serena

Serena est probablement le meilleur gain immédiat local pour des agents de code. C'est un toolkit MCP open-source qui expose aux agents des opérations sémantiques sur le code, proches de ce qu'un IDE obtient via LSP.

Points forts :

- local-first ;
- compatible MCP ;
- utilisable par plusieurs agents MCP-compatible ;
- navigation et édition au niveau symboles ;
- utile pour trouver définitions/références ;
- évite les recherches texte aveugles ;
- très adapté aux refactors et gros fichiers.

Limites :

- dépend de la qualité des language servers ;
- pas un index historique multi-repo complet ;
- setup à vérifier projet par projet ;
- ne remplace pas Obsidian pour la mémoire produit/business.

Recommandation Hugo : installer/tester en priorité sur un repo complexe comme arche-des-anges ou devlab-IA, puis brancher à Claude Code/Codex/Hermes si possible.

### 2. Sourcegraph + SCIP

Sources :

- https://sourcegraph.com/docs/code-navigation/precise-code-navigation
- https://sourcegraph.com/docs/code-navigation/auto-indexing
- https://sourcegraph.com/docs/api/mcp
- https://github.com/scip-code/scip

Sourcegraph est le meilleur choix mature pour une indexation codebase/multi-repo durable. SCIP est le protocole d'indexation code intelligence utilisé pour la navigation précise.

Points forts :

- recherche cross-repo ;
- recherche symbolique ;
- définitions/références/hover ;
- recherche dans commits/diffs/historique ;
- indexation automatique possible ;
- très bon pour impact analysis ;
- citations fichier/ligne ;
- adapté aux gros ensembles de repos.

Limites :

- plus lourd à opérer ;
- certaines fonctions avancées sont Enterprise/self-hosted ;
- peut être overkill pour un seul repo ;
- nécessite de penser infra et permissions.

Recommandation Hugo : à évaluer après Serena. Très pertinent si DEVLAB contient plusieurs repos interdépendants où les agents perdent du temps à naviguer.

### 3. Obsidian Local REST API with MCP

Source : https://github.com/coddingtonbear/obsidian-local-rest-api

C'est le pont principal entre Obsidian et les agents externes.

Capacités importantes :

- REST API locale ;
- serveur MCP intégré ;
- lecture/écriture de fichiers du vault ;
- patch ciblé par heading/block/frontmatter ;
- recherche simple ;
- recherche JsonLogic sur metadata ;
- tags ;
- active file ;
- periodic notes ;
- ouverture UI ;
- authentification par API key.

Utilité dans la stack :

- rend Obsidian agent-readable ;
- permet aux agents de mettre à jour des notes sans remplacer tout le fichier ;
- permet de centraliser contexte projet, décisions, recherches, protocoles.

Limites :

- Obsidian doit tourner ;
- attention aux outils destructifs ;
- ne pas exposer command_execute/delete sans contrôle ;
- nécessite une discipline de notes.

Recommandation Hugo : déjà en place côté Hermes. À garder comme socle mémoire long terme.

### 4. Dataview

Source : https://github.com/blacksmithgu/obsidian-dataview

Dataview transforme les notes Markdown en base de données queryable via frontmatter et inline fields.

Rôle :

- structurer les notes ;
- créer dashboards ;
- filtrer par projet/type/status ;
- rendre la mémoire lisible par humain et agent.

Champs recommandés :

```yaml
type: project | decision | research | protocol | source | skill
status: active | draft | done | archived
project: nom-projet
area: ai-agents | ecommerce | devops | architecture
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
confidence: low | medium | high
source:
ai_use: true
```

Recommandation Hugo : obligatoire si le vault devient base mémoire projet.

### 5. Omnisearch

Source : https://github.com/scambier/obsidian-omnisearch

Omnisearch apporte une recherche lexicale/BM25 rapide et plus pertinente que la recherche native.

Points forts :

- recherche instantanée ;
- pertinence BM25/MiniSearch ;
- phrases exactes ;
- exclusions ;
- filtres ;
- typo tolerance ;
- possible serveur HTTP selon configuration ;
- complément idéal de la recherche sémantique.

Recommandation Hugo : installer si absent. Les agents doivent privilégier exact/BM25 pour IDs, noms de fichiers, symboles, termes métier précis.

### 6. Obsidian Git

Source : https://github.com/Vinzent03/obsidian-git

Indispensable si des agents peuvent écrire dans le vault.

Rôle :

- historique ;
- diff ;
- rollback ;
- audit des modifications IA ;
- sync/backup éventuel.

Recommandation Hugo : actif recommandé. Les agents doivent faire des modifications ciblées et vérifiables.

### 7. Smart Connections

Source : https://github.com/brianpetro/obsidian-smart-connections

Smart Connections ajoute une couche sémantique locale dans Obsidian.

Points forts :

- embeddings locaux par défaut ;
- recherche par sens ;
- suggestions de notes connexes ;
- utile pour resurfacer du contexte oublié ;
- bon outil humain dans Obsidian.

Limites :

- pas le backend principal idéal pour agents externes ;
- la recherche sémantique peut manquer un terme exact ;
- à combiner avec Omnisearch et MCP.

Recommandation Hugo : très utile côté Obsidian, mais pas suffisant pour l'indexation code.

### 8. Aider repo-map

Source : https://aider.chat/docs/repomap.html

Aider repo-map crée une carte compacte du repo pour donner au LLM une vue globale.

Points forts :

- léger ;
- local ;
- priorise fichiers/classes/fonctions importants ;
- utilise un graphe/ranking de dépendances ;
- très bon pour contexte initial.

Limites :

- pas serveur d'indexation complet ;
- pas impact analysis exhaustive ;
- moins précis que LSP/SCIP pour références.

Recommandation Hugo : utile comme outil complémentaire ou inspiration pour générer des notes `Fichiers importants` dans Obsidian.

### 9. Cursor codebase indexing

Source : https://cursor.com/docs/agent/tools/search

Cursor a une très bonne expérience IDE intégrée.

Points forts :

- indexation automatique ;
- recherche sémantique ;
- grep ;
- agent explore ;
- sync incrémentale périodique ;
- très bonne UX.

Limites :

- fermé/hébergé ;
- moins contrôlable ;
- difficile d'en faire une couche commune pour Claude/Codex/Hermes ;
- pas idéal comme socle open/local.

Recommandation Hugo : bon si tu travailles dans Cursor, mais ne pas baser toute l'architecture dessus.

### 10. Claude Code et Codex

Sources :

- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/how-claude-code-works
- https://developers.openai.com/codex
- https://developers.openai.com/codex/mcp

Claude Code et Codex sont surtout des agents/runtimes, pas des indexeurs complets.

Ils deviennent beaucoup plus performants si on leur donne :

- MCP Obsidian ;
- Serena MCP ;
- Sourcegraph MCP/API ;
- Git/filesystem tools ;
- règles projet ;
- notes projet structurées.

Recommandation Hugo : ne pas attendre que Claude/Codex “sachent tout” seuls. Leur fournir des outils d'indexation et un protocole de recherche.

### 11. Semgrep / OpenGrep

Sources :

- https://docs.semgrep.dev/semgrep-code/overview
- https://github.com/opengrep/opengrep

Ces outils apportent une analyse statique très utile.

Points forts :

- sécurité ;
- qualité ;
- patterns ;
- conventions ;
- détection d'erreurs cross-file selon capabilities ;
- résultats actionnables.

Limites :

- pas un index général ;
- nécessite règles/config ;
- surtout utile pour review, refactor, sécurité.

Recommandation Hugo : intégrer plus tard comme couche “agent review/security”.

### 12. tree-sitter

Source : https://tree-sitter.github.io/tree-sitter/

tree-sitter est idéal pour construire un indexer local custom : parsing incrémental, AST, chunks propres, symboles.

Points forts :

- rapide ;
- local ;
- incrémental ;
- support multi-langage ;
- excellent pour chunking AST-aware ;
- utile pour créer des contextes RAG fiables.

Limites :

- bibliothèque, pas produit complet ;
- il faut construire l'orchestration autour.

Recommandation Hugo : à utiliser si création d'un outil DEVLAB maison d'indexation.

### 13. Qdrant / vector DB hybride

Source : https://qdrant.tech/documentation/overview/

Qdrant est une bonne option vector DB locale/self-hosted pour recherche sémantique/hybride.

Points forts :

- dense vectors ;
- recherche hybride possible ;
- metadata filtering ;
- self-hostable ;
- bon pour docs + code chunks.

Limites :

- embeddings seuls insuffisants ;
- nécessite chunking, metadata, refresh, citations ;
- doit être couplé à symboles/références.

Recommandation Hugo : option avancée après stabilisation Serena/Obsidian/Sourcegraph.

### 14. Joern / Code Property Graph

Source : https://docs.joern.io/code-property-graph/

Joern/CPG est une couche très avancée pour analyser code, data-flow, control-flow, sécurité.

Points forts :

- graphe riche du code ;
- queries puissantes ;
- sécurité/data-flow ;
- impact profond.

Limites :

- lourd ;
- setup et apprentissage ;
- pas nécessaire pour le MVP.

Recommandation Hugo : seulement si besoin sécurité/data-flow très poussé.

### 15. Neo4j GraphRAG

Source : https://neo4j.com/docs/neo4j-graphrag-python/current/

Neo4j GraphRAG est pertinent pour relier code, docs, services, tickets, décisions, incidents.

Points forts :

- knowledge graph ;
- multi-hop reasoning ;
- relations explicites ;
- utile pour architecture d'entreprise/projets.

Limites :

- ne remplace pas LSP/SCIP/Sourcegraph ;
- demande une stratégie de graphe ;
- coût opérationnel.

Recommandation Hugo : couche long terme si DEVLAB veut un graphe de connaissances multi-projets.

## Outils moins recommandés comme base principale

### Bloop

Source : https://github.com/BloopAI/bloop

Projet archivé/read-only depuis 2025. Intéressant historiquement, mais pas recommandé pour nouvelle architecture 2026.

### Continue

Sources :

- https://docs.continue.dev/
- https://github.com/continuedev/continue

Continue reste intéressant comme assistant open-source, mais n'est pas le meilleur pari actuel pour construire une infrastructure d'indexation neuve, surtout face à Serena/Claude/Codex/Sourcegraph/Cursor.

### Sweep

Source : https://github.com/sweepai/sweep

Moins pertinent comme solution d'indexation principale moderne.

### Vector-only RAG

À éviter comme unique solution. Trop faible pour symboles/références/impact analysis.

## Stack cible proposée

### Stack minimale fiable

Objectif : améliorer immédiatement les agents avec peu de complexité.

Composants :

- Obsidian Local REST API with MCP
- Dataview
- Obsidian Git
- Omnisearch
- Smart Connections
- Serena MCP
- Git + filesystem tools existants

Résultat attendu :

- les agents retrouvent les notes projets ;
- ils lisent les décisions ;
- ils cherchent dans le vault ;
- ils naviguent mieux dans le code ;
- ils modifient moins à l'aveugle ;
- les connaissances durables sont sauvegardées.

### Stack pro multi-repo

Objectif : donner un cerveau code durable à plusieurs projets complexes.

Composants :

- stack minimale ;
- Sourcegraph ;
- indexes SCIP ;
- Sourcegraph MCP/API ;
- Semgrep/OpenGrep ;
- conventions CI pour réindexation.

Résultat attendu :

- recherche cross-repo ;
- références précises ;
- meilleure impact analysis ;
- historique code ;
- citations fiables fichier/ligne.

### Stack avancée DEVLAB maison

Objectif : créer une infrastructure custom d'indexation agentique.

Composants :

- tree-sitter ;
- LSP wrappers ;
- Qdrant ou autre vector DB ;
- graphe dépendances ;
- Semgrep/OpenGrep ;
- Obsidian as memory ;
- MCP server custom ;
- éventuellement Neo4j/Joern.

Résultat attendu :

- système spécialisé aux projets DEVLAB ;
- index incrémental ;
- retrieval hybride ;
- impact analysis plus riche ;
- agents multi-outils.

## Protocole agent recommandé

Quand un agent travaille sur un projet complexe, il devrait suivre ce protocole :

1. Identifier le projet et le repo.
2. Chercher dans Obsidian la note projet par nom/alias/tags.
3. Lire les sections : résumé, architecture, décisions, points sensibles, workflows.
4. Lire les ADR/recherches liées.
5. Inspecter le repo via Serena/LSP : symboles, définitions, références.
6. Chercher exact avec ripgrep/Sourcegraph/Omnisearch selon contexte.
7. Vérifier git status/diff.
8. Planifier les fichiers impactés.
9. Modifier précisément.
10. Exécuter tests/build/lint.
11. Mettre à jour Obsidian seulement pour information durable.
12. Ne pas transformer les notes de recherche/projet en journal temporaire ; ne garder dans Obsidian que les informations durables et utiles aux développements.

## Convention Obsidian recommandée pour projets

Chemins :

```text
10_Projects/<Projet>/<Projet>.md
10_Projects/<Projet>/Architecture.md
10_Projects/<Projet>/Decisions/
10_Projects/<Projet>/Research/
10_Projects/<Projet>/Agent Notes.md
10_Projects/<Projet>/Runbooks/
```

Frontmatter projet :

```yaml
---
type: project
status: active
project: nom-projet
repo_path: /chemin/local/repo
main_stack: []
tags: [project, ai-memory]
created: YYYY-MM-DD
updated: YYYY-MM-DD
confidence: high
ai_use: true
aliases: []
related: []
---
```

Sections projet :

```markdown
## Résumé
## Objectifs business
## Stack technique
## Architecture
## Fichiers importants
## Workflows de développement
## Tests et commandes
## Points sensibles
## Décisions liées
## Sources
## Journal durable
```

## Décision stratégique

Pour les projets Hugo/DEVLAB, Obsidian doit contenir la mémoire durable : décisions, contexte business, architecture, conventions, recherches, liens.

Le code indexer doit contenir l'intelligence code : symboles, références, dépendances, historique, impact analysis.

MCP doit connecter les deux aux agents.

Ne pas essayer de transformer Obsidian seul en index complet de code. Obsidian doit être le cerveau mémoire/documentation, pas le moteur principal de code intelligence.

## Plan d'action futur

### Étape 1 — Audit local

Vérifier :

- plugins Obsidian installés ;
- état Local REST API/MCP ;
- Dataview actif ;
- Omnisearch actif ;
- Obsidian Git actif ;
- Smart Connections actif ;
- MCP Claude Code configurés ;
- MCP Codex configurés ;
- MCP Hermes actifs ;
- repos DEVLAB prioritaires.

### Étape 2 — Installer/tester Serena

Tester sur un repo prioritaire :

- arche-des-anges ;
- devlab-IA ;
- autre gros repo DEVLAB.

Valider :

- liste symboles ;
- définition ;
- références ;
- édition ciblée ;
- compatibilité Claude/Codex/Hermes.

### Étape 3 — Standardiser notes projet

Créer/mettre à jour une note projet par repo prioritaire avec :

- architecture ;
- fichiers clés ;
- commandes ;
- décisions ;
- points sensibles ;
- protocoles agent.

### Étape 4 — Évaluer Sourcegraph

Critères d'adoption :

- plus de 3-5 repos interdépendants ;
- agents perdent trop de temps à chercher ;
- besoin de références cross-repo ;
- besoin d'historique/code search durable ;
- besoin de citations précises.

### Étape 5 — Ajouter static analysis

Ajouter Semgrep/OpenGrep pour :

- sécurité ;
- qualité ;
- conventions ;
- review automatique ;
- règles projet.

### Étape 6 — Envisager index DEVLAB custom

Uniquement si les outils standards ne suffisent pas :

- tree-sitter ;
- LSP ;
- Qdrant ;
- graphe dépendances ;
- MCP custom ;
- Neo4j/Joern si besoin.

## Sources détaillées

### Code indexing / agents

- Serena : https://github.com/oraios/serena
- Aider repo-map : https://aider.chat/docs/repomap.html
- Sourcegraph precise code navigation : https://sourcegraph.com/docs/code-navigation/precise-code-navigation
- Sourcegraph auto-indexing : https://sourcegraph.com/docs/code-navigation/auto-indexing
- Sourcegraph MCP/API : https://sourcegraph.com/docs/api/mcp
- SCIP : https://github.com/scip-code/scip
- Cursor codebase search/indexing : https://cursor.com/docs/agent/tools/search
- Claude Code overview : https://code.claude.com/docs/en/overview
- Claude Code architecture : https://code.claude.com/docs/en/how-claude-code-works
- OpenAI Codex : https://developers.openai.com/codex
- Codex MCP : https://developers.openai.com/codex/mcp

### Obsidian / mémoire

- Local REST API with MCP : https://github.com/coddingtonbear/obsidian-local-rest-api
- Smart Connections : https://github.com/brianpetro/obsidian-smart-connections
- Obsidian Copilot : https://github.com/logancyang/obsidian-copilot
- Dataview : https://github.com/blacksmithgu/obsidian-dataview
- Obsidian Git : https://github.com/Vinzent03/obsidian-git
- Omnisearch : https://github.com/scambier/obsidian-omnisearch
- Excalibrain : https://github.com/zsviczian/excalibrain
- Graph Analysis : https://github.com/SkepticMystic/graph-analysis

### Architectures avancées

- MCP resources spec : https://modelcontextprotocol.io/specification/2025-06-18/server/resources
- MCP filesystem server : https://raw.githubusercontent.com/modelcontextprotocol/servers/main/src/filesystem/README.md
- MCP git server : https://raw.githubusercontent.com/modelcontextprotocol/servers/main/src/git/README.md
- LSP : https://microsoft.github.io/language-server-protocol/
- tree-sitter : https://tree-sitter.github.io/tree-sitter/
- Semgrep : https://docs.semgrep.dev/semgrep-code/overview
- OpenGrep : https://github.com/opengrep/opengrep
- Joern Code Property Graph : https://docs.joern.io/code-property-graph/
- Neo4j GraphRAG : https://neo4j.com/docs/neo4j-graphrag-python/current/
- Qdrant : https://qdrant.tech/documentation/overview/
- Microsoft GraphRAG : https://microsoft.github.io/graphrag/

## Mots-clés pour retrouver cette note

indexation projet, codebase indexing, Obsidian, agents IA, Claude Code, Codex, Hermes, MCP, Serena, Sourcegraph, SCIP, Smart Connections, Dataview, Omnisearch, Obsidian Git, Aider repo-map, tree-sitter, LSP, Semgrep, OpenGrep, Qdrant, GraphRAG, Joern, Neo4j, impact analysis, réindexation automatique, recherche sémantique, recherche symbolique, mémoire projet.

## Conclusion finale

La meilleure trajectoire pour augmenter considérablement les performances des agents IA sur les projets complexes est :

1. rendre Obsidian proprement exploitable comme mémoire durable ;
2. brancher les agents via MCP ;
3. ajouter Serena pour intelligence code locale ;
4. ajouter Sourcegraph/SCIP si le besoin multi-repo/impact analysis devient important ;
5. compléter avec Omnisearch, Dataview, Obsidian Git et Smart Connections ;
6. éviter le piège du vector-only RAG ;
7. construire éventuellement une couche custom tree-sitter/LSP/vector/graph si DEVLAB veut une infrastructure propriétaire.

Cette note doit servir de référence future pour choisir et implémenter l'architecture d'indexation projet/agents IA.
