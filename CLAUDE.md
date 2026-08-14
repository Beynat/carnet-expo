# Carnets d'exposition

Bibliothèque personnelle de fiches de préparation de visite d'exposition, appartenant à Nathan.
Publiée en site statique via GitHub Pages (branche `main`, dossier racine) et consultée depuis
une icône sur l'écran d'accueil de son iPhone.

Ce repo est la **source de vérité unique** de la bibliothèque. Il n'y a pas d'autre copie à
synchroniser ailleurs.

## Structure

```
index.html            Affichage seul (HTML/CSS/JS autonome). Charge data/fiches.json via fetch.
data/fiches.json      Toutes les fiches. C'est le seul fichier à modifier pour ajouter du contenu.
images/<id-fiche>/    Les photos d'œuvres de la fiche, une par œuvre (01.jpg, 02.jpg…).
.claude/skills/       Le skill preparation-visite-expo, qui décrit comment produire une fiche.
```

**Règle importante : pour ajouter, modifier ou supprimer une fiche, ne toucher que
`data/fiches.json`.** `index.html` ne contient aucune donnée et n'a pas à être modifié, sauf
demande explicite portant sur l'apparence ou le fonctionnement de la page.

## Schéma d'une fiche

`data/fiches.json` est un tableau JSON d'objets. Chaque objet suit exactement ce schéma :

```json
{
  "id": "prenom-nom-lieu-annee",
  "artist": "Nom de l'artiste",
  "expoTitle": "Titre de l'exposition",
  "venue": "Lieu",
  "expoDates": "Dates de l'exposition, en texte libre",
  "visitDate": "AAAA-MM-JJ, ou chaîne vide si la date de visite n'est pas connue",
  "essential": "Résumé condensé, 5-6 lignes",
  "biography": "Biographie de l'artiste, paragraphes séparés par \n\n",
  "artwork": "Spécificités de l'œuvre",
  "reception": "Critiques, polémiques et hauts faits — au sujet de l'ARTISTE, pas de l'exposition",
  "focus": "Focus sur l'exposition, y compris sa réception critique",
  "vocabulary": [{ "term": "Terme", "def": "Définition" }],
  "sources": [{ "label": "Nom de la source", "url": "https://..." }],
  "artworks": [
    {
      "file": "images/id-de-la-fiche/01.jpg",
      "title": "Titre de l'œuvre",
      "year": "1947",
      "credit": "Source de l'image, et détenteur des droits si connu"
    }
  ]
}
```

Contraintes de contenu :

- `visitDate` est **toujours** au format ISO `AAAA-MM-JJ`, jamais en français. L'affichage se
  charge de la mise en forme. Une date relative donnée par Nathan (« demain », « samedi
  prochain ») doit être résolue en date absolue avant écriture.
- `vocabulary` contient 3 à 4 entrées.
- `sources` ne contient que des sources réellement consultées, avec URL valide.
- Les champs longs (`biography`, `artwork`, `reception`, `focus`) séparent les paragraphes par
  `\n\n` et n'utilisent pas de listes à puces, sauf si le contenu s'y prête vraiment (dans ce cas,
  lignes commençant par `- `).
- Pas de doublon d'`id` : ajouter une fiche dont l'`id` existe déjà signifie remplacer l'existante.
- `artworks` contient 5 entrées quand c'est possible, moins si on ne trouve pas d'images fiables. Le champ peut être absent : l'affichage le gère.

## Images d'œuvres

Objectif : garder une trace visuelle de ce qui a été vu. Les images sont **copiées dans le repo**, jamais liées à un site externe — un lien cassé dans deux ans ferait perdre le souvenir.

Règles de collecte, toutes obligatoires :

- **Des œuvres nommément attestées comme exposées**, par le dossier de presse du musée, la page officielle détaillant le parcours, ou un compte rendu de presse citant l'œuvre. Une œuvre non attestée n'entre pas dans la fiche, même si elle est célèbre et de la bonne période. Mieux vaut deux images certaines que cinq images plausibles.
- **Sources ouvertes en priorité** : Wikimedia Commons, open access des musées (Met, Art Institute, Rijksmuseum, Centre Pompidou…), sites institutionnels. Éviter les banques d'images commerciales et les revendeurs de posters.
- **Vignettes, pas des reproductions** : redimensionner à **800 px de large maximum**, qualité JPEG 82. C'est un carnet documentaire personnel, pas une republication.
- **Crédit systématique** dans le champ `credit` : d'où vient l'image, et le détenteur des droits quand il est identifiable (« Wikimedia Commons, domaine public », « Centre Pompidou / © Succession H. Matisse »).
- **Chemins** : `images/<id-de-la-fiche>/01.jpg`, numérotation à deux chiffres dans l'ordre d'affichage. Toujours du `.jpg`.

Commande de redimensionnement (ImageMagick est disponible dans les sessions cloud) :

```bash
curl -sL "<url>" -o /tmp/src && \
  convert /tmp/src -resize '800x>' -quality 82 images/<id>/01.jpg
```

Vérifier après coup que chaque fichier référencé dans `artworks` existe bien et pèse plus de quelques kilo-octets — une page d'erreur enregistrée en `.jpg` passerait sinon inaperçue.

## Tri

Les fiches sont stockées et affichées de la visite la plus récente/proche à la plus ancienne :
`visitDate` décroissant (comparaison lexicale des chaînes ISO), les fiches sans `visitDate` en
dernier, triées entre elles par `artist` en ordre alphabétique français.

Le tri est appliqué à l'affichage par `index.html`, mais garder le fichier trié de la même façon
rend les diffs plus lisibles.

## Ton et parti pris éditorial

- La bibliothèque est aussi un carnet de souvenirs : elle contient des expositions à venir comme
  des expositions déjà vues ou déjà terminées. **Ne jamais signaler qu'une exposition est
  terminée ou n'est plus visitable**, ni dans `expoDates`, ni dans `essential`, ni ailleurs. Une
  expo passée se traite exactement comme une expo en cours.
- `reception` parle de l'artiste : controverses, polémiques, remises en question rétrospectives,
  faits marquants. Pas de la réception de l'exposition.
- `focus` parle de l'exposition, y compris de son accueil critique.
- Rédaction dense et factuelle : dates précises, noms, anecdotes vérifiables. Pas de généralités.

## Publication

Tout commit sur `main` déclenche une republication GitHub Pages (1 à 2 minutes). Aucune étape
de build : les fichiers sont servis tels quels.

## Vérification avant de conclure

Après modification de `data/fiches.json`, vérifier que le JSON est valide, par exemple :

```bash
python3 -m json.tool data/fiches.json > /dev/null && echo OK
```

Un JSON invalide casse l'affichage de toute la bibliothèque, pas seulement de la fiche concernée.
