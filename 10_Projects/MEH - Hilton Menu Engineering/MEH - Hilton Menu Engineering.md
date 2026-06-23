---
type: project
status: active
project: MEH - Hilton Menu Engineering
area: Client Devlab / Hilton Hotel Tahiti
tags: [project, client, hilton, meh, menu-engineering, devlab, maintenance]
created: 2026-06-22
updated: 2026-06-22
confidence: high
source: "Hermes analysis of repo /Users/hugo/Desktop/DEVLAB/Projets/hilton-menu-engineering-prod on 2026-06-22"
owner: hugo
ai_use: true
---

# MEH - Hilton Menu Engineering

> Projet client stratégique Devlab pour le Hilton Hotel Tahiti. Hugo est le développeur principal/solo sur le projet. Même après la livraison et la démo direction, ce projet doit rester documenté comme un actif long terme : maintenance, améliorations, suivi client, corrections financières et évolutions produit.

## Résumé exécutif

MEH est une plateforme SaaS F&B sur mesure pour le Hilton Hotel Tahiti. Elle remplace des fichiers Excel dispersés par un outil centralisé pour :

- importer les données réelles Hilton : NAV, HGD/Agilysys, DMR, réquisitions, OC ;
- gérer les articles et prix NAV ;
- créer et valider des fiches techniques ;
- calculer le coût recette théorique ;
- calculer le Cost Control outlet réel ;
- surveiller la conformité DGAE ;
- relier fiches techniques et plats vendus ;
- produire une matrice Menu Engineering Kasavana & Smith.

## Repo et environnement

- Repo local : `/Users/hugo/Desktop/DEVLAB/Projets/hilton-menu-engineering-prod`
- Repo GitHub : `devlab-io/menu-engineering-prod`
- Stack : Turborepo + pnpm, Next.js 16, React 19, NestJS 11, Prisma 6, PostgreSQL, Clerk, Doppler, Railway, Vercel.
- Déploiement API : Railway.
- Déploiement web : Vercel.
- Suivi projet : Notion `MEH — Tasks` + changelog repo.
- Source de vérité workflow repo : `AGENTS.md`, `CLAUDE.md`, `.agents/skills/meh-workflow/SKILL.md`, `docs/changelog/`, `docs/superpowers/plans/phase-0-progress-handoff-v3.md`.

## Notes liées

- [[10_Projects/MEH - Hilton Menu Engineering/Contexte technique et produit]]
- [[10_Projects/MEH - Hilton Menu Engineering/Préparation démo direction Hilton - 2026-06-24]]
- [[10_Projects/MEH - Hilton Menu Engineering/Chantiers et risques actifs]]

## Concepts métier à ne jamais confondre

### Coût recette

Coût théorique d'une fiche technique ou d'une portion, calculé à partir des ingrédients, prix NAV, unités, pertes, cuisson et prix de vente.

### Coût matière outlet / Cost Control

Coût réel d'un outlet sur une période : réquisitions + achats directs - OC, rapporté au revenu officiel. C'est la logique Brenda/Hilton de pilotage financier.

Ces deux calculs coexistent mais ne répondent pas à la même question.

## État général au 2026-06-22 / round 108

- PR #101 mergée : revenu DMR officiel utilisé pour le Cost Control, split food/bev via CBD Revenue Category.
- PR #102 ouverte : ingestion des mouvements `Achat` comme `PURCHASE` dans `RequisitionLine.movementType`.
- Le Cost Control reste volontairement TRANSFER-only jusqu'à PR2 `[C2]` pour ne pas changer les ratios sans arbitrage Brenda.
- Prochaine étape importante : merger #102, approuver Railway, puis attendre/obtenir Brenda Q2 sur le traitement de `11171` avant PR2.

## Pourquoi ce projet mérite une documentation ultra précise

- Client direction Hilton : impact business et financier direct.
- Données réelles sensibles : revenus, coûts, réquisitions, OC, conformité DGAE.
- Hugo est le seul dev principal : la doc doit compenser le bus factor.
- Maintenance post-livraison probable : corrections de formats Excel, règles métier, nouveaux exports, évolution des dashboards.
- Les chiffres financiers doivent être explicables devant Brenda/Sébastien/la direction.

## Principes de documentation pour la suite

- Documenter chaque décision métier durable dans Obsidian ou `docs/superpowers/specs/` selon sa nature.
- Garder les sources chiffrées et arbitrages dans des notes dédiées, jamais uniquement dans le chat.
- Relier les notes Obsidian au repo : chemins fichiers, PR, changelog, specs.
- Maintenir une note `Chantiers et risques actifs` avant chaque session critique.
- Créer une note de préparation pour chaque rendez-vous client important.
