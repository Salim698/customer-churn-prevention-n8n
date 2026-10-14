# Grille de scoring

Le health score est une note sur **100 points**, somme de 5 dimensions.

| Dimension | Points max | Règle |
|---|---|---|
| Connexions (14 jours) | 30 | ≥ 15 : 30 / ≥ 7 : 20 / ≥ 3 : 10 / sinon 0 |
| Features utilisées | 25 | ≥ 4 : 25 / ≥ 2 : 15 / ≥ 1 : 8 / sinon 0 |
| Support | 20 | 0 ticket : 20 / 1-2 tickets : 10 / ≥ 3 tickets : 0. Sentiment `very_negative` : -5 (minimum 0) |
| Paiement | 15 | `paid` : 15 / `pending` : 8 / sinon 0 |
| Renouvellement | 10 | > 14 jours : 10 / > 5 jours : 6 / sinon 2 |

## Niveaux de risque

| Score | Niveau | Action automatique |
|---|---|---|
| ≥ 70 | 🟢 FAIBLE | Aucune |
| 40 à 69 | 🟡 MOYEN | Email de ressources |
| 20 à 39 | 🔴 CRITIQUE | Email "action requise" |
| < 20 | ⚫ TRES_CRITIQUE | Email urgent |

## Modifier la grille

Toute la logique est dans le nœud `Health Score1` (JavaScript). Les seuils sont des valeurs en dur : adapte-les à ton produit (par exemple, le nombre de connexions attendu dépend de ton usage).
