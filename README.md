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

## Le pari central (non encore validé)

Toute l'analyse (neuro / socio / game design) converge sur **une seule
inconnue** : est-ce que recevoir et prolonger la pierre d'un inconnu crée un
lien qu'on *ressent* ? Si oui, le jeu tient. Si non, il manque l'essentiel.
C'est ce que testent les protos.

## État d'avancement

- [x] `DESIGN.md` — la vision figée (à lire en premier)
- [x] Proto 1 — **jugé le 29/08/2026 : NON.** Le geste est bon, sa répétition
      ne l'est pas. « Il y a un aspect redondant ; envie d'une 2e pierre : non. »
      Analyse complète dans `proto-01-solo/NOTES.md`.
- [ ] Proto 2 — **bloqué par le protocole** tant que la redondance du Proto 1
      n'est pas comprise. Trois de ses causes sont structurelles, pas des
      réglages : un seul modèle à vie, une seule stratégie gagnante, aucun aval.

## Ordre de travail (à respecter strictement)

1. Poser `DESIGN.md` et ce README.
2. Coder `proto-01-solo/` — ne passer au 2 QUE s'il est concluant.
3. Coder `proto-02-heritage/` — juge de paix : « Le tordu » doit être PLUS
   prenant que « Le propre ».

Les protos sont des **instruments de mesure**, pas des jeux. Chacun répond à
UNE question par oui/non. Tout ce qui n'aide pas à répondre (graphismes, son,
menus, sauvegarde) est banni.
