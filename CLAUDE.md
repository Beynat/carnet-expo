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

## Images d'œuvres — abandonné

La bibliothèque est **volontairement textuelle**. Ne pas chercher d'images d'œuvres, ne pas créer
de champ `artworks` sur une nouvelle fiche, ne rien ajouter sous `images/`.

Raison : les reproductions librement réutilisables sont introuvables pour la plupart des artistes
exposés, en particulier les artistes vivants, dont les œuvres sont sous droits et absentes des
fonds ouverts. Une tentative menée en août 2026 sur six fiches n'a abouti que sur Matisse, entré
dans le domaine public en 2025, et encore partiellement. Le coût de recherche est réel, le
résultat quasi nul.

Quelques fiches conservent un champ `artworks` hérité de cette tentative. L'affichage le gère et
le champ doit être **préservé tel quel** si l'on réécrit une de ces fiches — mais il n'est plus
alimenté.

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
