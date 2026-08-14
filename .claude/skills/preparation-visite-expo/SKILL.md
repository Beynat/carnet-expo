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
- **La liste des œuvres exposées** — chercher en priorité le dossier de presse du musée, souvent en PDF, qui la contient presque toujours. Elle conditionne toute l'étape 4 : sans elle, la fiche n'aura pas d'images.

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

## Étape 4 — Rassembler 5 images d'œuvres

Objectif : garder une trace visuelle de l'exposition. Les règles complètes (sources acceptables, taille, crédit, chemins) sont dans `CLAUDE.md`, section « Images d'œuvres » — les appliquer telles quelles.

### 4.1 — Établir la liste des œuvres exposées

C'est l'étape déterminante, et celle où il est le plus facile de se tromper sans que ça se voie. Une galerie composée des œuvres les plus célèbres de l'artiste paraîtra parfaitement crédible tout en ne montrant rien de ce qui était accroché. Nathan n'aurait aucun moyen de s'en apercevoir.

Chercher la liste des œuvres dans cet ordre, en s'arrêtant dès qu'une source convient :

1. **Le dossier de presse du musée** (souvent un PDF, cherché avec « <titre expo> dossier de presse » ou « press kit ») — c'est la source la plus fiable : il contient presque toujours la liste des œuvres exposées.
2. **La page officielle de l'exposition**, si elle détaille le parcours salle par salle ou cite nommément des œuvres.
3. **Les comptes rendus de presse** qui citent nommément des œuvres vues dans l'exposition.

Aucune autre source n'est acceptable pour établir cette liste. En particulier, ni la connaissance générale de l'artiste, ni une liste de ses œuvres majeures, ni ce qui « devait sûrement » y être.

### 4.2 — Sélectionner 5 œuvres

**Règle de traçabilité, absolue :** chaque œuvre retenue doit être **nommément attestée** dans l'une des sources ci-dessus. Pas d'attestation, pas d'entrée. Ne jamais compléter une liste trop courte avec des œuvres non attestées.

Parmi les œuvres attestées, choisir en :

- **couvrant l'étendue de l'exposition** — périodes, techniques, salles ou sections différentes — plutôt que cinq variations du même motif ;
- **privilégiant les pièces que les sources signalent comme maîtresses** (affiche de l'expo, œuvres mises en avant dans le dossier de presse, œuvres sur lesquelles la critique revient).

Reprendre le **titre exact tel que donné par l'exposition**, et l'année qu'elle indique.

### 4.3 — Récupérer les images

Pour chaque œuvre retenue, trouver une reproduction sur une source ouverte (Wikimedia Commons, open access de musée, site institutionnel), la télécharger, la redimensionner à 800 px de large et l'enregistrer dans `images/<id-de-la-fiche>/01.jpg` à `05.jpg`. Renseigner `file`, `title`, `year` et `credit`.

Distinguer deux échecs différents, et ne jamais confondre l'un avec l'autre :

- **L'œuvre est attestée mais aucune reproduction correcte n'est trouvable** → passer à l'œuvre attestée suivante.
- **Aucune œuvre n'est attestée** (liste introuvable) → ne pas inventer. Mettre moins de 5 entrées, voire aucune, et le dire.

Mieux vaut une fiche avec deux images certaines qu'une fiche avec cinq images plausibles.

### 4.4 — Contrôler avant de committer

Étape non facultative. Une image trop lourde committée par erreur reste **définitivement dans l'historique Git** et alourdit chaque clone, donc chaque session future. L'Action de publication refusera de publier, mais le mal sera déjà fait côté historique. Ce contrôle doit donc passer **avant** `git add` :

```bash
for f in images/<id-de-la-fiche>/*.jpg; do
  taille=$(stat -c%s "$f")
  echo "$f — $((taille/1024)) Ko — $(file -b --mime-type "$f") — $(identify -format '%wx%h' "$f" 2>/dev/null)"
  if [ "$taille" -gt 409600 ]; then echo "  ⚠ TROP LOURDE, redimensionner avant de committer"; fi
done
```

Attendu pour chaque ligne : type `image/jpeg`, largeur 800 px, poids inférieur à 400 Ko.

Ne pas se fier au poids pour juger qu'une image est valide : une œuvre en aplats de couleur (gouache découpée, monochrome, affiche) peut légitimement peser moins de 10 Ko. Le seul critère fiable est le type MIME renvoyé par `file` : s'il indique `text/html`, le téléchargement a récupéré une page d'erreur, pas l'image.

Une image hors normes doit être re-téléchargée ou re-redimensionnée **avant** le commit, jamais corrigée après.

### 4.5 — Rendre compte

Dans la réponse finale, indiquer **d'où vient la liste d'œuvres** (dossier de presse, page officielle, presse) et **combien d'images ont été retenues**. Si la liste n'a pas pu être établie, le dire explicitement : « je n'ai pas trouvé de liste d'œuvres exposées, la fiche n'a pas d'images », ou « je n'ai retenu que les 2 œuvres citées par la presse ».

Cette phrase est ce qui permet à Nathan de distinguer une galerie documentaire d'une galerie approximative. Ne jamais la passer sous silence, ni la noyer dans un compte rendu satisfait.

## Étape 5 — Écrire dans data/fiches.json

1. Lire `data/fiches.json`.
2. Construire l'objet de la fiche selon le schéma de `CLAUDE.md`. Générer l'`id` en slugifiant
   `artiste-lieu-annee` (minuscules, accents retirés, tirets).
3. Si une fiche porte déjà cet `id`, la remplacer entièrement plutôt que d'en ajouter une seconde.
4. Retrier le tableau : `visitDate` décroissant, fiches sans date en dernier par ordre
   alphabétique d'`artist`.
5. Réécrire le fichier en JSON indenté de 2 espaces, accents et caractères non-ASCII conservés tels
   quels (pas d'échappement `\uXXXX`).
6. Vérifier la validité : `python3 -m json.tool data/fiches.json > /dev/null`.

Ne modifier ni `index.html` ni le CSS : ils ne contiennent aucune donnée. Les seuls ajouts hors `data/fiches.json` sont les images sous `images/`.

## Étape 6 — Committer et répondre

Committer avec un message court et explicite, par exemple
`Ajoute la fiche Hilma af Klint (Grand Palais)`.

Puis répondre à Nathan en quelques lignes seulement : quelle exposition a été retenue, la date de
visite enregistrée s'il y en a une, le nombre d'images récupérées, et le fait que la page se
republie automatiquement d'ici une à deux minutes. Ne pas recopier le contenu de la fiche dans la
réponse — elle est consultable dans la bibliothèque.

## Cas particulier — ajouter les images à une fiche existante

Si Nathan demande d'ajouter les images à une fiche déjà en ligne, ne pas refaire toute la
recherche documentaire : lire la fiche dans `data/fiches.json` et appliquer l'étape 4 seule.

Attention : le champ `focus` de la fiche mentionne souvent des œuvres, mais **il ne vaut pas
attestation**. Il a été rédigé lors d'une session précédente et peut lui-même contenir des œuvres
non vérifiées. Refaire la recherche de la liste d'œuvres selon la hiérarchie de sources de
l'étape 4.1, comme pour une fiche neuve.

## Cas particulier — mettre à jour une date de visite

Si Nathan donne ou corrige seulement une date de visite pour une fiche existante, ne pas refaire
la recherche : modifier le champ `visitDate` de la fiche concernée dans `data/fiches.json`,
retrier, vérifier le JSON, committer.
