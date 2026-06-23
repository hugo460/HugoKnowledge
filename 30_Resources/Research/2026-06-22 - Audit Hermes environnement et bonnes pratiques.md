---
type: research
status: active
project: Hermes Obsidian Memory
area: AI Agents
tags: [research, hermes, obsidian, mcp, audit, tools, configuration]
created: 2026-06-22
updated: 2026-06-22
last_verified: 2026-06-22
confidence: high
source: official Hermes docs + local CLI audit + Obsidian MCP verification
owner: hugo
ai_use: true
---

# 2026-06-22 - Audit Hermes environnement et bonnes pratiques

## Question

Comment exploiter au maximum Hermes Agent, Obsidian et l’environnement local de Hugo avec une configuration robuste, sûre, maintenable et orientée progression autonome ?

## Sources principales consultées

Documentation officielle Hermes Agent consultée le 2026-06-22 :

- https://hermes-agent.nousresearch.com/docs/user-guide/configuration
- https://hermes-agent.nousresearch.com/docs/user-guide/configuring-models
- https://hermes-agent.nousresearch.com/docs/user-guide/features/tools
- https://hermes-agent.nousresearch.com/docs/reference/toolsets-reference
- https://hermes-agent.nousresearch.com/docs/reference/tools-reference
- https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp
- https://hermes-agent.nousresearch.com/docs/guides/use-mcp-with-hermes
- https://hermes-agent.nousresearch.com/docs/reference/mcp-config-reference
- https://hermes-agent.nousresearch.com/docs/user-guide/features/memory
- https://hermes-agent.nousresearch.com/docs/user-guide/features/skills
- https://hermes-agent.nousresearch.com/docs/guides/work-with-skills
- https://hermes-agent.nousresearch.com/docs/user-guide/features/cron
- https://hermes-agent.nousresearch.com/docs/guides/automate-with-cron
- https://hermes-agent.nousresearch.com/docs/guides/cron-script-only
- https://hermes-agent.nousresearch.com/docs/user-guide/features/fallback-providers
- https://hermes-agent.nousresearch.com/docs/user-guide/features/plugins
- https://hermes-agent.nousresearch.com/docs/user-guide/features/built-in-plugins
- https://hermes-agent.nousresearch.com/docs/user-guide/features/tool-search
- https://hermes-agent.nousresearch.com/docs/user-guide/security
- https://hermes-agent.nousresearch.com/docs/developer-guide/context-compression-and-caching
- https://hermes-agent.nousresearch.com/docs/developer-guide/prompt-assembly

Notes Obsidian liées : [[Hermes Obsidian Memory Protocol]], [[2026-06-22 - Bonnes pratiques mémoire Obsidian-Hermes 2026]], [[2026-06-22 - Review câblage Obsidian Hermes MCP]], [[2026-06-22 - Architecture mémoire Obsidian-Hermes]].

## Synthèse exécutive

L’environnement Hermes de Hugo est déjà très solide : Hermes v0.17.0 à jour, modèle principal `gpt-5.5` via `openai-codex`, Obsidian connecté par MCP local HTTPS avec allowlist stricte, mémoire Hermes compacte, skills locales pertinentes, historique de sessions FTS5, et wrapper `hermes-with-obsidian` pour ouvrir le vault automatiquement.

Actions appliquées pendant l’audit :

- `ripgrep` installé et vérifié (`rg 15.1.0`) pour accélérer les recherches fichiers.
- `cua-driver` installé et vérifié (`cua-driver 0.6.5`) pour activer `computer_use` côté Hermes.
- `agent-browser` installé globalement (`agent-browser 0.29.1`) pour satisfaire le backend navigateur local.
- `websockets==15.0.1` installé dans l’environnement Python Hermes pour résoudre l’import manquant du module `tools.browser_dialog_tool`.
- Obsidian MCP revérifié : serveur `obsidian` enabled, 16 outils côté serveur, 11 outils exposés par allowlist.

État restant : optimal pour l’usage CLI local + Obsidian, mais pas encore “maximal” côté web/API/providers/automatisation autonome, faute de clés/API ou choix utilisateur : web search provider, fallback providers, gateway permanent, cron jobs, permissions macOS computer-use, plugins utiles à activer.

## État local vérifié

### Version et modèle

- Hermes Agent : `v0.17.0 (2026.6.19)`, à jour.
- Python Hermes : `3.14.6`.
- Modèle principal : `gpt-5.5`.
- Provider principal : `openai-codex`, authentifié via OAuth ChatGPT/Codex.
- Aucun fallback provider configuré.
- Aucun cron job configuré.
- Gateway service arrêté.

### Config principale

Fichier : `/Users/hugo/.hermes/config.yaml`

Points importants :

- `toolsets`: `hermes-cli` + `mcp-obsidian`.
- `agent.max_turns`: 90.
- `agent.tool_use_enforcement`: `auto`.
- `agent.parallel_tool_call_guidance`: true.
- `agent.environment_probe`: true.
- `terminal.backend`: `local`.
- `terminal.persistent_shell`: true.
- `compression.enabled`: true.
- `compression.threshold`: 0.5.
- `compression.codex_gpt55_autoraise`: true, donc Hermes peut relever automatiquement le seuil à 85% pour `gpt-5.5` via OpenAI Codex OAuth.
- `context.engine`: `compressor`.
- `memory.memory_enabled`: true.
- `memory.user_profile_enabled`: true.
- Limites mémoire : `memory_char_limit=2200`, `user_char_limit=1375`.
- `curator.enabled`: true, cadence 168h.
- `approvals.mode`: manual.
- `approvals.cron_mode`: deny.
- `approvals.mcp_reload_confirm`: false.
- `security.tirith_enabled`: true.
- `security.allow_lazy_installs`: true.
- `tools.tool_search.enabled`: auto, seuil 10%.

### Obsidian MCP

- Endpoint : `https://127.0.0.1:27124/mcp/`.
- Plugin Obsidian Local REST API with MCP : v4.1.3.
- Obsidian : v1.12.7.
- REST health : OK.
- Auth secret : `${OBSIDIAN_API_KEY}` dans `~/.hermes/.env`, pas en clair dans `config.yaml`.
- `ssl_verify: false`, acceptable uniquement parce que c’est du localhost loopback.
- `supports_parallel_tool_calls: false`, cohérent pour éviter écritures concurrentes risquées côté vault.
- Outils MCP serveur découverts : 16.
- Outils exposés à Hermes : 11 via allowlist stricte.

Allowlist actuelle :

- `vault_list`
- `vault_read`
- `vault_append`
- `vault_patch`
- `vault_get_document_map`
- `active_file_get_path`
- `periodic_note_get_path`
- `search_query`
- `search_simple`
- `tag_list`
- `open_file`

Outils volontairement non exposés par défaut : `vault_write`, `vault_delete`, `vault_move`, `command_execute`, `command_list`.

### Toolsets et outils

Outils CLI activés ou disponibles :

- web toolset activé dans la config CLI, mais aucun backend API web n’est configuré ; l’outil est donc conceptuellement actif mais limité par les secrets manquants.
- browser actif ; backend local amélioré par installation `agent-browser`.
- terminal, file, code_execution actifs.
- vision, image_gen, tts actifs côté toolsets, mais certains backends avancés dépendent de clés.
- skills, todo, memory, session_search, clarify, delegation, cronjob actifs.
- computer_use activé et maintenant dépendance `cua-driver` installée ; il reste à valider les permissions macOS Accessibility + Screen Recording.
- kanban disponible en mode runtime-gated.

Dépendances ajoutées :

- `rg`: `/opt/homebrew/bin/rg`, version 15.1.0.
- `cua-driver`: `/Users/hugo/.local/bin/cua-driver`, version 0.6.5.
- `agent-browser`: disponible dans le PATH fnm, version 0.29.1.

### Skills installées

Skills Hermes actives :

- `claude-code`
- `codex`
- `devlab-ai-telemetry-sources`
- `workstation-environment-onboarding`
- `obsidian-memory-integration`
- `obsidian-memory-workflow`
- `devlab-ai-maintenance`
- `ecommerce-conversion-audit`
- `meh-project-onboarding`

Point notable : la skill officielle/bundled `hermes-agent` existe dans la documentation comme `autonomous-ai-agents/hermes-agent`, mais elle n’est pas installée localement. Les instructions système demandaient de la charger, mais `skill_view('hermes-agent')` échoue. À corriger en installant la skill depuis le hub/catalogue si disponible.

### Plugins

Tous les plugins bundled sont actuellement `not enabled`. C’est le comportement normal : les plugins Hermes sont opt-in.

Plugins potentiellement utiles pour Hugo :

- `disk-cleanup`: nettoyage automatique des fichiers temporaires de session.
- `security-guidance`: avertissements non bloquants sur patterns de code dangereux lors des écritures.
- provider plugins selon besoins : OpenRouter, Anthropic, Copilot, xAI, FAL, web providers.
- `web-ddgs` ou un backend web sans clé, si stable dans l’environnement.
- `langfuse` uniquement si Hugo veut de l’observabilité LLM.

### Sécurité

- `approvals.mode=manual` : bon choix en local Mac réel.
- Les aliases `hm`/`hy` lancent `hermes --yolo`, ce qui bypass les prompts dangereux pour la session. Productif, mais à réserver aux repos/outils de confiance.
- MCP Obsidian en allowlist : très bon.
- `vault_delete`, `vault_write`, `vault_move`, `command_execute` non exposés : très bon.
- API key Obsidian hors vault : très bon.
- `~/.hermes/.env` en permissions 0600 : bon.
- Doctor : pas d’advisory actif, pas de commande MCP stdio suspecte.

## Bonnes pratiques Hermes à appliquer

### 1. Organisation mémoire

Garder trois couches nettes :

1. Hermes memory : faits courts et durables injectés dans chaque session.
2. Hermes skills : procédures réutilisables qui doivent guider l’agent.
3. Obsidian : contexte long, recherches, décisions, specs, meetings, notes projet, sources.

Ne pas mettre dans Hermes memory : tâches temporaires, PRs, résultats de session, détails périssables, recherches longues.

### 2. Obsidian comme base de connaissance agent-readable

Conserver frontmatter stable : `type`, `status`, `project`, `area`, `tags`, `updated`, `confidence`, `source`, `last_verified`, `review_after`, `supersedes`, `superseded_by` quand pertinent.

Toujours rechercher avant de créer, lire les notes pertinentes, utiliser `vault_patch`/`vault_append`, relire après écriture.

### 3. Skills comme mémoire procédurale

Créer/patcher une skill quand un workflow devient réutilisable : audit e-commerce, maintenance repo, setup MCP, workflow Obsidian, debugging CI, etc.

Ne pas créer une skill pour chaque tâche ; consolider les procédures proches.

### 4. Cron/autonomie

Les cron jobs tournent dans des sessions fraîches sans contexte de chat courant : le prompt doit être autonome.

Utiliser :

- `no_agent=True` pour watchdogs simples dont le script produit exactement le message attendu.
- job agentique avec `skills=[...]` pour revues hebdo, synthèses, audits, briefs.
- `workdir` pour jobs attachés à un repo, en sachant que les jobs avec workdir tournent séquentiellement.
- `context_from` pour chaîner jobs collecte → synthèse.

### 5. Tool Search

`tools.tool_search.enabled=auto` est cohérent. Laisser `auto` tant que peu de MCP/plugin tools sont exposés. Si un jour beaucoup de MCPs sont ajoutés, Tool Search évitera que les schemas d’outils consomment trop de contexte.

### 6. Compression/contexte

Configuration actuelle bonne pour `gpt-5.5` Codex grâce à `codex_gpt55_autoraise=true`. Attention : le modèle de compression auxiliaire doit avoir assez de contexte, sinon les résumés de compression peuvent échouer. Garder `provider:auto` tant que le provider principal est robuste.

### 7. Providers et fallbacks

Gros manque actuel : aucun fallback provider. Pour un environnement vraiment robuste, ajouter au moins un fallback indépendant du provider principal.

Options recommandées :

- OpenRouter avec `OPENROUTER_API_KEY`, bon fallback multi-modèles et donne accès à `moa` selon config.
- Anthropic OAuth/API si Hugo a Claude Max/API.
- Gemini/Google AI Studio comme fallback économique/large contexte.
- Local Ollama/LM Studio comme fallback offline pour tâches simples.

### 8. Web research

Gros manque actuel : aucun backend web API (`EXA`, `PARALLEL`, `TAVILY`, `FIRECRAWL`, etc.). Le navigateur peut dépanner, mais pour recherches robustes Hermes devrait avoir un vrai backend web.

Priorité recommandée :

1. Exa ou Tavily pour recherche/extraction généraliste.
2. Firecrawl si besoin crawl/extraction de sites entiers.
3. Parallel si besoin recherches objectives/rapides.
4. Brave Search API free ou SearXNG si priorité coût/self-host/privacy.

### 9. Plugins utiles

Activer prudemment :

- `security-guidance` pour un filet sécurité sur écritures de code.
- `disk-cleanup` pour hygiène de fichiers temporaires.
- un provider web concret plutôt que toolset web “vide”.

### 10. Computer Use macOS

`cua-driver` est installé, mais les permissions TCC macOS sont inconnues tant que le daemon n’a pas été lancé sous son identité. Étape manuelle restante :

```bash
cua-driver permissions grant
```

Puis vérifier :

```bash
cua-driver permissions status
hermes computer-use status
```

## Priorités recommandées

### P0 — déjà corrigé pendant l’audit

- Installer `ripgrep`.
- Installer `cua-driver`.
- Installer `agent-browser`.
- Installer `websockets==15.0.1` pour résoudre l’import browser dialog.

### P1 — à faire avec choix/secret utilisateur

- Accorder permissions macOS à CuaDriver.
- Ajouter un backend web réel : Exa/Tavily/Firecrawl/Parallel/Brave/SearXNG.
- Ajouter un fallback provider robuste.
- Installer la skill officielle/bundled `hermes-agent` localement si disponible via hub/catalogue.
- Activer `security-guidance` et `disk-cleanup` si Hugo accepte ces plugins.

### P2 — autonomie progressive

- Créer un cron hebdo “Memory Quality Review” qui vérifie inbox, notes sans `updated`, `review_after` dépassé, orphelines et low-confidence dans Obsidian.
- Créer un cron quotidien/hebdo par projet important si le projet a une source vérifiable (repo, notes, analytics, issues), pas un cron générique qui invente de l’activité.
- Éventuellement lancer un gateway permanent si Hugo veut interagir avec Hermes via mobile/messaging.

### P3 — observabilité et multi-agent

- Évaluer Kanban multi-agent seulement si Hugo veut paralléliser des travaux longs.
- Évaluer Langfuse uniquement si l’observabilité des coûts/traces devient utile.
- Évaluer context_engine alternatif seulement si la compression standard devient insuffisante.

## Commandes utiles

Audit :

```bash
hermes --version
hermes status
hermes doctor
hermes config show
hermes tools --summary list
hermes mcp list
hermes mcp test obsidian
hermes prompt-size
```

Providers :

```bash
hermes model
hermes fallback add
hermes fallback list
hermes config set <key> <value>
```

Computer use :

```bash
hermes computer-use status
hermes computer-use install
cua-driver permissions grant
cua-driver permissions status
```

Cron :

```bash
hermes cron list
hermes cron create "0 9 * * 1" "...prompt autonome..." --name "..."
```

Plugins :

```bash
hermes plugins list
hermes plugins enable security-guidance
hermes plugins enable disk-cleanup
```

## Verdict

Le socle Hermes + Obsidian est très bon et déjà meilleur que la plupart des setups : mémoire structurée, MCP restreint, notes protocole, skills locales, recherches sessions, et configuration à jour.

Les gains restants ne sont pas dans Obsidian/MCP mais dans l’écosystème autour : providers de secours, web search réel, permissions macOS pour computer-use, plugins d’hygiène/sécurité, et automatisations cron ciblées.
