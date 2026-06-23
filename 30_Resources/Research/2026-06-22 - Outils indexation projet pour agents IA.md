---
type: research
status: done
project:
area: ai-agents
tags:
  - ai-agents
  - code-indexing
  - obsidian
  - mcp
  - sourcegraph
created: 2026-06-22
updated: 2026-06-22
confidence: high
source: web research via Hermes subagents; official docs/GitHub primarily
owner: hugo
ai_use: true
---

# Outils d'indexation de projet pour agents IA

## Question
Quels outils actuels augmentent le plus les performances d'agents IA de code comme Claude Code, Codex et Hermes sur des projets complexes, avec Obsidian comme mémoire/index de projet ?

## Synthèse courte
La meilleure architecture n'est pas un seul plugin Obsidian. C'est une pile en couches :

1. Obsidian comme mémoire longue structurée du projet.
2. Local REST API with MCP pour que les agents lisent/patchent les notes.
3. Serena MCP pour donner aux agents des outils sémantiques de code locaux, proches d'un IDE/LSP.
4. Sourcegraph/SCIP si plusieurs gros repos ou besoin de recherche/navigation cross-repo durable.
5. Aider repo-map comme résumé local léger du repo.
6. Omnisearch + Dataview + Obsidian Git pour rendre le vault robuste, cherchable et auditable.
7. Option avancée : tree-sitter + LSP + Qdrant + Semgrep/OpenGrep + éventuellement Joern/Neo4j GraphRAG.

## Recommandation pour Hugo

### Stack minimale à installer/standardiser
- Obsidian Local REST API with MCP : pont agent <-> vault.
- Dataview : couche metadata/frontmatter.
- Obsidian Git : rollback/audit des edits IA.
- Omnisearch : recherche lexicale BM25 rapide.
- Smart Connections : recherche sémantique locale dans Obsidian.
- Serena MCP : outils code sémantiques locaux pour Claude/Codex/Hermes.
- Aider repo-map : aperçu local de repo utile comme référence et fallback.

### Stack premium si gros besoin multi-repo
- Sourcegraph self-hosted ou cloud + SCIP indexes.
- Sourcegraph MCP/API pour recherche, symboles, références, historique et code navigation.
- Semgrep/OpenGrep pour static analysis contextualisé.
- Qdrant ou autre vector DB hybride pour recherche sémantique code/docs.
- Tree-sitter pour parsing/chunking/symboles locaux.
- Joern/Code Property Graph + Neo4j GraphRAG uniquement si besoin profond d'analyse data-flow/sécurité/architecture.

## Classement pratique

1. Serena MCP — meilleur gain local immédiat pour agents de code.
2. Sourcegraph/SCIP — meilleur index durable multi-repo et impact analysis.
3. Obsidian Local REST API with MCP — indispensable pour connecter la mémoire Obsidian.
4. Omnisearch + Dataview + Obsidian Git — fondation fiable du vault.
5. Aider repo-map — lightweight repo overview efficace.
6. Cursor codebase indexing — très bon si workflow Cursor accepté, mais fermé/hébergé.
7. Smart Connections / Copilot for Obsidian — utile côté Obsidian, moins comme index principal externe.
8. Tree-sitter + LSP + vector DB custom — excellent si on accepte de construire/maintenir.
9. Semgrep/OpenGrep — très utile pour review/sécurité, pas index général.
10. Joern/Neo4j GraphRAG — puissant mais lourd.

## Sources principales
- Serena: https://github.com/oraios/serena
- Aider repo-map: https://aider.chat/docs/repomap.html
- Sourcegraph precise code navigation: https://sourcegraph.com/docs/code-navigation/precise-code-navigation
- Sourcegraph auto-indexing: https://sourcegraph.com/docs/code-navigation/auto-indexing
- Sourcegraph MCP/API: https://sourcegraph.com/docs/api/mcp
- Cursor codebase indexing/search: https://cursor.com/docs/agent/tools/search
- Claude Code docs: https://code.claude.com/docs/en/overview
- Codex MCP: https://developers.openai.com/codex/mcp
- Obsidian Local REST API with MCP: https://github.com/coddingtonbear/obsidian-local-rest-api
- Smart Connections: https://github.com/brianpetro/obsidian-smart-connections
- Obsidian Copilot: https://github.com/logancyang/obsidian-copilot
- Dataview: https://github.com/blacksmithgu/obsidian-dataview
- Obsidian Git: https://github.com/Vinzent03/obsidian-git
- Omnisearch: https://github.com/scambier/obsidian-omnisearch
- Tree-sitter: https://tree-sitter.github.io/tree-sitter/
- SCIP: https://github.com/scip-code/scip
- Semgrep: https://docs.semgrep.dev/semgrep-code/overview
- OpenGrep: https://github.com/opengrep/opengrep
- Joern CPG: https://docs.joern.io/code-property-graph/
- Neo4j GraphRAG: https://neo4j.com/docs/neo4j-graphrag-python/current/
- Qdrant: https://qdrant.tech/documentation/overview/

## Notes d'architecture
Un bon système pour agents doit combiner :
- recherche exacte : ripgrep/Omnisearch/Sourcegraph;
- navigation symbolique : LSP/SCIP/Serena;
- graphe de dépendances : repo-map, SCIP, Sourcegraph, CPG;
- recherche sémantique : embeddings/hybrid vector DB;
- metadata projet : Obsidian frontmatter/Dataview;
- historique/provenance : Git/Sourcegraph;
- interface agent : MCP.

Les embeddings seuls ne suffisent pas pour savoir quels fichiers seront impactés. Ils doivent être complétés par symboles, références, imports, graphes de dépendances et historique Git.

## Prochaine action recommandée
Faire un audit de l'environnement local Hugo : quels plugins Obsidian sont déjà installés, quels MCP Claude/Codex/Hermes sont actifs, quels repos méritent Serena/Sourcegraph, puis proposer un plan d'installation incrémental.
