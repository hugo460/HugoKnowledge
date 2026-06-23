---
type: meeting
status: draft
project: MEH - Hilton Menu Engineering
area: Client Devlab / Hilton Hotel Tahiti
tags: [meeting, demo, hilton, meh, client, direction]
created: 2026-06-22
updated: 2026-06-22
confidence: high
source: "User context: Hugo sees Hilton direction on Wednesday to show the tool"
owner: hugo
ai_use: true
---

# Préparation démo direction Hilton - 2026-06-24

## Contexte

Hugo voit la direction du Hilton mercredi pour montrer l'outil. Le projet est stratégique et doit être présenté comme un outil déjà sérieux, connecté aux vrais formats Hilton, avec des limites assumées sur les arbitrages encore en validation.

## Objectif de la démo

Montrer une plateforme crédible et exploitable, pas une liste exhaustive de features. Priorité : stabilité, lisibilité business, traçabilité des chiffres.

## Message recommandé

L'outil est déjà branché sur les formats réels Hilton. Les modules clés existent : imports, articles, fiches, validation, DGAE, liaison, menu engineering et Cost Control. Les chiffres financiers sont sécurisés par étapes : revenu DMR officiel, ventes CBD pour split food/bev, réquisitions/OC, puis finalisation des achats directs et du traitement cuisine centrale avec Brenda.

## Parcours de démo recommandé

1. Connexion / tenant Hilton.
2. Dashboard général.
3. Navigation et rôles.
4. Import données : montrer que l'outil accepte les vrais exports.
5. Articles NAV : catalogue et prix.
6. Fiche technique : builder, coût live, workflow de validation.
7. DGAE : conformité alcool et alertes.
8. Liaison ventes : rapprocher fiches et plats vendus.
9. Menu Engineering : matrice K&S.
10. Cost Control : expliquer revenu DMR + coût matière, en restant prudent sur les arbitrages Brenda en finalisation.

## Écrans à éviter ou cadrer explicitement

- SpotCheck si placeholder.
- Historique prix Food/Beverage si placeholder.
- Export PDF/menu si non livré.
- Toute lecture trop définitive des ratios Cost Control si la règle `11171` n'est pas encore validée.

## Préparation technique avant démo

- Vérifier PR #102 : merger uniquement si CI verte + GO.
- Après merge : approuver Railway au dashboard, migration `20260623090000`, smoke `/health` et `/health/db`.
- Vérifier web prod Vercel.
- Vérifier que les routes importantes ne cassent pas.
- Préparer un navigateur déjà connecté si possible.
- Préparer un plan B : screenshots ou explication structurée si un service externe est lent.

## Questions sensibles possibles

### Les chiffres Cost Control sont-ils définitifs ?

Réponse : le revenu officiel DMR est en place, le split food/bev est basé sur les ventes réelles CBD, les réquisitions/OC sont intégrées. La dernière règle de ventilation cuisine centrale/pâtisserie est en validation avec Brenda pour éviter d'afficher un ratio financier approximatif.

### Est-ce prêt pour la production ?

Réponse : plusieurs modules sont déjà déployés et testés. La livraison est itérative : les surfaces cœur sont prêtes pour démonstration et usage encadré, les règles financières sensibles sont finalisées avec validation métier.

### Pourquoi ne pas tout automatiser tout de suite ?

Réponse : les exports réels Hilton ont des cas métiers spécifiques. L'outil privilégie la justesse et la traçabilité plutôt qu'une automatisation opaque.

## À préparer ensuite

- Note de compte-rendu après réunion.
- Liste des retours direction.
- Décisions métier validées oralement.
- Chantiers post-démo classés P0/P1/P2.
