# Proto 2 — La pierre récupérée (juge de paix)

## Question testée (LE pari central du jeu)
**Hériter de la pierre à moitié taillée d'un inconnu, et devoir « faire avec »,
a-t-il un sel que tailler de zéro n'a pas ?**

> ⚠️ Ne coder ce proto QUE si le Proto 1 est concluant. Sinon on teste un étage
> sur des fondations non vérifiées.

---

## Brief pour Claude Code

Reprends **exactement le moteur de taille du Proto 1** (mêmes cubes iso, mêmes
outils, mêmes jauges, même vue fixe iso 2D). On **copie** le code, on ne
factorise pas — vitesse avant élégance.

**Seule différence, et c'est tout le sujet :** la pierre ne démarre pas brute.
Elle démarre **à moitié taillée, comme abandonnée par un autre joueur**,
imparfaitement.

**3 états hérités, codés en dur, sélectionnables au lancement :**
- **« Le propre »** — bien entamé, dans les règles, facile à finir.
- **« Le tordu »** — quelqu'un a trop mangé d'un côté ; la matière manque, il
  faut composer avec le manque et viser un résultat décalé mais valable.
- **« Le bâclé »** — dégrossi n'importe comment, surface en dents de scie, gros
  travail de rattrapage.

**Les cubes déjà retirés par « l'autre » doivent être VISIBLES comme tels** :
les cubes qu'il a touchés / les bords qu'il a laissés sont marqués (teinte ou
hachure différente de mes propres coups), pour que je voie le travail hérité et
m'y adapte.

**Le score récompense le rattrapage :** finir « Le tordu » ou « Le bâclé »
correctement doit rapporter **PLUS** que finir « Le propre » (c'est plus dur).

**Interdits :** identiques au Proto 1 (son, menu, niveaux, sauvegarde,
framework, 3D, rotation).

---

## Le verdict qui décide de TOUT le projet

**« Le tordu » doit être PLUS prenant que « Le propre ».**

- Si OUI → le pari est gagné. L'imperfection héritée crée de la valeur. Cairn a
  un cœur que personne n'a. On peut construire les étages.
- Si NON (« Le propre » plus agréable que « Le tordu ») → l'hypothèse centrale
  du jeu est **fausse**. Il faut la revoir AVANT de construire quoi que ce soit.
  Mieux vaut le découvrir ici, sur un proto moche, que sur un jeu fini.

---

## Verdict (à remplir APRÈS avoir joué)

- « Le tordu » vs « Le propre » : lequel est le plus prenant ? →
- Voir les traces de l'autre change-t-il le ressenti, ou c'est indifférent ? →
- Ressent-on un mini-lien / une adresse à l'inconnu ? (« bon, je récupère ») →
- « Le bâclé » : rattrapage satisfaisant ou juste pénible ? →
- Décision : l'hypothèse tient ? OUI / NON →
- Notes libres :
