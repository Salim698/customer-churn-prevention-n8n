# Description des nœuds

| Nœud | Type | Rôle |
|---|---|---|
| Schedule Trigger1 | Schedule | Déclenche le workflow chaque jour à 8h |
| Mock Customers1 | Code | 5 clients fictifs couvrant les 4 niveaux de risque (à remplacer par une vraie source) |
| Health Score1 | Code | Calcule les 5 sous-scores, le total, le niveau de risque et l'emoji |
| Append or update row in sheet1 | Google Sheets | Crée ou met à jour la ligne du client (clé : `id`) avec toutes les données et le score |
| Build Prompt1 | Code | Construit le prompt Customer Success et le body JSON pour Gemini |
| HTTP Request1 | HTTP Request | Appelle l'API Gemini (clé via `$env.GEMINI_API_KEY`), 3 tentatives en cas d'échec |
| Parse Response1 | Code | Extrait le JSON de Gemini ; en cas d'échec de parsing, écrit "Vérifier manuellement" |
| Update row in sheet1 | Google Sheets | Écrit diagnostic, raison, action et email dans la ligne du client |
| Switch1 | Switch | Oriente selon `risk_level` (FAIBLE n'a pas de sortie connectée) |
| Gmail MOYEN1 | Gmail | Email "quelques ressources pour vous aider" |
| Gmail CRITIQUE1 | Gmail | Email "action requise" (5 tentatives max) |
| Gmail TRES_CRITIQUE1 | Gmail | Email "urgent" |

## Format attendu de Gemini

```json
{
  "diagnostic": "2-3 phrases",
  "raison_principale": "cause principale",
  "action_recommandee": "action concrète",
  "message_client": "email court prêt à envoyer"
}
```
