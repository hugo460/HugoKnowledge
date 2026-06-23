---
type: project
status: active
project: MEH - Hilton Menu Engineering
area: Client Devlab / Hilton Hotel Tahiti
tags: [project, client, hilton, meh, architecture, product, context]
created: 2026-06-22
updated: 2026-06-22
confidence: high
source: "Hermes analysis of repo and project docs on 2026-06-22"
owner: hugo
ai_use: true
---

# Contexte technique et produit - MEH

## Mission produit

Construire une plateforme F&B pour le Hilton Hotel Tahiti afin de centraliser les fiches techniques, le costing, la conformité DGAE, la liaison fiche/vente et le Menu Engineering.

## Architecture

- `apps/api` : NestJS 11, DDD-light par modules métier.
- `apps/web` : Next.js 16 avec Feature-Sliced Design.
- `packages/contracts` : contrats Zod partagés front/back.
- `packages/domain` : logique métier pure.
- `apps/api/prisma` : schéma et migrations PostgreSQL.

Flux backend standard : controller → service → domain/contracts/infrastructure → Prisma.

Flux web standard : `shared → entities → features → widgets → views → app`.

## Modules clés

### Imports

Endpoints : NAV, HGD-PBD, HGD-CBD, REQUISITIONS, OC, DMR, reprocess.

Rôle : transformer les fichiers Excel réels du Hilton en données normalisées et auditables.

### Articles NAV

Catalogue articles, prix courant, historique de prix, unités, catégories, alcool/DGAE.

### Fiches techniques

Builder, ingrédients, coûts live, outlets M:N, workflow de validation, rejet, approbation, RBAC cuisine/finance.

### DGAE

Calcul de plafond de prix alcool + journal append-only d'alertes.

### Liaison ventes

Relie les fiches techniques aux plats vendus POS/HGD via `SoldItem` et `FicheSoldItem`.

### Menu Engineering

Matrice Kasavana & Smith : STAR / PUZZLE / WORKHORSE / DOG.

### Cost Control

Coût matière outlet réel. Aujourd'hui : revenu officiel DMR + split food/bev par CBD. En cours : intégration complète achats directs + traitement `11171` + cafétéria.

## Points de vigilance technique

- Next.js 16 : lire les docs locales avant changement Next.
- Tailwind v4 via CSS layers.
- Prisma migrations : timestamps UTC manuels.
- Railway API : gate `NEEDS_APPROVAL` à approuver manuellement au dashboard après merge.
- Vercel Preview rouge systémique : ne pas confondre avec la prod.
- Ne jamais stocker de secret dans Obsidian ou repo.

## Points de vigilance métier

- Ne jamais mélanger coût recette et coût matière outlet.
- Ne jamais afficher de ratio Cost Control si la couverture revenue est incomplète.
- Ne jamais compter tous les `Achat` : seulement les achats directs imputés au deptCode outlet ; économat central exclu.
- Ne jamais hardcoder un mapping deptCode dans le parser.
- `11171` cuisine centrale/pâtisserie est à traiter à part selon réponse Brenda.
- `11250` cafétéria est une ligne budget/réel séparée.
