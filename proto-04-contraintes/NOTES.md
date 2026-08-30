# Proto 4 — Scoring par contraintes (la version JUSTE)

## Pourquoi ce proto existe
Les protos 1-2-3 avaient une **silhouette-cible** à atteindre. « Atteindre un
bloc prédéfini » = UNE seule bonne réponse = le schéma-à-copier qu'on avait
rejeté. Ils testaient donc une version **dégradée** du jeu (bon geste, mais
jugé à la ressemblance).

Ce proto remplace le scoring « distance à une cible » par un scoring
« **satisfaction d'un cahier des charges à solutions multiples** ». C'est la
version juste. Voir DESIGN.md §3bis.

> ⚠️ Ce qui NE change PAS : le geste (tailler = enlever des cubes), le rendu
> (iso 2D façon Age of Empires, vue fixe), les outils, l'idée des jauges.
> SEUL le calcul du score change. Ne pas réécrire le moteur, le faire évoluer.

---

## Question testée
**Est-ce qu'un scoring par contraintes (multitude de solutions valables) donne
un meilleur jeu que le scoring par cible — et est-ce que la "liberté qui se
referme" produit bien une variabilité intéressante ?**

Sous-question critique (le vrai enjeu) : **re-tester le duel tordu/propre dans
cette version juste.** Le Proto 2 a validé le duel en version dégradée — il faut
confirmer qu'il tient quand « faire avec » veut dire *composer avec un choix
légitime* et non *corriger un écart vers une cible commune*.

---

## Spécification fonctionnelle

### Le bloc et le geste (inchangés)
- Pierre = grille de cubes (ex. 10×10×10), rendu iso 2D, vue fixe à 45°.
- 3 outils : Chasse (gros retrait, imprécis) · Pointe (moyen) · Ciseau (1 cube,
  précis). Force du coup par curseur ou durée d'appui.
- Tailler = retirer des cubes. Gravé : on ne remet jamais de cube.

### CE QUI CHANGE — le score n'est plus une distance à une cible
La pierre doit satisfaire un **cahier des charges** = une liste de contraintes
mesurables, chacune notée de 0 à 100 %. PAS de bloc-cible prédéfini.

Contraintes d'une pierre de mur (exemple v1, à coder) :
1. **Face du dessous plane** — mesure la planéité de la face inférieure
   (variance des hauteurs des cubes de cette face). Peu importe OÙ est le plan,
   il faut qu'il y en ait un. 100 % = parfaitement plane.
2. **Face du dessus plane ET parallèle au dessous** — même mesure + parallélisme.
3. **Hauteur dans une fourchette [Hmin..Hmax]** — pas une valeur exacte. Dans la
   fourchette = 100 %. Hors fourchette = chute (trop basse = trop taillé =
   irréversible ; trop haute = pas fini).
4. **Matière portante suffisante** — volume de cubes restant ≥ un seuil.
5. **(si voisine présente) Emboîtement** — la face latérale doit épouser le
   profil de la pierre voisine déjà posée. Mesure l'écart entre les deux
   surfaces. C'est la SEULE contrainte qui dépend d'un autre joueur.

**Score global** = combinaison des contraintes (moyenne pondérée, ou minimum
pour forcer à toutes les respecter — à tester). Afficher chaque contrainte
séparément avec sa couleur (vert/rouge) + le score global.

> Le point capital : plein de blocs de cubes DIFFÉRENTS satisfont ces
> contraintes. Une pierre haute-dans-la-fourchette ET une pierre
> basse-dans-la-fourchette sont TOUTES DEUX valables. Il n'y a pas UN cube
> correct. C'est ça, la « multitude de solutions ».

### La silhouette-guide change de statut
Elle n'est PLUS la réponse notée. C'est un **repère facultatif** (togglable)
qui montre « une zone raisonnable ». Un joueur peut s'en écarter et faire un
MEILLEUR score s'il satisfait mieux les contraintes (ex. mieux épouser la
voisine). Idéalement : bouton pour l'afficher/masquer, et le score ne la
mentionne jamais.

### La liberté qui se referme (à matérialiser)
Pour tester la propriété reine, prévoir un mode « chaîne » :
- Pierre 1 : peu de contraintes actives (pas de voisine) → beaucoup de solutions.
- Chaque pierre validée devient la « voisine » de la suivante → active la
  contrainte d'emboîtement → **restreint** le champ des solutions suivantes.
- Observable : les premières pierres sont libres, les dernières très contraintes.

**Garde-fou OBLIGATOIRE** : le système doit garantir qu'il reste TOUJOURS au
moins une solution valable, même étroite. Une pierre ne doit jamais devenir
mathématiquement impossible à cause de la précédente. Si tu ne peux pas le
garantir formellement en v1, au minimum : détecter le cas « plus de solution »
et le signaler (pour vérifier qu'il n'arrive pas en jeu normal).

### Fin d'une pierre
Bouton « Pierre finie » → fige (gravé) → score global + phrase sur le point
faible (« solide, mais l'emboîtement à droite est juste »).

---

## Ce qu'on observe (à noter dans le verdict)
- Le scoring par contraintes donne-t-il une sensation de LIBERTÉ (plusieurs
  façons de réussir) vs le scoring par cible (une seule bonne réponse) ?
- La liberté se referme-t-elle de façon lisible et intéressante pierre après
  pierre ?
- **Duel tordu/propre re-testé** : hériter d'une pierre voisine « tordue mais
  légitime » et composer avec — est-ce toujours plus fun que « propre » ? Est-ce
  que ça a la bonne saveur (composer, pas corriger) ?
- Danger : la multitude de solutions rend-elle le jeu FLOU / sans but ? (risque
  inverse du schéma). La silhouette-repère suffit-elle à guider sans enfermer ?

---

## Contraintes techniques
- Un seul fichier `index.html`, canvas 2D pur, aucun framework, aucun build.
- Iso 2D vue fixe, pas de 3D, pas de rotation.
- Réutiliser la logique de taille des protos précédents ; ne changer que le
  scoring et l'activation des contraintes.
- Zéro fioriture : pas de son, menu, sauvegarde, graphismes léchés.
- localStorage OK en local (VS Code / navigateur), à éviter si porté dans l'app.

---

## Ce qui a été construit

`index.html` — un seul fichier, canvas 2D, aucune dépendance.

### Le modèle est devenu un modèle de COLONNES

C'est ce qu'exigeait l'arasement, et c'est ce qui rend la planéité mesurable.
Chaque colonne (x,y) est pleine de `bas` à `haut`, en hauteurs **fractionnaires**
— sinon l'arasement se ferait en marches d'escalier au lieu de progresser.

Deux surfaces, donc **deux lits**. D'où un ajout que je signale, parce qu'il
n'était pas au brief : un bouton **« Retourner la pierre »**. Sans lui la face du
dessous est le sol, donc plane d'office, donc la contrainte 1 serait morte. On
retourne l'objet sur le chevalet — la caméra, elle, ne bouge pas.

### L'arasement, et l'angle mort de la règle

Codé avant toute mesure de planéité, comme demandé. Dans le rayon de l'outil :
on repère le point le plus bas de la zone, et chaque colonne perd une part de ce
qui **dépasse** de ce point. Une colonne déjà au fond n'est pas touchée.

> **Mesuré** : une bosse de 3 cubes passe de 9,00 à 6,55 en 12 coups de ciseau,
> et l'écart moyen tombe de 0,059 à 0,011. La planéité se **gagne**.
> La colonne voisine, déjà au niveau, ne bouge pas d'un cheveu.

⚠️ **La règle prise au pied de la lettre a un angle mort**, trouvé au test : si
toute la zone frappée est au même niveau, rien ne « dépasse », donc l'outil ne
mord jamais — on ne pourrait même pas dégrossir un bloc neuf. J'ai donc ajouté
un second régime : **sur une surface déjà de niveau, le fer prend un copeau franc
sur toute son emprise** (c'est ce que fait un vrai fer). Sur une surface
bosselée, arasement proportionnel pur. Les deux régimes, pas un compromis entre
les deux.

### Le scoring : cinq contraintes, aucune cible

Chacune est commentée dans le code. Le point qui compte, mesuré :

| Pierre | Épaisseur | Posée à | Score |
|---|---|---|---|
| mince, posée bas | 5,1 | 0 | **100** |
| épaisse, posée bas | 6,9 | 0 | **100** |
| mince, posée haut | 5,2 | 3 | **100** |

Trois pierres **franchement différentes**, toutes parfaites. Il n'y a pas UN
cube correct. C'est la multitude de solutions, et elle est vérifiée.

Le repère (fourchette d'épaisseur) est togglable et **n'entre jamais dans le
calcul** : on peut s'en écarter et mieux scorer.

### Les outils gardent leur sens dans le monde des contraintes

Un tailleur complet, sur un bloc neuf, contraintes `[lit bas, lit haut,
épaisseur, portance]` :

| Étape | Épaisseur | Contraintes | Score |
|---|---|---|---|
| bloc de carrière | 8,29 | 74 · 43 · 46 · 100 | **63** |
| chasse sur le dessus | 7,44 | 74 · 70 · 82 · 100 | **80** |
| retourné, chasse sur l'autre lit | 6,61 | 95 · 89 · 100 · 100 | **96** |
| pointe, puis ciseau | 6,23 | 99 · 92 · 100 · 100 | **98** |
| retourné, pointe et ciseau | 5,85 | 99 · 98 · 100 · 100 | **99** |

La chasse fait le gros, les fins vont chercher les derniers points, et **il faut
travailler les deux lits** — un bot qui n'en travaille qu'un plafonne à 84.

### Le garde-fou de solvabilité, prouvé et non espéré

Il existe toujours au moins une solution : tailler exactement le profil de la
voisine — **à condition** que ce profil soit atteignable en enlevant seulement
(gravé) et que l'épaisseur qu'il impose tienne dans la fourchette. Les deux sont
vérifiés à chaque instant et affichés en clair si l'un échoue.

Premier test : les voisines tordues étaient **souvent hors d'atteinte** (le
garde-fou les refusait, à raison). Cause : je les fabriquais dans le vide. Elles
sont maintenant engendrées **d'après le bloc qui vient d'être tiré**, dans la
bande qu'il peut encore donner. **40 tirages, 40 atteignables.** Et une voisine
volontairement impossible est bien détectée.

### Les deux modes de score

Bascule dans le panneau. Sur la pierre finie ci-dessus : **minimum 98 / moyenne
99**. Le minimum est plus sévère par construction ; c'est au ressenti de
trancher, et c'est une des questions du verdict.

---

## Verdict (à remplir APRÈS avoir joué)## Verdict (à remplir APRÈS avoir joué)
- Sensation de multitude de solutions vs cible unique ? →
- La liberté se referme-t-elle de façon lisible/intéressante ? →
- Duel tordu/propre en version contraintes : tient-il ? bonne saveur ? →
- Le jeu devient-il flou/sans but ? la silhouette-repère suffit-elle ? →
- Le score par « minimum des contraintes » ou « moyenne pondérée » — lequel est
  le plus juste au ressenti ? →
- Décision : la version contraintes remplace définitivement la version cible ?
  OUI / NON →
- Notes libres :
