# Cairn

> Nom de code. Un *cairn*, c'est un tas de pierres où chaque passant inconnu
> ajoute la sienne. C'est le jeu en un mot. Le vrai nom commercial sera choisi
> quand le jeu aura prouvé qu'il existe — pas avant.

## Le pitch en 5 lignes

Un jeu **coopératif, asynchrone, sans compétition et sans échec possible**, où
des joueurs qui ne se rencontreront jamais taillent des pierres et les empilent
pour faire monter ensemble un **château fort**. Chaque geste est **gravé** :
la matière ôtée ne revient pas. On hérite de la pierre à moitié taillée d'un
inconnu, on « fait avec », on la finit, on la signe, on la passe au métier
suivant. L'objet fini n'a pas d'autre but que d'exister et de porter la trace
de toutes les mains qui l'ont fait.

## Ce que le jeu vole à Pokémon

La **méthode**, pas le contenu (voir `docs/analyse-pokemon.md`) :
1. Un substitut numérique à une expérience réelle que l'époque a fait
   disparaître — ici, la **coopération artisanale**, le chantier commun,
   le compagnonnage.
2. Une boucle de **croissance / collection** — on monte en compétence, le
   château croît, on collectionne les **traces de ses contributions**.
3. **Social par nécessité** — le château ne se bâtit pas seul, l'interdépendance
   est structurelle. C'est l'effet réseau qui a rendu Pokémon pérenne.

## Le pari central — **validé le 29/08/2026**

L'inconnue était : est-ce que recevoir et prolonger la pierre d'un inconnu crée
quelque chose qu'on *ressent* ? Le Proto 2 a répondu : **oui**. Reprendre une
pierre tordue ou bâclée est plus prenant que tailler un bloc neuf.

Restriction : le proto simule l'inconnu par un bot. Le lien avec une **vraie**
personne — le cœur social — reste à tester, et ne peut pas l'être en solo.

## État d'avancement

- [x] `DESIGN.md` — la vision figée (à lire en premier)
- [x] Proto 1 — **CONCLUANT.** Jugé NON le 29/08 (« aspect redondant »), réparé
      par les commandes — chaque pierre demande autre chose, et « à la cote » a
      une tolérance qui dépend de la face — puis **rejugé OUI** le même jour :
      « j'aime beaucoup les challenges d'une pierre à l'autre », et « le frisson
      est présent ». **Les deux questions du juge répondent OUI.**
- [x] Proto 2 — **CONCLUANT. Le pari est gagné.** Jugé le 29/08 : « les coups
      tordus et bâclés sont plus fun ». L'imperfection héritée crée de la
      valeur — le juge de paix du projet est passé.

## Comment lire ce dépôt

| Fichier | Ce qu'il contient |
|---|---|
| `DESIGN.md` | La vision, et **§10 : ce que les protos ont tranché**. À lire en premier. |
| `docs/analyse-pokemon.md` | La boussole : pourquoi ce jeu existe, où est le vrai pari. |
| `proto-01-solo/` | Le geste de taille. `NOTES.md` retrace les 4 refontes et leurs verdicts. |
| `proto-02-heritage/` | La pierre reçue d'un inconnu. `NOTES.md` porte le verdict décisif. |

Les deux protos sont des fichiers HTML autonomes : on les ouvre, rien à
installer.

## Ce qui est prouvé

1. **Tailler est satisfaisant** — à condition qu'il n'existe pas *une seule*
   bonne méthode. La première version du Proto 1 a été jugée redondante ; la
   cause mesurée était une stratégie dominante unique (94 points contre 42 pour
   toutes les autres). Réparé en donnant à chaque pierre une **commande** qui
   dit ce qui compte pour elle.
2. **Le frisson du dernier coup existe** — s'attarder trois secondes de trop
   coûte une trentaine de points, et ça se voit venir coup par coup.
3. **L'imperfection héritée crée de la valeur.** Le juge de paix du projet.
   Reprendre une pierre tordue ou bâclée est *plus* prenant que partir d'un
   bloc neuf.

## Ce qui n'est pas prouvé

1. **Le lien avec une vraie personne.** L'inconnu du Proto 2 est un bot. Toute
   la thèse sociale du `DESIGN.md` §1 — la solidarité d'inconnus — reste
   entièrement à tester, et c'est désormais **la principale inconnue**.
2. **La rétention dans la durée.** Les protos se jugent en une séance. La
   gratitude en aval et la contemplation du château (§5, moteurs 2 et 3) n'ont
   jamais été mises à l'épreuve.
3. **Que ça ressemble à de la taille de pierre.** Le geste *joue* juste ; l'image
   ressemble encore à du minage abstrait. Causes analysées dans `DESIGN.md` §10 —
   la première est la maille de cubes, qui était une facilité de prototype et
   ne doit pas survivre.

## Si on reprend

Trois pistes, par ordre de risque décroissant pour le projet :

1. **Le proto 3 : deux vraies personnes.** C'est là qu'est le risque restant.
   Tout le reste est du confort à côté.
2. **Une étude d'aspect** — relief continu au lieu de cubes, pierre posée sur un
   chevalet, éclats qui s'accumulent. Hors proto : ça ne se mesure pas, ça se
   regarde.
3. **Le son.** Interdit au proto, jamais essayé. Une vingtaine de lignes de
   WebAudio sans aucun fichier, et probablement le meilleur rapport
   effort/ressenti qui reste.

---

*Les protos étaient des **instruments de mesure**, pas des jeux : chacun
répondait à UNE question par oui/non, et tout ce qui n'aidait pas à répondre
était banni. Ils ont répondu. On ne les polit plus.*
