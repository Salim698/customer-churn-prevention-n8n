# Installation, configuration et tests

## Prérequis

- Une instance n8n
- Une clé API Google Gemini (Google AI Studio)
- Un compte Google (Sheets et Gmail)

## 1. Google Sheet

Crée un classeur avec un onglet nommé **`customers`**. La ligne 1 doit contenir exactement ces en-têtes (modèle dans `data/customers_template.csv`) :

| Colonne | Rôle |
|---|---|
| `id` | Identifiant unique, sert de clé de mise à jour |
| `name`, `email`, `plan`, `mrr` | Informations client |
| `last_payment_status` | `paid`, `pending` ou `failed` |
| `days_until_renewal` | Jours avant renouvellement |
| `logins_last_14_days` | Connexions sur 14 jours |
| `key_features_used` | Liste de features (texte séparé par des virgules) |
| `open_tickets` | Tickets support ouverts |
| `last_ticket_sentiment` | `positive`, `neutral`, `negative`, `very_negative` |
| `signup_date`, `active_users` | Ancienneté et utilisateurs actifs |
| `risk_emoji`, `risk_level`, `score_total` | Remplis par le workflow |
| `ai_diagnostic`, `ai_raison`, `ai_action`, `ai_message_client` | Remplis par Gemini |

L'ID du classeur se trouve dans l'URL : `https://docs.google.com/spreadsheets/d/<ID>/edit`.

## 2. Variable d'environnement

| Variable | Description |
|---|---|
| `GEMINI_API_KEY` | Clé API Gemini |

Sur n8n self-hosted, injecte-la dans l'environnement du conteneur.

> Les versions récentes de n8n peuvent bloquer `$env` dans les nœuds. En cas d'erreur "access to env vars denied", ajoute `N8N_BLOCK_ENV_ACCESS_IN_NODE=false`. Sur n8n Cloud, `$env` n'est pas disponible : utilise une credential "Header Auth" sur le nœud `HTTP Request1`.

## 3. Import et configuration

1. n8n > **Workflows > Import from file** > `workflow.json`
2. Dans `Append or update row in sheet1` et `Update row in sheet1` : remplace `YOUR_GOOGLE_SHEET_ID_HERE` et sélectionne ton credential Google Sheets
3. Si les colonnes n'apparaissent pas dans le mapping, ouvre le nœud et clique sur **Refresh columns**
4. Dans les 3 nœuds Gmail : sélectionne ton credential Gmail et remplace `YOUR_TEST_EMAIL@example.com`

## 4. Test

1. Clique sur **Execute workflow** (le trigger quotidien n'est pas nécessaire pour tester)
2. Vérifie dans le Sheet : 5 lignes, avec scores, niveaux de risque et textes IA
3. Vérifie ta boîte mail : 3 types d'emails attendus (MOYEN, CRITIQUE, TRES_CRITIQUE). Les clients FAIBLE ne déclenchent rien.

Résultats attendus avec les données de démo :

| Client | Score | Niveau |
|---|---|---|
| TechFlow SaaS | 2 | ⚫ TRES_CRITIQUE |
| StartupX | 2 | ⚫ TRES_CRITIQUE |
| CloudNova | 100 | 🟢 FAIBLE |
| DataBridge | 59 | 🟡 MOYEN |
| PixelForge | 30 | 🔴 CRITIQUE |

## 5. Passer en production

- Remplace le nœud `Mock Customers1` par ta vraie source de données
- Remplace l'adresse de test des nœuds Gmail par `{{ $json.email }}` **uniquement** après avoir validé la qualité des emails générés
- Active le workflow (interrupteur en haut à droite)

## Dépannage

| Symptôme | Cause probable |
|---|---|
| Erreur 400/403 sur Gemini | Clé invalide ou variable non définie |
| Colonnes vides dans le Sheet | En-têtes différents de ceux attendus |
| Champs IA vides | Réponse Gemini coupée : vérifier la sortie de `HTTP Request1` |
| Aucun email | Client classé FAIBLE, ou credential Gmail non connecté |
