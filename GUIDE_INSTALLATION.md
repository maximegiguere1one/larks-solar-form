# Formulaire Larks Solar — Guide d'installation

## Vue d'ensemble

Ce formulaire de collecte de leads multi-étapes a été conçu spécifiquement pour Larks Solar. Il reprend fidèlement le design system du site (fond noir premium, or solaire #F5A623, polices Syne/Inter, glassmorphism) et offre une expérience utilisateur dynamique en 3 étapes.

---

## Fonctionnalités

| Fonctionnalité | Description |
|---|---|
| Multi-étapes (wizard) | 3 étapes fluides : Coordonnées, Objectif, Propriété |
| Animations | Transitions animées entre chaque étape, shimmer sur le bouton, glow ambiant |
| Validation en temps réel | Validation par étape avec messages d'erreur et animation shake |
| Formatage automatique | Le numéro de téléphone se formate automatiquement en (514) 555-1234 |
| Slider interactif | Sélection de la facture mensuelle avec un slider visuel dynamique |
| Option cards | Sélection visuelle de l'objectif (Mesurage net, Protection pannes, Hybride) |
| Écran de succès | Récapitulatif complet après soumission avec animation de confirmation |
| Trust indicators | Certifié RBQ, Données sécurisées, Réponse en 24h |
| 100% responsive | S'adapte parfaitement du mobile au desktop |
| Standalone | Un seul fichier HTML, aucune dépendance externe (sauf Google Fonts) |

---

## Installation sur GoDaddy Website Builder

### Étape 1 : Ouvrir l'éditeur GoDaddy

Connectez-vous à votre compte GoDaddy, puis accédez à l'éditeur de votre site web.

### Étape 2 : Ajouter une section HTML

Dans l'éditeur, cliquez sur **Ajouter une section** puis sélectionnez **HTML** (ou **Code personnalisé**). GoDaddy permet d'intégrer du code HTML/CSS/JS personnalisé dans une section dédiée.

### Étape 3 : Coller le code

Ouvrez le fichier `index.html`, copiez **tout le contenu** (de `<!DOCTYPE html>` jusqu'à `</html>`), et collez-le dans la section HTML de GoDaddy.

**Note importante :** Si vous intégrez le formulaire dans une page existante (pas en page complète), vous devrez extraire uniquement le contenu entre `<div class="lf-root">` et la fin du `</script>`, en conservant le `<style>` et le `<script>`.

### Étape 4 : Publier

Cliquez sur **Publier** pour mettre le formulaire en ligne.

---

## Connexion à Monday.com

### Option A : API Monday.com directe (recommandée)

Cette méthode envoie les leads directement dans votre board Monday.com sans outil tiers.

**Prérequis :**
1. Un compte Monday.com avec accès API
2. Un board dédié aux leads avec les colonnes appropriées

**Configuration :**

1. **Obtenir votre API Key Monday.com :**
   - Allez dans Monday.com, cliquez sur votre avatar (en bas à gauche), puis sur **Developers**, puis **My Access Tokens**
   - Copiez votre token API personnel

2. **Obtenir le Board ID :**
   - Ouvrez votre board de leads dans Monday.com
   - Le Board ID se trouve dans l'URL : `monday.com/boards/XXXXXXX`

3. **Obtenir le Group ID (optionnel) :**
   - Si vous voulez que les leads arrivent dans un groupe spécifique du board
   - Utilisez l'API Playground de Monday.com pour lister les groupes

4. **Modifier le code du formulaire :**

Dans le fichier `index.html`, localisez la section CONFIG en haut du `<script>` et remplacez les valeurs :

```javascript
const MONDAY_API_KEY = 'votre_vrai_api_key_ici';
const MONDAY_BOARD_ID = 'votre_board_id_ici';
const MONDAY_GROUP_ID = 'votre_group_id_ici'; // optionnel
```

5. **Décommenter le bloc d'envoi Monday.com :**

Dans la fonction `submitForm()`, décommentez le bloc `try/catch` qui contient l'appel à l'API Monday.com, et adaptez les IDs de colonnes à votre board.

6. **Adapter les IDs de colonnes :**

Chaque colonne dans Monday.com a un ID unique. Pour les trouver, utilisez l'API Playground de Monday.com avec cette requête :

```graphql
{
  boards(ids: VOTRE_BOARD_ID) {
    columns {
      id
      title
      type
    }
  }
}
```

Puis mappez les champs du formulaire aux colonnes de votre board dans l'objet `columnValues`.

7. **Supprimer la simulation :**

Retirez la ligne `await new Promise(r => setTimeout(r, 1500));` qui simule un délai d'envoi.

### Option B : Via Make.com (Integromat) ou Zapier

Si vous préférez utiliser un outil no-code comme intermédiaire :

1. **Créez un webhook** dans Make.com ou Zapier
2. **Copiez l'URL du webhook**
3. **Décommentez le bloc webhook** dans la fonction `submitForm()` et remplacez l'URL
4. **Configurez le scénario** dans Make.com/Zapier pour créer un item dans Monday.com à chaque réception

---

## Colonnes Monday.com recommandées

| Colonne Monday.com | Type | Champ du formulaire |
|---|---|---|
| Nom de l'item | Nom | Prénom + Nom |
| Email | Email | Courriel |
| Téléphone | Téléphone | Téléphone cellulaire |
| Adresse | Texte | Adresse complète |
| Objectif | Statut | Mesurage net / Protection pannes / Hybride |
| Facture mensuelle | Nombre | Montant en $ |
| Matériau toiture | Texte | Matériau sélectionné |
| Année réfection | Texte | Période sélectionnée |
| Type propriété | Texte | Type sélectionné |
| Message | Texte long | Commentaires |
| Date | Date | Date de soumission (automatique) |

---

## Personnalisation

### Modifier les couleurs

Toutes les couleurs sont définies dans les variables CSS `:root` au début du `<style>`. La couleur principale est `--gold: #F5A623`.

### Modifier les champs

Pour ajouter ou retirer des champs, modifiez le HTML dans la section `lf-step` correspondante, puis ajustez la validation dans la fonction `validateStep()` et la collecte de données dans `submitForm()`.

### Modifier les options d'objectif

Les 3 options (Mesurage net, Protection pannes, Hybride) sont dans la section `lf-options`. Vous pouvez en ajouter, en retirer ou modifier les textes.

---

## Support

Pour toute question technique, contactez l'équipe de développement.
