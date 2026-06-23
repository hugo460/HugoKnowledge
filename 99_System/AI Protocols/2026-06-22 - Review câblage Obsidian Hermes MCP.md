---
type: protocol
status: active
project: Hermes Obsidian Memory
area: AI Agents
tags: [hermes, obsidian, mcp, review, system]
created: 2026-06-22
updated: 2026-06-22
confidence: high
owner: hugo
ai_use: true
---

# 2026-06-22 - Review câblage Obsidian Hermes MCP

## Résultat

Le système Obsidian ⇄ Hermes est correctement câblé et vérifié en exécution réelle.

## Architecture validée

- Obsidian sert de mémoire longue structurée.
- Hermes memory conserve uniquement les faits courts et durables.
- Hermes skills conservent les procédures réutilisables.
- Le vault Obsidian est exposé via le plugin `Local REST API with MCP` sur `https://127.0.0.1:27124/mcp/`.
- Hermes charge le toolset runtime `mcp-obsidian` en plus de `hermes-cli`.

## Vérifications effectuées

- `hermes --version` : Hermes Agent v0.17.0, à jour.
- `hermes mcp list` : serveur `obsidian` enabled.
- `hermes mcp test obsidian` : connexion OK, 16 outils découverts.
- REST health : plugin `Local REST API with MCP` v4.1.3, status OK.
- Port local : Obsidian écoute sur `127.0.0.1:27124`.
- `hermes tools --summary list` : serveur MCP Obsidian visible, avec exclusions actives.
- Test agent réel : une session Hermes fraîche a utilisé l'outil MCP Obsidian pour lire `README.md` et confirmer le titre `Hugo Knowledge Vault`.
- Test REST authentifié : lecture de `README.md` réussie avec la clé stockée hors vault.

## Sécurité validée

- La clé API Obsidian est dans l'environnement Hermes, pas dans le vault.
- Le fichier env a les permissions `0600`.
- La config Hermes utilise `${OBSIDIAN_API_KEY}` et ne contient pas la clé en clair.
- Les outils MCP dangereux sont exclus : `vault_delete`, `command_execute`.
- Le fichier de config du plugin REST contenant la clé est ignoré par Git.
- L'état généré de Smart Connections `.smart-env/` est ignoré et n'est plus tracké.

## Maintenance

- Le vault est versionné en Git local.
- Obsidian Git peut faire les sauvegardes automatiques locales.
- Pas de remote Git configuré pour l'instant ; à ajouter seulement si Hugo veut une synchronisation distante.
- Les templates et index système sont présents dans `99_System/`.
- Le skill Hermes `obsidian-memory-workflow` documente la procédure d'utilisation.

## Points à surveiller

- Si `/reload-mcp` affiche `Agent updated — 0 tool(s) available`, vérifier que `mcp-obsidian` est bien dans `toolsets` puis redémarrer Hermes.
- Si MCP ne répond pas, vérifier d'abord qu'Obsidian est ouvert sur le bon vault et que le plugin REST est actif.
- Si un endpoint non-local est utilisé un jour, remplacer `ssl_verify: false` par un certificat/CA valide.
- Smart Connections génère des fichiers `.smart-env/` volumineux : ils doivent rester ignorés par Git.

## État final

Optimal pour l'usage actuel : local-first, sécurisé, maintenable, versionné, et exploitable directement par Hermes via MCP.
