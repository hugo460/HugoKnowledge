---
type: project
status: active
project: MEH - Hilton Menu Engineering
area: Client Devlab / Hilton Hotel Tahiti
tags: [project, risk, roadmap, hilton, meh]
created: 2026-06-22
updated: 2026-06-22
confidence: high
source: "Hermes analysis of round 108 repo context"
owner: hugo
ai_use: true
---

# Chantiers et risques actifs - MEH

## Priorité immédiate

### PR #102 - ingestion Achat

- Branche : `feat/imports-achat-direct-issue`
- Commit : `668f3bb`
- But : ingérer les mouvements NAV `Achat` comme `PURCHASE` dans `RequisitionLine.movementType`.
- Migration : `20260623090000_requisition_movement_type`.
- Effet voulu : préparer PR2 `[C2]` sans modifier encore les ratios Cost Control.
- Après merge : approuver Railway au dashboard.

## Bloqueur métier PR2 `[C2]`

### Brenda Q2 - traitement `11171`

Question : les transferts reçus de la cuisine centrale/pâtisserie `11171` sont-ils inclus dans le coût par restaurant, ou retirés et regroupés dans la ligne `Cuisine Hors 111` ?

Impact : cette réponse décide si le food par outlet doit retirer les transferts 11171→outlet ou les garder.

## Chantiers après PR #102

1. PR2 `[C2]` Cost Control : achats directs outlet + classification deptCode + cafétéria + 11171 + warnings.
2. Contrôle croisé PBD vs DMR puis retrait du PBD comme source principale.
3. Décision `C&E(83)` / Banquet côté revenue/menu engineering.
4. Renforcement documentation Obsidian + repo pour maintenance long terme.

## Risques démo mercredi

- Montrer un ratio Cost Control non finalisé comme vérité définitive.
- Oublier Railway `NEEDS_APPROVAL` après un merge.
- Confondre coût recette et coût matière devant le client.
- Ouvrir un placeholder ou écran non prêt.
- Se laisser entraîner dans une promesse de date contractuelle.

## Invariants à rappeler aux agents

- Search/read repo before editing.
- No hardcoded financial mapping in parsers.
- No PR without tests/gates/Notion discipline.
- Keep documentation synced : changelog, handoff, Obsidian for long-form memory.
