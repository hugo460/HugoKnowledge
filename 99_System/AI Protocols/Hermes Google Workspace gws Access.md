---
type: protocol
status: active
project: Hermes Agent
area: Google Workspace integration
tags:
  - hermes
  - google-workspace
  - gmail
  - gws
  - oauth
created: 2026-06-22
updated: 2026-06-22
confidence: high
source: Hermes setup session and live gws verification
owner: hugo
ai_use: true
---

# Hermes Google Workspace gws Access

## Résumé

Hermes dispose d'un accès Google Workspace persistant pour `hugo@devlab.io` via le CLI `gws`.

L'accès est conçu pour éviter de refaire la connexion OAuth à chaque session : `gws` conserve un refresh token chiffré localement et rafraîchit les tokens d'accès quand nécessaire.

## Commandes et chemins

- CLI stable : `/Users/hugo/.local/bin/gws`
- Helper d'auth : `/Users/hugo/.local/bin/gws-auth-login-xlab`
- Credentials OAuth chiffrés : `/Users/hugo/.config/gws/credentials.enc`
- Config client OAuth : `/Users/hugo/.config/gws/client_secret.json`
- Cache token temporaire : `/Users/hugo/.config/gws/token_cache.json`

## Secrets

Les secrets OAuth client ne doivent pas être écrits dans Obsidian, ni dans le chat, ni dans des fichiers projet.

Ils sont stockés dans le trousseau macOS via `xlab secret` :

- `claude-code.GOOGLE_WORKSPACE_CLI_CLIENT_ID`
- `claude-code.GOOGLE_WORKSPACE_CLI_CLIENT_SECRET`

Vérification sans exposition :

```bash
xlab secret check GOOGLE_WORKSPACE_CLI_CLIENT_ID
xlab secret check GOOGLE_WORKSPACE_CLI_CLIENT_SECRET
```

## État validé

Dernière validation : 2026-06-22.

`gws auth status` a confirmé :

- utilisateur : `hugo@devlab.io`
- stockage : chiffré
- refresh token : présent
- token valide : oui
- credentials chiffrés présents : oui
- scopes actifs : 42

Tests réussis :

```bash
gws gmail users getProfile --params '{"userId":"me"}' --format json
gws drive files list --params '{"pageSize":1,"fields":"files(id,name,mimeType,modifiedTime)"}' --format json
gws calendar calendarList list --params '{"maxResults":1}' --format json
gws calendar +agenda
```

Note : après extension des scopes, il peut être nécessaire de supprimer le cache token temporaire si un ancien token Gmail-only continue d'être utilisé :

```bash
rm -f /Users/hugo/.config/gws/token_cache.json
```

## Scopes / services

Le helper par défaut demande les services principaux :

- Gmail
- Calendar
- Drive
- Docs
- Sheets
- People / Contacts
- Tasks
- Forms
- Slides
- Keep
- Apps Script
- Meet

Google Chat est volontairement exclu du défaut, car `gws` l'étend à de nombreux scopes `chat.admin` susceptibles de compliquer la validation OAuth. Ajouter Chat seulement si nécessaire :

```bash
gws-auth-login-xlab -s chat
```

## Règles d'utilisation par Hermes

Hermes peut utiliser `gws` pour lire, rechercher et analyser les données Google Workspace lorsque Hugo le demande.

Actions autorisées sans confirmation supplémentaire si elles répondent à la demande :

- rechercher/lister des mails, fichiers, calendriers ou documents ;
- lire des éléments nécessaires à l'analyse ;
- préparer des brouillons ou recommandations.

Actions nécessitant une confirmation explicite de Hugo :

- envoyer, répondre ou transférer un email ;
- supprimer, archiver, déplacer ou labelliser des mails ;
- créer/modifier/supprimer des événements Calendar ;
- créer/modifier/supprimer/partager des fichiers Drive, Docs, Sheets, Slides ou Forms ;
- toute modification durable de données Google.

## Commandes utiles

```bash
gws auth status
gws gmail +triage
gws calendar +agenda
gws drive files list --params '{"pageSize":10}'
gws --help
```

## Liens

- [[Hermes Obsidian Memory Protocol]]
- [[Hermes Launcher Obsidian Autostart]]
