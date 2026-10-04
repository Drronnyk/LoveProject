# 💌 Lettre interactive

Une page web d'une seule page : une enveloppe à ouvrir, une lettre qui apparaît au fil du défilement, puis une question finale. Au clic sur le bouton de réponse, **WhatsApp s'ouvre avec un message déjà écrit** : il ne reste qu'à appuyer sur envoyer.

Aucune dépendance, aucun build, aucun framework. Tout le projet tient dans un seul fichier : `index.html`.

---

## Aperçu du fonctionnement

1. **Hero** : le prénom, une enveloppe cachetée et un indice « fais défiler pour lire ».
2. **Lettre** : quatre paragraphes qui apparaissent en fondu quand on arrive dessus.
3. **Signature** : un sceau de cire et le nom de l'expéditeur.
4. **Question** : un bouton de réponse qui ouvre WhatsApp avec le message prérempli.

---

## Lancer le projet en local

Ouvre simplement `index.html` dans ton navigateur (double-clic). Il n'y a rien à installer.

Pour tester l'ouverture de WhatsApp, utilise de préférence ton téléphone (ou WhatsApp Web sur ordinateur).

---

## Comment modifier le contenu

Tout le texte se personnalise dans **un seul bloc** : l'objet `CONFIG`, tout en bas de `index.html`, dans la balise `<script>`. Tu n'as normalement rien d'autre à toucher.

| Clé | Rôle |
|---|---|
| `prenom` | Le prénom affiché en grand sur la première section |
| `peek` | La ligne qui apparaît sur la lettre quand l'enveloppe s'ouvre |
| `paragraphes` | La liste des textes de la lettre (voir la limite ci-dessous) |
| `signature` | Le nom affiché sous la lettre (un tiret est ajouté automatiquement) |
| `question` | La question posée dans la dernière section |
| `whatsappNumber` | Le numéro qui recevra le message |
| `reponses` | Le message prérempli dans WhatsApp, par type de réponse |
| `apresEnvoi` | Le texte affiché sur la page après le clic, par type de réponse |

### Le numéro WhatsApp

Il doit être au **format international, sans `+`, sans espaces, sans zéros au début**.
Exemple : indicatif du pays suivi du numéro, collés (`237` + le numéro pour le Cameroun).

### Le message prérempli

Tu peux écrire librement : accents, emojis et ponctuation sont gérés. Le message est automatiquement encodé pour être accepté dans une URL (`encodeURIComponent`). Tu n'as donc rien à encoder à la main.

### Nombre de paragraphes

La page contient **quatre** sections de lettre dans le HTML, chacune reliée à une entrée de `paragraphes` par son numéro (`data-i="0"`, `"1"`, `"2"`, `"3"`).

- Pour **modifier** un paragraphe : change simplement le texte dans `CONFIG`.
- Pour **en ajouter** un : ajoute le texte dans `CONFIG.paragraphes` **et** duplique une section `letter-section` dans le HTML en incrémentant son `data-i`.
- Pour **en retirer** un : supprime la section correspondante, puis renumérote les `data-i`.

### Ajouter un autre bouton de réponse

Le bouton appelle la fonction `sendAnswer('clé')`. Pour en ajouter un :

1. Ajoute une même clé dans `reponses` et dans `apresEnvoi` (dans `CONFIG`).
2. Ajoute un bouton dans le bloc `proposal-actions` du HTML, qui appelle `sendAnswer` avec cette clé.

---

## Comment modifier l'apparence

Tout se passe dans la balise `<style>`, en haut du fichier.

- **Couleurs** : les variables CSS au début (`--paper`, `--ink`, `--wax`, `--gold`...). Les changer modifie toute la page d'un coup.
- **Polices** : EB Garamond et Dancing Script, chargées depuis Google Fonts dans le `<head>`. Pour en changer, modifie le lien et les `font-family` correspondants.
- **Animations** : les durées et courbes se règlent dans les `transition` de chaque bloc. Elles sont automatiquement désactivées pour les personnes qui ont demandé « réduire les animations » dans leur système.

---

## Comment ça marche techniquement

- **Ouverture de l'enveloppe** : un clic ajoute la classe `open`, et le CSS gère l'animation du rabat, du sceau et de la lettre.
- **Apparition au défilement** : un `IntersectionObserver` ajoute la classe `visible` aux éléments `.reveal` quand ils deviennent visibles.
- **Lien WhatsApp** : la fonction `sendAnswer` construit une URL de la forme `https://wa.me/NUMERO?text=MESSAGE` et l'ouvre dans un nouvel onglet. C'est un « deep link » officiel de WhatsApp, sans serveur ni API. Le message **n'est jamais envoyé automatiquement** : c'est la personne qui appuie sur envoyer.

---

## Déploiement

Le site est statique, donc compatible avec n'importe quel hébergeur gratuit.

**Vercel**
1. Pousse le projet sur GitHub.
2. Sur vercel.com, importe le dépôt.
3. Laisse les réglages par défaut (pas de commande de build, pas de dossier de sortie à définir).
4. Chaque `git push` redéploie automatiquement le site.

GitHub Pages ou Netlify fonctionnent aussi de la même façon.

---

## ⚠️ À savoir sur la confidentialité

Tout ce qui est dans un site web est **public** : n'importe qui peut afficher le code source (Ctrl+U) et voir le numéro WhatsApp et les messages. Si tu publies ce dépôt sur GitHub en public, le numéro sera visible là aussi.

Avant de rendre le dépôt public, pense à :
- mettre le dépôt en **privé** si tu ne veux pas exposer le numéro ;
- ou à utiliser un numéro que tu acceptes de rendre visible.

---

## Structure du projet

```
.
├── index.html   # toute la page : HTML, CSS et JavaScript
└── README.md    # ce fichier
```
