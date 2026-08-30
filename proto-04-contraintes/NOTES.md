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

### Le modèle : six profondeurs de coupe, une par face

Première version : des colonnes verticales, donc seulement le dessus et le
dessous accessibles. Corrigé — **un tailleur dresse six faces.** La pierre est
maintenant ce qui survit à l'intersection de six coupes, une par face. C'est
exactement la façon dont on équarrit un bloc.

**On ne frappe que le dessus** — la face en l'air. Pour en attaquer une autre,
on **bascule** la pierre. C'est le geste réel, et ça enlève toute ambiguïté :
plus de clic qui part sur un flanc qu'on ne visait pas.

Deux gestes de chevalet, **Tourner** (T) et **Basculer** (B). Basculer change la
face du dessus ; tourner fait pivoter les flancs sans y toucher — il faut les
deux pour atteindre les six faces. La caméra ne bouge jamais.

> Conséquence voulue : tourner la pierre n'est plus une commodité, c'est **le
> seul moyen** d'atteindre les six faces.
La pierre **garde ses rôles** à travers les rotations — le panneau dit en
permanence où sont passés les deux lits et la face de joint, et les lits se
teintent en froid sur la pierre.

> Vérifié : seul le dessus est cliquable (936 points de l'écran, tous sur le
> dessus) ; quatre quarts de tour et quatre basculements ramènent **exactement**
> à l'état de départ ; **les six faces peuvent être amenées en l'air** (marquées
> une à une et retrouvées sur le dessus) ; et les six s'entament réellement une
> fois là-haut.

### L'arasement, et les deux angles morts trouvés au test

Codé avant toute mesure de planéité. Dans le rayon de l'outil : on repère le
point le plus saillant, et chaque colonne perd une part de ce qui dépasse. Une
colonne déjà au fond n'est pas touchée.

**Angle mort n°1 — la règle ne mord pas une surface plane.** Si toute la zone
est de niveau, rien ne « dépasse », donc rien n'est ôté : on ne pourrait même
pas dégrossir un bloc neuf. Deux régimes, donc : arasement proportionnel sur une
surface bosselée, **copeau franc sur toute l'emprise** sur une surface plane —
ce que fait un vrai fer.

**Angle mort n°2 — la coupe restait bloquée au premier cube.** J'ancrais la
profondeur sur la surface *quantifiée au cube* : elle valait 0 tant qu'aucun
cube entier n'était tombé, donc la coupe ne pouvait jamais avancer. Il fallait
une profondeur **continue**. Symptôme : 400 coups de ciseau ne changeaient
strictement rien.

> Après correction : l'écart moyen passe de **0,131 à 0,068** en 400 coups.
> La planéité se gagne, progressivement.

**Et une précaution de mesure** : près des arêtes, une colonne peut avoir été
emportée par la coupe d'une *autre* face. La compter dans la planéité rendait
celle-ci impossible à gagner — le ciseau ne peut rien contre une arête rongée
d'ailleurs. On ne mesure donc que les colonnes qui appartiennent vraiment à la
face. C'est le geste du tailleur : on règle à la règle sur la face, pas sur
l'arête.

### Le scoring : six contraintes, aucune cible

Une contrainte de plus que le brief, rendue possible par les six faces :
**faces de joint d'équerre** — les quatre faces qui ne sont pas des lits doivent
être dressées, c'est par elles que la pierre touche ses voisines.

Le point qui compte, mesuré :

| Pierre | Score |
|---|---|
| mince, matière vers le bas | **94** |
| épaisse, matière vers le bas | **96** |
| mince, matière vers le haut | **94** |

Des pierres franchement différentes, toutes valables. Il n'y a pas UN cube
correct. Le repère (fourchette d'épaisseur) est togglable et **n'entre jamais**
dans le calcul.

### Les outils gardent leur sens

Un tailleur qui tourne sa pierre et surveille ses jauges :

| Étape | Épaisseur | Score |
|---|---|---|
| bloc de carrière | 9,6 | **66** |
| chasse sur les deux lits | 6,4 | **88** |
| pointe | 5,9 | **89** |
| ciseau, puis retourné | 5,0 | **90** |

### Le garde-fou, prouvé et non espéré

Il reste toujours une solution — tailler exactement le profil de la voisine — à
condition qu'il soit atteignable en **enlevant seulement**. Vérifié en continu et
affiché en clair si ça échoue. Les voisines sont engendrées **d'après le bloc qui
vient d'être tiré** : **40 tirages, 40 atteignables**, et une voisine
volontairement impossible est bien détectée.

### Les deux modes de score

Bascule dans le panneau, comme demandé. Le minimum est plus sévère par
construction ; c'est au ressenti de trancher — une des questions du verdict.

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
