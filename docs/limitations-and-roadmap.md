# Limites connues et roadmap

## Limites actuelles (v1)

- **Données fictives** : les clients viennent d'un nœud Code, pas d'une vraie source.
- **Emails envoyés sans validation humaine** : le texte est généré par une IA. Risque d'erreur de ton ou de contenu avant envoi à de vrais clients.
- **Pas d'anti-doublon** : relancé chaque jour, le workflow renvoie un email au même client tant que son risque ne change pas.
- **Adresse de test** : les nœuds Gmail envoient à une adresse fixe, pas à `email` du client.
- **Seuils en dur** dans le scoring.
- **Google Sheets comme base de données** : adapté au prototype, limité en volume.
- **Pas de gestion d'erreur globale** : pas d'alerte si Gemini ou Gmail échoue durablement.
- **Clients FAIBLE** : aucune action ni log dédié.
- **Conformité** : envoyer des emails automatiques à des clients suppose de respecter le consentement et la réglementation applicable (RGPD, etc.).

## Pistes d'amélioration

- Brancher une vraie source (Stripe, CRM, base produit)
- Étape de validation humaine (brouillon Gmail ou message Slack avec bouton d'approbation)
- Colonne `last_contacted` pour limiter la fréquence des relances
- Alerte interne (Slack, WhatsApp) pour les comptes TRES_CRITIQUE à fort MRR
- Pondération du risque par MRR
- Tableau de bord (Looker Studio) sur l'évolution des scores
- Workflow d'erreur global avec notification
