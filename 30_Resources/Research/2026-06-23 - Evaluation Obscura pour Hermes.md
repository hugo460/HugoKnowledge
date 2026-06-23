---
type: research
status: active
project: Hermes Obsidian Memory
area: AI Agents
tags: [research, hermes, obscura, scraping, headless-browser, mcp, security]
created: 2026-06-23
updated: 2026-06-23
last_verified: 2026-06-23
confidence: high
source: official Hermes docs + GitHub h4ckf0r0day/obscura audit + local macOS tests
owner: hugo
ai_use: true
---

# 2026-06-23 - Évaluation Obscura pour Hermes

## Question

Faut-il intégrer Obscura comme outil/plugin Hermes pour scraping/headless browsing puissant, propre et maintenable ?

## Verdict

Oui pour un prototype local expérimental sandboxé ; non pour une intégration Hermes globale par défaut à ce stade.

- Pertinence : 7/10
- Risque : 6/10
- Maturité : 5/10
- Architecture recommandée : wrapper local + éventuel MCP stdio allowlisté plus tard, pas plugin Hermes custom maintenant.

## Prototype créé

Dossier : `/Users/hugo/Desktop/Outils/hermes-obscura-experiment`

Fichiers principaux :

- `README.md` : rapport complet, commandes, tests, limites, risques.
- `obscura_wrapper.py` : wrapper expérimental avec garde-fous.
- `obscura-policy.json` : domaines/hosts sensibles bloqués.
- `hermes-mcp-obscura.example.yaml` : exemple d’intégration MCP Hermes non appliqué.
- `bin/obscura`, `bin/obscura-worker` : release macOS arm64 v0.1.8.
- `obscura-src/` : clone shallow du repo audité.
- `test-output/` : sorties réelles des tests.

## Sources et version

- Repo : https://github.com/h4ckf0r0day/obscura
- Release testée : v0.1.8
- Commit local : `edde67d`, 2026-06-22
- Licence : Apache-2.0
- Archive macOS arm64 SHA256 local : `dfa84fa20e0e33c7b1af9ded190cdbf928c5a52a3edb308600595e11455ee7bb`

## Tests réels réussis

- `fetch https://example.com --eval document.title` → `Example Domain`
- `fetch https://example.com --dump text` → texte Example Domain
- `fetch https://example.com --dump links` → lien IANA
- `fetch https://en.wikipedia.org/wiki/Web_scraping --dump markdown` → exit 0, sortie markdown mais nettoyage sélecteur à améliorer
- `fetch https://news.ycombinator.com --eval ...` → titres HN extraits
- `fetch https://example.com --stealth --dump text` → exit 0 sur URL neutre
- `scrape` parallèle example.com + HN → JSON avec titres, exit 0
- CDP Puppeteer-core → OK sur example.com
- CDP Playwright-core → OK sur example.com
- Comparaison curl example.com → curl suffisant pour page statique

## Garde-fous recommandés

- Ne pas utiliser pour contourner des protections d’accès, CAPTCHA, anti-bot actifs, login, paiement ou données privées.
- Ne pas exposer le MCP HTTP hors loopback sans auth/reverse proxy.
- Ne pas exposer cookies/storage tools dans Hermes par défaut.
- Ne pas utiliser `--allow-private-network` par défaut.
- Ne pas configurer proxy/stealth globalement par défaut.
- Pin de version + checksum local.

## Prochaine étape

Si Hugo valide : tester Obscura comme MCP stdio local avec allowlist restrictive dans une session Hermes dédiée, sans modifier définitivement `~/.hermes/config.yaml` avant confirmation.


## Intégration Hermes activée le 2026-06-22

Suite à validation utilisateur, Obscura a été ajouté proprement à Hermes comme serveur MCP stdio local expérimental, sans HTTP exposé.

Configuration appliquée dans `/Users/hugo/.hermes/config.yaml` :

- `toolsets` contient maintenant `mcp-obscura` en plus de `hermes-cli` et `mcp-obsidian`.
- `mcp_servers.obscura.command`: `/Users/hugo/Desktop/Outils/hermes-obscura-experiment/bin/obscura`
- `mcp_servers.obscura.args`: `["mcp"]`
- Allowlist exposée :
  - `browser_navigate`
  - `browser_snapshot`
  - `browser_markdown`
  - `browser_links`
  - `browser_extract`
  - `browser_search`
  - `browser_evaluate`
  - `browser_network_requests`
  - `browser_console_messages`
  - `browser_close`

Outils volontairement non exposés par défaut : cookies, storage, set_cookie, clear_cookies, fill/click/type/select/scroll/forms/tabs. Objectif : garder Obscura puissant pour extraction/rendu/JS/CDP, sans ouvrir par défaut les surfaces sensibles d’automatisation interactive ou de session hijacking.

Vérifications réelles :

- `hermes mcp list` montre `obscura` activé avec 10 outils sélectionnés.
- `hermes mcp test obscura` se connecte en stdio, découvre 35 outils côté serveur.
- `hermes tools --summary list` montre `obscura` avec l’allowlist prévue.
- Test end-to-end fresh Hermes : prompt demandant d’utiliser MCP Obscura sur `https://example.com` → `OBSCURA_OK: Example Domain`.

Commande terminal pratique ajoutée : `/Users/hugo/.local/bin/obscura-hermes`, wrapper vers `obscura_wrapper.py`.

Exemples :

```bash
obscura-hermes fetch https://example.com --format markdown --timeout 10
obscura-hermes eval https://example.com --js 'document.title' --timeout 10
```

Pour utiliser dans une session Hermes : demander explicitement “utilise Obscura MCP” ou “utilise le serveur MCP Obscura pour rendre la page/exécuter JS/extraire markdown/liens”. Après modification de config dans une session déjà ouverte, lancer `/reload-mcp` ou redémarrer Hermes.


## Mise à jour 2026-06-22 — Obscura MCP en mode complet

À la demande explicite de Hugo, l’allowlist Obscura a été élargie de 10 à 35 outils MCP découverts par le serveur.

Backup avant modification : `/Users/hugo/.hermes/config.yaml.backup-before-obscura-full-tools-20260622-204520`

Outils exposés maintenant :

- `browser_navigate`
- `browser_snapshot`
- `browser_click`
- `browser_fill`
- `browser_type`
- `browser_press_key`
- `browser_select_option`
- `browser_evaluate`
- `browser_wait_for`
- `browser_network_requests`
- `browser_console_messages`
- `browser_close`
- `browser_markdown`
- `browser_links`
- `browser_interactive_elements`
- `browser_back`
- `browser_forward`
- `browser_reload`
- `browser_get_cookies`
- `browser_set_cookie`
- `browser_clear_cookies`
- `browser_wait_for_text`
- `browser_detect_forms`
- `browser_fill_form`
- `browser_scroll`
- `browser_get_attribute`
- `browser_count`
- `browser_extract`
- `browser_tab_new`
- `browser_tab_list`
- `browser_tab_switch`
- `browser_tab_close`
- `browser_search`
- `browser_storage_state`
- `browser_set_storage_state`

Garde-fou conservé : liste explicite plutôt qu’exposition non filtrée, afin qu’un futur ajout upstream ne soit pas exposé automatiquement sans revue.

Vérifications :

- `hermes mcp list` → `obscura` activé avec 35 outils sélectionnés.
- `hermes mcp test obscura` → connexion stdio OK, 35 outils découverts.
- `hermes tools --summary list` → allowlist complète visible.
- Fresh Hermes smoke test → `OBSCURA_FULL_OK`.

Règle d’usage : puissance complète disponible, mais ne pas utiliser cookies/storage/interactions pour contourner accès, login, paiement, CAPTCHA, anti-bot actif ou restrictions privées sans instruction explicite et légitime.
