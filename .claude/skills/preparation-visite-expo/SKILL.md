---
name: preparation-visite-expo
description: Recherche et rédige une fiche de préparation de visite d'exposition (artiste + expo), puis l'ajoute à data/fiches.json et la committe. À déclencher dès que Nathan donne le nom d'un artiste et/ou d'une exposition et demande de préparer une visite, d'ajouter une fiche, ou de "chercher" sur une expo — même formulé familièrement ("prépare-moi Nan Goldin", "j'ai une expo Soulages à voir", "ajoute Hilma af Klint"). À déclencher aussi quand il donne ou corrige une date de visite sur une fiche existante.
---

## Objectif

Produire une fiche d'exposition documentée et l'écrire directement dans `data/fiches.json`, puis
committer. Le contexte du repo, le schéma d'une fiche, les règles de tri et le parti pris
éditorial sont décrits dans `CLAUDE.md` à la racine — le lire avant de commencer et s'y conformer.

Le livrable n'est pas un message de synthèse : c'est un commit qui modifie `data/fiches.json`.

## Étape 1 — Clarifier, seulement si c'est réellement ambigu

Si Nathan ne donne que l'artiste sans préciser l'exposition (ou l'inverse) et que plusieurs
expositions correspondent, poser une question courte. Sinon, partir sur l'exposition la plus
évidente et le signaler en une phrase dans la réponse finale.

Si Nathan précise une date de visite (« j'y vais samedi », « je l'ai vue le 14 mai »), la résoudre
en date absolue et la renseigner dans `visitDate`.

## Étape 2 — Recherche

Rechercher sur le web et consulter les pages pertinentes pour rassembler :

- La page officielle de l'exposition : lieu, dates, thème, œuvres présentées, commissariat
- La biographie de l'artiste, sur des sources encyclopédiques ou institutionnelles sérieuses
- Le regard critique porté aujourd'hui sur **l'artiste lui-même** : controverses, polémiques,
  faits marquants, remises en question rétrospectives (comportements, propos, positions jugés
  problématiques à l'aune d'aujourd'hui, y compris quand l'artiste reste célébré par ailleurs)
- Des retours de presse et de visiteurs sur **cette exposition précise**, pour nourrir le focus

Toujours vérifier les dates et le lieu exact sur une source de première main, ne pas se fier à la
mémoire. Si l'accès réseau bloque un domaine utile, le signaler à Nathan plutôt que d'inventer :
l'environnement cloud doit être en accès réseau `Full` ou `Custom` pour joindre les sites de
musées.

## Étape 3 — Rédiger les 8 sections

1. **En-tête** : `artist`, `expoTitle`, `venue`, `expoDates`, `visitDate`
2. **`essential`** : résumé condensé de 5-6 lignes — ce qu'il faut savoir avant d'entrer, ce qu'il
   ne faut pas rater
3. **`biography`** : dates et lieux clés, contexte familial, formation, vie privée, inscription
   dans l'histoire de l'art, influences revendiquées, collaborateurs et proches marquants.
   Paragraphes courts et denses, riches en faits précis.
4. **`artwork`** : pourquoi l'artiste est connu, courant artistique, « patte » reconnaissable
   (technique, matériaux, sujets récurrents), œuvres majeures de toute la carrière — pas seulement
   celles de l'exposition
5. **`reception`** : centré sur **l'artiste**. Controverses et polémiques qui l'entourent, faits
   marquants institutionnels, judiciaires ou militants, débats sur la réception de son œuvre à
   travers le temps. Traiter avec nuance et factualité, sans militantisme ni minimisation. La
   réception de l'exposition en cours ne va pas ici.
6. **`focus`** : thème précis de l'exposition, contexte de conception, période couverte, liste
   concrète des œuvres présentées quand l'information est disponible, commissariat, artistes
   contemporains associés à la même scène, **et** l'accueil critique de cette exposition (presse,
   visiteurs, scénographie, affluence)
7. **`vocabulary`** : 3 à 4 termes techniques du champ de l'artiste, définis simplement, sans
   jargon dans la définition
8. **`sources`** : 2 à 5 sources réellement consultées, avec URL

## Étape 4 — Écrire dans data/fiches.json

1. Lire `data/fiches.json`.
2. Construire l'objet de la fiche selon le schéma de `CLAUDE.md`. Générer l'`id` en slugifiant
   `artiste-lieu-annee` (minuscules, accents retirés, tirets).
3. Si une fiche porte déjà cet `id`, la remplacer entièrement plutôt que d'en ajouter une seconde.
4. Retrier le tableau : `visitDate` décroissant, fiches sans date en dernier par ordre
   alphabétique d'`artist`.
5. Réécrire le fichier en JSON indenté de 2 espaces, accents et caractères non-ASCII conservés tels
   quels (pas d'échappement `\uXXXX`).
6. Vérifier la validité : `python3 -m json.tool data/fiches.json > /dev/null`.

Ne modifier aucun autre fichier. `index.html` ne contient pas de données et n'a pas à changer.

## Étape 5 — Committer et répondre

Committer avec un message court et explicite, par exemple
`Ajoute la fiche Hilma af Klint (Grand Palais)`.

Puis répondre à Nathan en quelques lignes seulement : quelle exposition a été retenue, la date de
visite enregistrée s'il y en a une, et le fait que la page se republie automatiquement d'ici une
à deux minutes une fois la modification sur `main`. Ne pas recopier le contenu de la fiche dans la
réponse — elle est consultable dans la bibliothèque.

## Cas particulier — mettre à jour une date de visite

Si Nathan donne ou corrige seulement une date de visite pour une fiche existante, ne pas refaire
la recherche : modifier le champ `visitDate` de la fiche concernée dans `data/fiches.json`,
retrier, vérifier le JSON, committer.
